title: Notify like a boss with Sweet Alert and Laravel
photo: v1477663916/photo-1464245051818-30da1a636203_z0i8h5.jpg
tags: [php, laravel, package]
---

## Alerts are Front End Logs

Even for the smallest web application it could be useful to display some little
messages to the end user. What comes first in mind is the javascript `alert()`
function. Yes, but it is ugly! What if we need something custom? Something
designed? Something with the ability to display more than just a message?

## SweetAlert at the Rescue

SweetAlert is an easy-to-use library written by [Tristan
Edwards](http://tristanedwards.me/ "Tristan Edwards") that aims to replace
JavaScript's existing "alert" and "prompt" features with better-looking
versions. It automatically centers itself on the page and looks great no matter
if you're using a desktop computer, mobile or tablet.

In a nutshell it can display a basic message, a title with a text under, a
success message, a warning message with a confirm function, a message with a
custom icon, an html message, a message with an auto close timer, a replacement
for the Javascript's "prompt" function, it can run a callback on cancel action
and display a loader for Ajax request,... Customizable any way you wish.

[SweetAlert Documentation](http://t4t5.github.io/sweetalert/ "SweetAlert Documentation")

[SweetAlert on Github](https://github.com/t4t5/sweetalert "SweetAlert on Github")

## SweetAlert and Laravel

Using SweetAlert with Laravel is something easy to implement but we still have
to deal with session (flash) values and to write some blade/html lines to
implement the logic. Tristan Edwards wrote a
[tutorial](https://www.ludu.co/lesson/how-to-use-sweetalert "How to use
SweetAlert") on how to install and use SweetAlert.

## SweetAlert for Laravel

Because Laravel is magic and the community is awesome, someone had to write and
share a package. [Uziel Bueno](https://github.com/uxweb/sweet-alert "SweetAlert
for Laravel") did it and guess what, he did a fantastic work. Using SweetAlert
with Laravel is now a breeze.

## Install the SweetAlert package for Laravel

- Require the package within the `composer.json` file and run the `composer
    update` command.

```php
"require": {
    "uxweb/sweet-alert": "~1.1"
}
```

- Include the service provider within the `app/config/app.php` file.

```php
'providers' => [
    //...
    'UxWeb\SweetAlert\SweetAlertServiceProvider'
];
```

- Include the Facade alias within the `app/config/app.php` file (optional).

```php
'aliases' => [
    //...
    'Alert' => 'UxWeb\SweetAlert\SweetAlert'
];
```

- Download and install the SweetAlert library from your favorite flavor.

From the [website](http://t4t5.github.io/sweetalert/ "SweetAlert website")

From the [source](https://github.com/t4t5/sweetalert "SweetAlert Github")

Or with Bower

```bash
bower install sweetalert
```

## Enable the SweetAlert package for Laravel

Include to your master layout or within any blade template the following lines:

- The `sweetalert.css` file

- The `sweetalert.min.js` file

- The `@include('sweet::alert')` blade include

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <link rel="stylesheet" href="css/sweetalert.css">
</head>
<body>

    <div class="container">
        <p>Content</p>
    </div>

    <script src="js/sweetalert.min.js"></script>

    <!-- Include this after the sweet alert js file -->
    @include('sweet::alert')

</body>
</html>
```

## Use the SweetAlert package for Laravel

SweetAlert for Laravel may be used in two flavors, with the Facade and with a
helper function. In both situations you will have access to the same methods:

- `message($text, $title = '', $type = null)`

Displays a simple alert with a message and an optional title.

- `basic($text, $title)`

Displays a basic alert message with a title.

- `info($text, $title = '')`

Displays an info alert and an optional title.

- `success($text, $title = '')`

Displays a success alert and an optional title.

- `error($text, $title = '')`

Displays an error alert and an optional title.

- `warning($text, $title = '')`

Displays a warning alert and an optional title.

- `autoclose($milliseconds = 1800)`

Sets the delay before autoclosing the alert.
By default, all alerts will dismiss after 1800 milliseconds.

- `persistent($buttonText = 'OK')`

Makes the alert persistent with a confirmation button.

### The Facade

- `Alert::`

```php
public function foo()
{
    //...

    Alert::message('Message', 'Optional Title');
    Alert::basic('Basic Message', 'Mandatory Title');
    Alert::info('Info Message', 'Optional Title');
    Alert::success('Success Message', 'Optional Title');
    Alert::error('Error Message', 'Optional Title');
    Alert::warning('Warning Message', 'Optional Title');

    Alert::basic('Basic Message', 'Mandatory Title')
        ->autoclose(3500);

    Alert::error('Error Message', 'Optional Title')
        ->persistent('Close');

    return redirect('/');
}
```

### The Helper

- `alert($message = null, $title = '')`

In addition to the previous listed methods you can also just use the helper
function without specifying any message type. Doing so is similar to:

- `alert()->message('Message', 'Optional Title')`

Like with the Facade we can use the helper with the same methods:

```php
public function foo()
{
    //...

    alert()->message('Message', 'Optional Title');
    alert()->basic('Basic Message', 'Mandatory Title');
    alert()->info('Info Message', 'Optional Title');
    alert()->success('Success Message', 'Optional Title');
    alert()->error('Error Message', 'Optional Title');
    alert()->warning('Warning Message', 'Optional Title');

    alert()->basic('Basic Message', 'Mandatory Title')
        ->autoclose(3500);

    alert()->error('Error Message', 'Optional Title')
        ->persistent('Close');

    return redirect('/');
}
```

## Customize the SweetAlert package for Laravel

To customize the partial run the following command:

```bash
php artisan vendor:publish --provider='UxWeb\SweetAlert\SweetAlertServiceProvider'
```

The partial is located within the `resources/views/vendor/sweet/` directory.

The complete list of available options can be found in the SweetAlert
[documentation](http://t4t5.github.io/sweetalert/ "SweetAlert documentation").

### Options

However we can use the following configuration options offered by the package
to customize the view:

```php
Session::get('sweet_alert.text')
Session::get('sweet_alert.type')
Session::get('sweet_alert.title')
Session::get('sweet_alert.confirmButtonText')
Session::get('sweet_alert.showConfirmButton')
Session::get('sweet_alert.allowOutsideClick')
Session::get('sweet_alert.timer')
```

### Default View

Here is the default view provided by the package:

```php
@if (Session::has('sweet_alert.alert'))
    <script>
        swal({!! Session::get('sweet_alert.alert') !!});
    </script>
@endif
```

### Custom View

You can completely rewrite and customize the view to your needs by mixing the
built-in and package options.

```php
@if (Session::has('sweet_alert.alert'))
    <script>
        swal({
            text: "{!! Session::get('sweet_alert.text') !!}",
            title: "{!! Session::get('sweet_alert.title') !!}",
            timer: {!! Session::get('sweet_alert.timer') !!},
            type: "{!! Session::get('sweet_alert.type') !!}",
            showConfirmButton: "{!! Session::get('sweet_alert.showConfirmButton') !!}",
            confirmButtonText: "{!! Session::get('sweet_alert.confirmButtonText') !!}",
            confirmButtonColor: "#AEDEF4"

            // more options
        });
    </script>
@endif
```

Note that you must use `""` (double quotes) to wrap the values except for the
`timer` option value.

Happy coding.
