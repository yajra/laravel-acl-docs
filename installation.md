---
title: "Installation"
description: "Install and configure Laravel ACL package with Composer and publish configuration files."
---

# Installation

Laravel ACL can be installed with [Composer](http://getcomposer.org/doc/00-intro.md). More details about this package in Composer can be found [here](https://packagist.org/packages/yajra/laravel-acl).

Run the following command in your project to get the latest version of the package:

```bash
composer require yajra/laravel-acl:"^13.0"
```

> **Note**: Laravel 13 auto-discovers service providers, so no manual registration is required.

## Configuration

### Publishing Configuration

To publish the configuration file, run the following command:

```bash
php artisan vendor:publish --tag=laravel-acl
```

This will publish the `config/acl.php` configuration file to your application.

### Middleware Registration

Laravel 13 uses `bootstrap/app.php` for middleware registration instead of `Kernel.php`. Add the middleware aliases to your `bootstrap/app.php` file:

```php
<?php

use Illuminate\Foundation\Application;
use Illuminate\Foundation\Configuration\Exceptions;
use Illuminate\Foundation\Configuration\Middleware;

return Application::configure(basePath: dirname(__DIR__))
    ->withRouting(
        web: __DIR__.'/../routes/web.php',
        api: __DIR__.'/../routes/api.php',
        commands: __DIR__.'/../routes/console.php',
        health: '/up',
    )
    ->withMiddleware(function (Middleware $middleware) {
        $middleware->alias([
            'role' => \Yajra\Acl\Middleware\RoleMiddleware::class,
            'permission' => \Yajra\Acl\Middleware\PermissionMiddleware::class,
            'canAtLeast' => \Yajra\Acl\Middleware\CanAtLeastMiddleware::class,
        ]);
    })
    ->withExceptions(function (Exceptions $exceptions) {
        //
    })->create();
```

### Migrations

Migrations are automatically loaded from the package. Run the following command to create the required tables:

```bash
php artisan migrate
```
