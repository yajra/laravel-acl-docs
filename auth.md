---
title: "Authorization"
description: "Learn how to check user roles and permissions using the HasRole trait methods."
---

# Authorization

> **Quick Summary**: The `HasRole` trait provides methods on your User model to check roles, permissions, and access control.

## Getting Started

The `HasRole` trait adds authorization methods to your User model:

```php
use Illuminate\Foundation\Auth\User as Authenticatable;
use Yajra\Acl\Traits\HasRole;

class User extends Authenticatable
{
    use HasRole;
}
```

## Checking Roles

<a name="hasrole-role"></a>

### `hasRole($role)`

Check if the user has the given role.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$role` | `string \| array<string> \| BackedEnum \| UnitEnum` | Role slug(s) or enum to check |

> [!TIP]
> Returns `true` if the user has **any** of the roles when using an array.

**Examples:**

```php
$user = User::find(1);

// Single role check
if ($user->hasRole('administrator')) {
    // User is an administrator
}

// Multiple roles (OR logic - true if user has ANY of the roles)
if ($user->hasRole(['administrator', 'registered'])) {
    // User is an administrator OR registered
}

// Using enums
enum RoleEnum: string
{
    case Admin = 'administrator';
    case User = 'registered';
}

if ($user->hasRole(RoleEnum::Admin)) {
    // User is an admin
}
```

## Checking Permissions

<a name="canaccess-acl"></a>

### `canAccess($acl)`

Check if the user has the given permission(s) or role(s). This is useful when you want to check for either permissions or roles in a single call.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$acl` | `string \| array<string>` | Permission slug(s) or role slug(s) to check |

> [!TIP]
> Returns `true` if the user has at least one of the specified permissions OR one of the specified roles.

**Example:**

```php
$user = User::find(1);

// Check a single permission
if ($user->canAccess('users.create')) {
    // User can create users
}

// Check multiple (any match)
if ($user->canAccess(['users.create', 'users.view', 'administrator'])) {
    // User has at least one of these
}
```

<a name="can-at-least"></a>

### `canAtLeast($permissions)`

Checks if the user has at least one of the given permission(s) through their assigned roles.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$permissions` | `string \| array<string>` | Permission slug(s) to check |

**Examples:**

```php
$user = User::find(1);

// Single permission check
if ($user->canAtLeast('users.create')) {
    // User can create users
}

// Multiple permissions (OR logic)
if ($user->canAtLeast(['users.create', 'users.view'])) {
    // User has at least one of these permissions
}
```

<a name="get-permissions"></a>

### `getPermissions()`

Get all permission slugs from the user's assigned roles.

**Returns:** Array of permission slugs (both direct and from roles).

**Example:**

```php
$user = User::find(1);

$permissions = $user->getPermissions();
// ['users.create', 'users.view', 'posts.delete']
```

## Enum Support

> [!INFO]
> All methods that accept a role or permission parameter support enums out of the box.

### Supported Enum Types

| Enum Type | Value Used | Example |
|-----------|------------|---------|
| `BackedEnum` | `->value` | `RoleEnum::Admin->value` = `'administrator'` |
| `UnitEnum` | `->name` | `PermissionLevel::Admin->name` = `'Admin'` |

**Examples:**

```php
// BackedEnum - Uses the enum's value
enum RoleEnum: string
{
    case Admin = 'administrator';
    case User = 'registered';
}

// UnitEnum - Uses the enum's name
enum PermissionLevel
{
    case Read;
    case Write;
    case Admin;
}

$user = User::find(1);

// All valid usages:
$user->hasRole(RoleEnum::Admin);                 // valid
$user->hasRole(PermissionLevel::Admin);          // valid
$user->canAccess(PermissionLevel::Write);        // valid
$user->canAtLeast(PermissionLevel::Read);        // valid
```

## Quick Reference

> [!TIP]
> Bookmark this section for a quick overview!

### Method Signatures

```php
// Checking Roles
hasRole(string|array|BackedEnum|UnitEnum $role): bool

// Checking Permissions
canAccess(string|array $acl): bool
canAtLeast(string|array $permissions): bool
getPermissions(): array
```
