title: Use Sqlite with Homestead
photo: v1477712069/9a2d8cf6_hre8u1.jpg
tags: [laravel, lumen, php, sqlite, homestead]
intro: How to properly use an Sqlite database with Laravel Homestead.
---

This article covers how to properly use an `Sqlite` database with Laravel
Homestead.

Sometime a simple sqlite database suffices to get the job done.
With Laravel, it's very quick and easy to configure connections.
Unfortunately I had a hard time to figure out why the basic settings didn't
work. I was stuck with the following error:

```
SQLSTATE[42S02]: Base table or view not found: 1146 Table 'homestead.users' doesn't exist (SQL: select * from `users` where `email` = email@mail.com limit 1)
```

It was caused by a bad `homestead` configuration...

## Laravel Config

Update the following file:

* `config/database.php`

Edit the `sqlite` array inside the `connections` array and name your database
if you don't want to stick with the default name.

```php
'connections' => [

    'sqlite' => [
        'driver'   => 'sqlite',
        'database' => storage_path().'/:database_name.sqlite',
        'prefix'   => '',
    ],
```

Create a new `.sqlite` database file inside the `storage/` folder.
Assuming you are in the root directory of your project, you can do the
following command:

```bash
touch storage/:database_name.sqlite
```

## Homestead Config

By default `homestead` defines a `homestead` database in its `Homestead.yaml`
configuration file.

```vim
vim ~/.homestead/Homestead.yaml
```

Edit the key `Databases`.

```yaml
databases:
    - :database_name
```

## Reload and Re-Provision Homestead

Prior to get your changes available you must reload `homestead` and
re-provision it.

```bash
vagrant global-status
```

Copy the `id` from the command output then run the following new command:

```bash
vagrant reload :id --provision
```

Happy coding.
