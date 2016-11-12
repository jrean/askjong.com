title: Resolve From Laravel Container
tags: [laravel, ioc, php]
---
## The Service Container is the Application

The Laravel service container is a powerful tool for managing class
dependencies and performing dependency injection. Think of it as the
Application itself. The container is a kind of box where we can push things in
(**bind**) and pull things out (**resolve**).

`\Illuminate\Foundation\Application` *extends*
`\Illuminate\Container\Container` and *implements*
`\Illuminate\Contracts\Foundation\Application`.

`\Illuminate\Container\Container` *implements*
`\Illuminate\Contracts\Container\Container`.

## Resolving the Application Instance

There are different ways to **resolve** the `\Illuminate\Foundation\Application` instance.
* The Facade
* The Helper
* The Attribute
* The Dependency Injection

### The Facade

```
var_dump(\App::getInstance());
// or
var_dump(\App::getFacadeApplication());
// or
var_dump(\App::getFacadeRoot());
```

or

```
use App;

var_dump(App::getInstance());
// or
var_dump(App::getFacadeApplication());
// or
var_dump(App::getFacadeRoot());
```

### The Helper

```
var_dump(app());
```

### The Attribute

We may have sometime access out of the box to the `\Illuminate\Foundation\Application` instance.
Inside the `app/Http/routes.php` for instance.

```
Route::get('/test', function() {
	var_dump($this->app);
});
```

Curious to know why? Try the following:

```
Route::get('/test', function() {
	var_dump($this);
});
```

`$this` is an instance of `\App\Providers\RouteServiceProvider` and **extends**
`\Illuminate\Foundation\Support\Providers\RouteServiceProvider` which
**extends** `\Illuminate\Support\ServiceProvider`.
The `\Illuminate\Contracts\Foundation\Application` instance is accessible
through the `$app` attribute.

## Injecting the Application Instance

* `\Illuminate\Contracts\Container\Container`
* `\Illuminate\Contracts\Foundation\Application`
* `\Illuminate\Container\Container`
* `\Illuminate\Foundation\Application`

Type hint any of these **contracts** (interfaces) or **concrete**
implementation classes and Laravel will **resolve** the
`\Illuminate\Foundation\Application` instance out of the container.

The following is a quick example to show how *constructor dependency injection*
and *method dependency injection* work.

Keep in mind when one needs the `\Illuminate\Foundation\Application` instance
it can be resolved with the *helper* or the *facade* call. Nothing wrong with
that and pretty fast.

A more *OOP* approach is to use both:

* Constructor Dependency Injection
* Method Dependency Injection

### Constructor Dependency Injection

The instance is available for the entire class.

Type hint the **contract** (interface)
`\Illuminate\Contracts\Container\Container`.

```
...

use Illuminate\Contracts\Container\Container;

...

protected $app;

public function __construct(Container $app)
{
	$this->app = $app;
}

public function test()
{
	var_dump($this->app);
}
```

or

Type hint the **contract** (interface)
`\Illuminate\Contracts\Foundation\Application`.

```
...

use Illuminate\Contracts\Foundation\Application;

...

protected $app;

public function __construct(Application $app)
{
	$this->app = $app;
}

public function test()
{
	var_dump($this->app);
}
```

or

Type hint the **concrete** implementation
`\Illuminate\Container\Container`.

```
...

use Illuminate\Container\Container;

...

protected $app;

public function __construct(Container $app)
{
	$this->app = $app;
}

public function test()
{
	var_dump($this->app);
}
```

or

Type hint the **concrete** implementation
`\Illuminate\Foundation\Application`.

```
...

use Illuminate\Foundation\Application;

...

protected $app;

public function __construct(Application $app)
{
	$this->app = $app;
}

public function test()
{
	var_dump($this->app);
}
```

### Method Dependency Injection

The instance is available only for that method.

Type hint the **contract** (interface)
`\Illuminate\Contracts\Container\Container`.

```
...

use Illuminate\Contracts\Container\Container;

...

public function test(Container $app)
{
	var_dump($app);
}
```

or

Type hint the **contract** (interface)
`\Illuminate\Contracts\Foundation\Application`.

```
...

use Illuminate\Contracts\Foundation\Application;

...

public function test(Application $app)
{
	var_dump($app);
}
```

or

Type hint the **concrete** implementation
`\Illuminate\Container\Container`.

```
...

use Illuminate\Container\Container;

...

public function test(Container $app)
{
	var_dump($app);
}
```

or

Type hint the **concrete** implementation
`\Illuminate\Foundation\Application`.

```
...

use Illuminate\Foundation\Application;

...

public function test(Application $app)
{
	var_dump($app);
}
```

## Resolving a Service out of the Container

Laravel is a fantastic toolbox and it provides many services out of the box.
To illustrate the next steps we will use the `\Illuminate\Config\Repository`
service. As seen before, there are different ways to resolve something out of
the container.

Lets resolve the `Illuminate\Config\Repository` instance.

### The Facade

```
var_dump(\Config::getFacadeRoot());
```

or

```
use Config;

var_dump(Config::getFacadeRoot());
```

### The Helper

```
var_dump(config());
// or
var_dump(app('config'));
// or
var_dump(app('Illuminate\Config\Repository'));
// or
var_dump(app()->config);
// or
var_dump(app()['config']);
```

* `app('Illuminate\Config\Repository');`

The helper function has resolved an instance of the **concrete** class
`\Illuminate\Config\Repository`.

The `\Illuminate\Container\Container` accepts **bindings**, allowing to bind a
**contract** (an interface) to a **concrete** implementation. We could have
done the following and have resolved the same **concrete** instance of
`\Illuminate\Config\Repository` by type hinting the contract, the interface.

* `var_dump(app('Illuminate\Contracts\Config\Repository'));`

### The Attribute

```
Route::get('/test', function() {
	var_dump($this->app->config);
});
```

## Injecting the Config Instance

* `\Illuminate\Contracts\Config\Repository`
* `\Illuminate\Config\Repository`

### Constructor Dependency Injection

The instance is available for the entire class.

Type hint the **contract** (interface)
`\Illuminate\Contracts\Config\Repository`.

```
...

use Illuminate\Contracts\Config\Repository;

...

protected $config;

public function __construct(Repository $config)
{
	$this->config = $config;
}

public function test()
{
	var_dump($this->config);
}
```

or

Type hint the **concrete** implementation
`\Illuminate\Config\Repository`.

```
...

use Illuminate\Config\Repository;

...

protected $config;

public function __construct(Repository $config)
{
	$this->config = $config;
}

public function test()
{
	var_dump($this->config);
}
```

### Method Dependency Injection

The instance is available only for that method.

Type hint the **contract** (interface)
`\Illuminate\Contracts\Config\Repository`.

```
...

use Illuminate\Contracts\Config\Repository;

...

public function test(Repository $config)
{
	var_dump($config);
}
```

or

Type hint the **concrete** implementation
`\Illuminate\Config\Repository`.

```
...

use Illuminate\Config\Repository;

...

public function test(Repository $config)
{
	var_dump($config);
}
```

## Bonus

We are now able to resolve existing services out of the container. For each
of these we need of course to digg into the [Laravel API Documentation](http://laravel.com/api/5.0/ "The Laravel Api Documentation")
and learn how they work. Here is some ways to interact with the
`\Illuminate\Config\Repository` service.

All these different ways will return the list of all configuration files
available in the `config/` folder.

```
var_dump(Config::all());
// or
var_dump(config()->all());
// or
var_dump(app('config')->all());
// or
var_dump(app('Illuminate\Contracts\Config\Repository')->all());
// or
var_dump(app('Illuminate\Config\Repository')->all());
// or
var_dump(app()->config->all());
// or
var_dump(app()['config']->all());
```

Let's access to the `app.php` configuration file.

```
var_dump(Config::get('app'));
// or
var_dump(config()->get('app'));
// or
var_dump(app('config')->get('app'));
// or
var_dump(app('Illuminate\Contracts\Config\Repository')->get('app'));
// or
var_dump(app('Illuminate\Config\Repository')->get('app'));
// or
var_dump(app()->config->get('app'));
// or
var_dump(app()['config']->get('app'));
// or
var_dump(app()['config']['app']);
```

The magic can go on and on. We can navigate through the array with dot
notation.

```
var_dump(Config::get('app.timezone'));
// or
var_dump(config()->get('app.timezone'));
// or
var_dump(app('config')->get('app.timezone'));
// or
var_dump(app('Illuminate\Contracts\Config\Repository')->get('app.timezone'));
// or
var_dump(app('Illuminate\Config\Repository')->get('app.timezone'));
// or
var_dump(app()->config->get('app.timezone'));
// or
var_dump(app()['config']->get('app.timezone'));
// or
var_dump(app()['config']['app.timezone']);
```

Happy coding.
