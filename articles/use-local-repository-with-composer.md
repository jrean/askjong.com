title: Use Local Repository with Composer
tags: [vcs, git, composer, php]
-------
> When developping composer packages it can be painfull to commit and push (to
github for instance) for each change we have done then update composer for the
project we are requiring that package. It is even worth when we just want to test
the package. A good solution is to require the package as a local repository.

Update your `composer.json` with the following:

    "repositories": [
        {
            "type": "vcs",
            "url": "/your/local/repository/acme/package/"
        }
    ],

> Update composer to pull from your local repository.

    composer update
    // or
    composer update acme/package

> It is still time consuming to `composer update` each time a change is done.
We can use a symbolic link to save some time.

Inside the project `vendor/` directory of the project requiring your package,
remove the package directory if it already exists and create a symbolic link instead:

    cd project/vendor/
    rm -rf acme/
    mkdir acme
    cd acme
    ln -s /your/local/repository/acme/package /path/to/project/vendor/acme/package

> Update your package source code and it will be available inside your project
without any aditional steps. Of course this is for developping and testing
only. Beware, as soon as you will run a new `composer update` command it may
lead to some troubles. The best is to remove the symbolic link before, then run
the `composer update` command and re-create the symbolic link just after. Not
that bad!

## Bonus

> As I do you probably use a [Vagrant](https://www.vagrantup.com/ "Vagrant")
virtual machine? Maybe [Laravel Homestead](https://github.com/laravel/homestead
"Laravel Homestead on Github")? Here is a little trick to not stay stuck. Put
your packages inside your `shared folder` because it won't like symbolic links
outside of it...

