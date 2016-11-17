title: Automatically enable HTTPS on your website with EFF's Certbot, deploying Let's Encrypt certificates
photo: v1478940435/photo-1468070454955-c5b6932bd08d_kvvmni.jpg
tags: [security, command-line, nginx, server]
intro: How to automatically enable HTTPS on your website with Let's Encrypt certificates.
---

This article covers how to install, configure and enable HTTPS on a Linux server using
Nginx and Certbot from Let's Encrypt.

## Install Certbot

First thing first, install the `Certbot` package.

```
sudo apt-get install certbot
```

## Update Nginx server block

Open and edit the the targeted Nginx virtual host file for which you want to
enable HTTPS. The files are located in the `/etc/nginx/sites-available`
directory.

Add the following to your server block. If you are blocking the access to dot
files, pay attention to add this **before** the corresponding rule.

```
server {

...

# Allow access to the Let's Encrypt ACME Challenge
location ~ /\.well-known\/acme-challenge {
    allow all;
}

# Prevent hidden files (beginning with a period) from being served
location ~ /\. {
    access_log off;
    log_not_found off;
    deny all;
}

...
```

## Generate a Strong Diffie-Hellman Group

To increase the security, generate a strong Diffie-Hellman 2048-bit group.
It will create a `/etc/ssl/certs/dhparam.pem` file, which is used by the Diffie-Hellman algorithm.

```
[sudo] openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```

## Generate the Let's Encrypt Certificate(s)

You can generate a single certificate for several domains at the same time
(optional).

```
sudo certbot certonly --webroot -w /var/www/example -d example.com -w /var/www/foo -d foo.bar -d www.foo.bar
```

This command will obtain a single certificate for `example.com`, `foo.bar` and
`www.foo.bar`. It will place the files below `/var/www/example` to prove
control of the first domain, and under `/var/www/foo` for the second pair.

The `-w option` indicates the webroot of your project. Usually the path to the
`public` folder where your app entry point is located.

The following files will be generated:

- **The Let's Encrypt private key**
```
/etc/letsencrypt/live/example.com/privkey.pem
```

- **The Let's Encrypt certificate**
```
/etc/letsencrypt/live/example.com/cert.pem
```

- **The Let's Encrypt intermediates certificates**
```
/etc/letsencrypt/live/example.com/chain.pem
```

- **The Let's Encrypt certificate and intermediates certificates**
```
/etc/letsencrypt/live/example.com/fullchain.pem
```

Backup the entire `/etc/letsencrypt` directory to a secure location. In
addition to the certificate, the directory also contains your Let’s Encrypt
account key.

## Create an SSL Configuration Snippet

Create a new file at `/etc/nginx/snippets/ssl-params.conf` and add the
following content.

```
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;
ssl_ecdh_curve secp384r1;
ssl_session_cache shared:SSL:128m;
ssl_session_tickets off;
ssl_stapling on;
ssl_stapling_verify on;
resolver 8.8.8.8 8.8.4.4 valid=300s;
resolver_timeout 5s;
add_header Strict-Transport-Security "max-age=31557600; includeSubDomains";
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-Content-Type-Options "nosniff" always;
add_header X-Xss-Protection "1";

ssl_dhparam /etc/ssl/certs/dhparam.pem;
```

Create a new file at `/etc/nginx/snippets/ssl-example.com.conf` and add the
following content. Replace `example.com` by the desired domain name.

```
ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
ssl_trusted_certificate /etc/letsencrypt/live/example.com/chain.pem
```

## Adjust the Nginx Configuration to Use SSL

Edit the corresponding Nginx virtualhost file(s) in the
`/etc/nginx/sites-available` directory.

Split the configuration in (2) two server blocks. Unencrypted HTTP requests
will be automatically redirected to encrypted HTTPS.

```
# :default_server is optional if not the default virtualhost
server {
    listen 80 [:default_server];
    listen [::]:80 [:default_server];
    server_name example.com www.example.com;
    return 301 https://$server_name$request_uri;
}

server {
    # SSL configuration

    listen 443 ssl http2 default_server;
    listen [::]:443 ssl http2 default_server;
    include snippets/ssl-example.com.conf;
    include snippets/ssl-params.conf;
    . . .
```

Restart Nginx to make the change available.

```
sudo service nginx restart
```

## Automating Renewal

Let's Encrypt certificates last for 90 days, it's highly advisable to create a
cronjob to automatically renew the certificate(s).

To test if the renewal process is working run the following command. It won't
do anything except testing the process.

```
[sudo] certbot renew --dry-run
```

### Add a Cronjob

```
[sudo] crontab -e
```

Then add the following command to your crontab. Adjust the settings to suit
your needs.

```
# Every day at 4:30 am
30 4 * * * sudo certbot renew --quiet --post-hook "sudo service nginx reload" >> /var/log/le-renew.log
```

Happy coding.
