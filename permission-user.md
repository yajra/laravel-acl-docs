---
title: "User Permissions"
description: "Assign direct permissions to users using the HasRoleAndPermission trait - grant, revoke, and sync user permissions."
---

# User Permissions

> **Quick Summary**: User Permissions extends the `HasRole` trait to allow direct permission assignment on `App\Models\User`. This is useful when you need to grant permissions that are not tied to roles.

## Getting Started

Use the `HasRoleAndPermission` trait to enable direct permission assignment on your User model:

```php
use Illuminate\Foundation\Auth\User as Authenticatable;
use Yajra\Acl\Traits\HasRoleAndPermission;

class User extends Authenticatable
{
    use HasRoleAndPermission;
}
```

## Getting Permissions

<a name="getPermissions"></a>

### `getPermissions()`

Retrieves an array of all permission slugs for the user. This includes both direct user permissions and permissions inherited from assigned roles.

**Returns:** Array of permission slugs.

**Example:**

```php
$user = User::find(1);

$permissions = $user->getPermissions();
// ['users.create', 'users.view', 'posts.delete']
```

## Granting Permissions

<a name="grant"></a>

### `grantPermission($permission, $attributes, $touch)`

Grant the given permission to the user. Accepts a permission model, permission ID, slug string, or enum.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `$permission` | `mixed` | — | Permission model instance, ID, slug string, or enum |
| `$attributes` | `array` | `[]` | Pivot table attributes (e.g., `['expires_at' => '2025-12-31']`) |
| `$touch` | `bool` | `true` | Whether to touch the model's timestamps |

**Examples:**

```php
$user = User::find(1);

// Grant by permission ID
$user->grantPermission(1);

// Grant by slug
$user->grantPermission('create-post');

// Grant with pivot attributes
$user->grantPermission('create-post', ['expires_at' => '2025-12-31']);

// Grant from Permission model
$permission = Permission::find(1);
$user->grantPermission($permission);

// Grant multiple permissions at once
$permissions = Permission::all();
$user->grantPermission($permissions);

// Grant from enum
enum PermissionEnum: string
{
    case CreatePost = 'create-post';
    case ViewPost = 'view-post';
}

$user->grantPermission(PermissionEnum::CreatePost);
```

<a name="grant-slug"></a>

### `grantPermissionBySlug($slug)`

Grant permissions by their slug(s). This finds permissions matching the given slug(s) and attaches them to the user.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$slug` | `string \| array<string>` | Permission slug(s) to grant |

**Examples:**

```php
$user = User::find(1);

// Grant single permission by slug
$user->grantPermissionBySlug('create-post');

// Grant multiple permissions by slug
$user->grantPermissionBySlug(['create-post', 'view-post']);
```

<a name="grant-resource"></a>

### `grantPermissionByResource($resource)`

Grant permissions by their resource name(s). This finds all permissions associated with the given resource(s).

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$resource` | `string \| array<string>` | Resource name(s) to grant permissions for |

**Examples:**

```php
$user = User::find(1);

// Grant all permissions for a single resource
$user->grantPermissionByResource('Posts');

// Grant permissions for multiple resources
$user->grantPermissionByResource(['Users', 'Posts']);
```

## Revoking Permissions

<a name="revoke"></a>

### `revokePermission($permission, $touch)`

Revokes the given permission from the user. Pass `null` to revoke all permissions.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `$permission` | `mixed` | `null` | Permission model, ID, slug, enum, or array |
| `$touch` | `bool` | `true` | Whether to touch the model's timestamps |

> [!NOTE]
> Returns the number of permissions detached.

**Examples:**

```php
$user = User::find(1);

// Revoke single permission by ID
$detached = $user->revokePermission(1);

// Revoke multiple permissions by ID
$detached = $user->revokePermission([1, 2]);

// Revoke by slug
$detached = $user->revokePermission('create-post');

// Revoke by enum
$detached = $user->revokePermission(PermissionEnum::CreatePost);

// Revoke all permissions (pass null)
$detached = $user->revokePermission();
```

<a name="revoke-slug"></a>

### `revokePermissionBySlug($slug)`

Revokes permissions by their slug(s). This finds permissions matching the given slug(s) and detaches them from the user.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$slug` | `string \| array<string>` | Permission slug(s) to revoke |

**Examples:**

```php
$user = User::find(1);

// Revoke single permission by slug
$user->revokePermissionBySlug('create-post');

// Revoke multiple permissions by slug
$user->revokePermissionBySlug(['create-post', 'update-post']);
```

<a name="revoke-permission-by-resource"></a>

### `revokePermissionByResource($resource)`

Revokes permissions by their resource name(s). This finds all permissions associated with the given resource(s) and detaches them.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$resource` | `string \| array<string>` | Resource name(s) to revoke permissions for |

**Examples:**

```php
$user = User::find(1);

// Revoke all permissions for a single resource
$user->revokePermissionByResource('Posts');

// Revoke permissions for multiple resources
$user->revokePermissionByResource(['Users', 'Posts']);
```

<a name="revoke-all"></a>

### `revokeAllPermissions()`

Revokes all direct permissions from the user. Returns the number of permissions detached.

> [!NOTE]
> This only revokes direct user permissions, not permissions inherited from roles.

**Example:**

```php
$user = User::find(1);

$detached = $user->revokeAllPermissions();
```

## Syncing Permissions

<a name="sync"></a>

### `syncPermissions($ids, $detaching)`

Syncs the given permissions with the user. This will detach any permissions not in the supplied list.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `$ids` | `Collection \| Model \| array<int>` | — | Permission ID(s) or model(s) to sync |
| `$detaching` | `bool` | `true` | Whether to detach permissions not in the given list |

> [!TIP]
> Returns `['attached' => [], 'detached' => [], 'updated' => []]` with permission IDs.

**Examples:**

```php
$user = User::find(1);

// Sync single permission
$user->syncPermissions(1);

// Sync multiple permissions
$user->syncPermissions([1, 2, 3]);

// Sync without detaching existing permissions
$user->syncPermissions([1, 2], detaching: false);
```

**Return value contains details about what changed:**

```php
$result = $user->syncPermissions([1, 2, 3]);

// $result = [
//     'attached' => [3],
//     'detached' => [4, 5],
//     'updated' => [1, 2],
// ]
```

## Enum Support

> [!INFO]
> All methods that accept a permission parameter support enums out of the box.

### Supported Enum Types

| Enum Type | Value Used | Example |
|-----------|------------|---------|
| `BackedEnum` | `->value` | `PermissionEnum::CreatePost->value` = `'create-post'` |
| `UnitEnum` | `->name` | `PermissionLevel::Admin->name` = `'Admin'` |

**Examples:**

```php
// BackedEnum - Uses the enum's value
enum PermissionEnum: string
{
    case CreatePost = 'create-post';
    case ViewPost = 'view-post';
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
$user->grantPermission(PermissionEnum::CreatePost);  // valid
$user->grantPermission(PermissionLevel::Write);     // valid
$user->revokePermission(PermissionEnum::ViewPost);  // valid
```

## Quick Reference

> [!TIP]
> Bookmark this section for a quick overview!

### Method Signatures

```php
// Getting Permissions
getPermissions(): array

// Granting Permissions
grantPermission(mixed $permission, array $attributes = [], bool $touch = true): void
grantPermissionBySlug(string|array $slug): void
grantPermissionByResource(string|array $resource): void

// Revoking Permissions
revokePermission(mixed $permission = null, bool $touch = true): int
revokePermissionBySlug(string|array $slug): void
revokePermissionByResource(string|array $resource): void
revokeAllPermissions(): int

// Syncing Permissions
syncPermissions(Collection|Model|array $ids, bool $detaching = true): array
```
