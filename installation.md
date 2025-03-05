# Installation

- [Installation](#installation)
    - [Installing Laravel ACL](#installing-laravel-acl)
    - [Configuration](#configuration)

<a name="installation"></a>
## Installation

<a name="installing-laravel-acl"></a>
### Installing Laravel ACL

Laravel ACL can be installed with [Composer](http://getcomposer.org/doc/00-intro.md). More details about this package in Composer can be found [here](https://packagist.org/packages/yajra/laravel-acl).

Run the following command in your project to get the latest version of the package:

```
composer require yajra/laravel-acl:"^12.0"
```

<a name="configuration"></a>
## Configuration

### Service Provider
Open the file ```config/app.php``` and then add following service provider.

```php
'providers' => [
    // ...
    Yajra\Acl\AclServiceProvider::class,
],
```

### Route Middleware
Open the `Kernel.php` file in `app\Http\Kernel.php` and add this to used it as middleware :

```php
'canAtLeast' => \Yajra\Acl\Middleware\CanAtLeastMiddleware::class,
'permission' => \Yajra\Acl\Middleware\PermissionMiddleware::class,
'role' => \Yajra\Acl\Middleware\RoleMiddleware::class,
```

### Configuration
> Note: This step is (optional) for the publication of configuration files.

After completing the step above, use the following command to publish configuration settings:

```
php artisan vendor:publish --tag=laravel-acl
```

### Migrations
You'll need to run the provided migrations against your database:

```php
php artisan migrate
```
