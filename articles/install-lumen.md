title: Install Lumen via Installer or Composer
photo: v1477712872/photo-1454779132693-e5cd0a216ed3_qkwcmp.jpg
tags: [lumen, composer, php]
intro: How to install Lumen via the installer or Composer.
---

This article covers how to install Lumen via the installer or Composer.

## Via Lumen Installer

```bash
composer global require "laravel/lumen-installer=~1.0"
```

Make sure to place the `~/.composer/vendor/bin` directory in your PATH so the `lumen` executable can be located by your system.

```bash
lumen new directory
```

The `lumen new directory` command will create a fresh Lumen installation in the directory you specify.

## Via Composer Create-Project

```bash
composer create-project laravel/lumen --prefer-dist directory
```

The `composer create-project` command will create a fresh Lumen installation in the directory you specify.

## Permissions

Lumen may require some permissions to be configured. Folders within `storage` directory need to be writable.

Find the server worker process:

```bash
ps aux|grep nginx|grep -v grep
// probably "www-data"
or:
ps aux|grep apache|grep -v grep
// probably "www-data"
```

Change `storage` folder group:

```bash
[sudo] chgrp -R www-data storage/
```

Set a 775 recursive permission:

```bash
[sudo] chmod -R 775 storage/
```

Source: [Lumen Installation Documentation](http://lumen.laravel.com/docs/installation "Lumen Installation Documentation")

Happy coding.
