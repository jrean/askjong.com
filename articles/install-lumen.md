title: Install Lumen via Installer or Composer
tags: [lumen, composer, php]
---
## Via Lumen Installer

    composer global require "laravel/lumen-installer=~1.0"

> Make sure to place the `~/.composer/vendor/bin` directory in your PATH so the `lumen` executable can be located by your system.

    lumen new directory

> The `lumen new directory` command will create a fresh Lumen installation in the directory you specify.

## Via Composer Create-Project

    composer create-project laravel/lumen --prefer-dist directory

> The `composer create-project` command will create a fresh Lumen installation in the directory you specify.

## Permissions

> Lumen may require some permissions to be configured. Folders within `storage` directory need to be writable.

Find the server worker process:

    ps aux|grep nginx|grep -v grep
    // probably "www-data"
    or:
    ps aux|grep apache|grep -v grep
    // probably "www-data"

Change `storage` folder group:

    [sudo] chgrp -R www-data storage/

Set a 775 recursive permission:

    [sudo] chmod -R 775 storage/

Source: [Lumen Installation Documentation](http://lumen.laravel.com/docs/installation "Lumen Installation Documentation")
