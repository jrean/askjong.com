title: Use Sqlite with Homestead
tags: [laravel, lumen, php, sqlite, homestead]
---
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

```
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

`touch storage/:database_name.sqlite`

## Homestead Config

By default `homestead` defines a `homestead` database in its `Homestead.yaml`
configuration file.

```
vim ~/.homestead/Homestead.yaml
```

Edit the key `Databases`.

```
databases:
    - :database_name 
```

## Reload and Re-Provision Homestead

Prior to get your changes available you must reload `homestead` and
re-provision it.

```
vagrant global-status
```

Copy the `id` from the command output then run the following new command:

```
vagrant reload :id --provision
```

You're done! Happy Codding.
