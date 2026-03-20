---
title: "Middleware"
description: "Learn how to use Laravel ACL middleware for route-level role and permission checks."
---

# Route Middleware

> **Quick Summary**: Laravel ACL provides middleware for checking user roles and permissions on routes.

## Getting Started

Middleware provides route-level authorization checks. These middleware are automatically registered and available in your routes.

## Role Middleware

<a name="role"></a>

### `role`

Check if the user has the given role to access the route.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$role` | `string` | Role slug(s), use `\|` to check multiple |

> [!TIP]
> Use `|` as a delimiter to check if the user has **any** of the specified roles.

**Examples:**

```php
use Illuminate\Support\Facades\Route;

// Single role check
Route::get('/admin', function () {
    //
})->middleware('role:administrator');

// Multiple roles (OR logic - user needs ANY of these roles)
Route::get('/users', function () {
    //
})->middleware('role:administrator|cashier');
```

## Permission Middleware

<a name="permission"></a>

### `permission`

Check if the user has the given permission to access the route.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$permission` | `string` | Permission slug(s), use `\|` to check multiple |

> [!TIP]
> Use `|` as a delimiter to check if the user has **any** of the specified permissions.

**Examples:**

```php
use Illuminate\Support\Facades\Route;

// Single permission check
Route::get('/users', function () {
    //
})->middleware('permission:users.view');

// Multiple permissions (OR logic - user needs ANY of these permissions)
Route::get('/users', function () {
    //
})->middleware('permission:users.view|users.create');
```

<a name="can-at-least"></a>

### `canAtLeast`

Check if the user has at least one of the given permission(s) to access the route.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$permissions` | `string` | Permission slugs, comma-separated |

> [!NOTE]
> Use comma-separated list for multiple permissions.

**Example:**

```php
use Illuminate\Support\Facades\Route;

// User needs at least one of these permissions
Route::get('/users', function () {
    //
})->middleware('canAtLeast:users.view,users.create');
```

## Using Middleware Groups

> [!INFO]
> You can also use these middleware in route groups for cleaner code.

**Examples:**

```php
use Illuminate\Support\Facades\Route;

// Role-based route group
Route::middleware(['auth', 'role:administrator'])->group(function () {
    Route::get('/admin', function () {
        //
    });
});

// Permission-based route group
Route::middleware(['auth', 'permission:users.view'])->prefix('users')->group(function () {
    Route::get('/', function () {
        //
    });
});
```

## Quick Reference

> [!TIP]
> Bookmark this section for a quick overview!

### Middleware Reference

| Middleware | Parameter | Description |
|------------|-----------|-------------|
| `role` | `string` | Role slug(s), use `\|` for multiple |
| `permission` | `string` | Permission slug(s), use `\|` for multiple |
| `canAtLeast` | `string` | Permission slugs, comma-separated |

### Usage Examples

```php
// Role checks
->middleware('role:admin')
->middleware('role:admin|moderator')

// Permission checks
->middleware('permission:users.view')
->middleware('permission:users.view|users.create')

// Can at least (any permission)
->middleware('canAtLeast:users.view,users.create')
```
