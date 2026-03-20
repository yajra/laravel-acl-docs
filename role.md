---
title: "Role Permissions"
description: "Manage role permissions using the InteractsWithPermission trait - grant, revoke, and sync permissions."
---

# Role Permissions

> **Quick Summary**: The `InteractsWithPermission` trait provides easy-to-use methods to manage and assign permissions to any role.

## Getting Started

All methods are available on any model that uses the `InteractsWithPermission` trait:

```php
use App\Models\Role;
use Yajra\Acl\Traits\InteractsWithPermission;

class Role extends Model
{
    use InteractsWithPermission;
}
```

## Checking Permissions

<a name="can"></a>

### `can($permission)`

Check if the role has the given permission. When passing an array, **ALL** permissions must match (AND logic).

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$permission` | `string \| array<string>` | Permission slug(s) to check |

> [!TIP]
> Returns `true` if the role has **all** of the permissions when using an array (AND logic).

**Examples:**

```php
$role = Role::find(1);

// Single permission check
if ($role->can('users.create')) {
    // Role has this permission
}

// Array means ALL must match (AND logic)
if ($role->can(['users.create', 'users.view'])) {
    // Role has BOTH permissions
}
```

<a name="cannot"></a>

### `cannot($permission)`

Check if the role does **NOT** have the given permission. When passing an array, **ALL** permissions must be absent (AND logic).

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$permission` | `string \| array<string>` | Permission slug(s) to check |

> [!TIP]
> Returns `true` if the role does **not** have **all** of the permissions when using an array.

**Examples:**

```php
$role = Role::find(1);

// Returns true if role does NOT have this permission
if ($role->cannot('users.delete')) {
    // Role lacks this permission
}

// Returns true only if role does NOT have ALL of these permissions
if ($role->cannot(['users.create', 'users.view'])) {
    // Role is missing at least one of these permissions
}
```

<a name="can-at-least"></a>

### `canAtLeast($permission)`

Check if the role has **at least one** of the given permissions (OR logic). Unlike `can()` which requires ALL permissions, this returns `true` if ANY permission in the array matches.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$permission` | `string \| array<string>` | Permission slug(s) to check |

**Example:**

```php
$role = Role::find(1);

// Returns true if role has at least ONE of these permissions
if ($role->canAtLeast(['users.create', 'users.view'])) {
    // Role has at least one of these permissions
}
```

<a name="get-permissions"></a>

### `getPermissions()`

Get all permission slugs assigned to the role.

**Returns:** Array of permission slugs.

**Example:**

```php
$role = Role::find(1);

$permissions = $role->getPermissions();
// ['users.create', 'users.view', 'users.delete']
```

## Granting Permissions

<a name="grant"></a>

### `grantPermission($permission, $attributes, $touch)`

Grant the given permission to the role. Accepts a Permission model, slug string, or enum.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `$permission` | `mixed` | — | Permission model instance, slug (string), or enum |
| `$attributes` | `array` | `[]` | Additional pivot attributes (e.g., `['expires_at' => now()->addMonth()]`) |
| `$touch` | `bool` | `true` | Whether to touch the model's timestamps |

**Examples:**

```php
$role = Role::find(1);

// Grant by model ID
$role->grantPermission(1);

// Grant by Permission model
$permission = Permission::find(1);
$role->grantPermission($permission);

// Grant by slug string
$role->grantPermission('create-post');

// Grant with pivot attributes
$role->grantPermission('create-post', ['expires_at' => now()->addMonth()]);

// Grant from enum
enum PermissionEnum: string
{
    case CreatePost = 'create-post';
}

$role->grantPermission(PermissionEnum::CreatePost);
```

<a name="grant-slug"></a>

### `grantPermissionBySlug($slug)`

Grant permission(s) to the role by slug. Accepts a single slug string or an array of slugs.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$slug` | `string \| array<string>` | Permission slug(s) to grant |

**Examples:**

```php
$role = Role::find(1);

// Grant single permission by slug
$role->grantPermissionBySlug('create-post');

// Grant multiple permissions by slug
$role->grantPermissionBySlug(['create-post', 'view-post', 'edit-post']);
```

<a name="grant-resource"></a>

### `grantPermissionByResource($resource)`

Grant all permissions associated with the given resource(s) to the role.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$resource` | `string \| array<string>` | Resource name(s) to grant permissions for |

**Examples:**

```php
$role = Role::find(1);

// Grant all permissions for a single resource
$role->grantPermissionByResource('Posts');

// Grant all permissions for multiple resources
$role->grantPermissionByResource(['Users', 'Posts']);
```

## Revoking Permissions

<a name="revoke"></a>

### `revokePermission($permission, $touch)`

Revoke the given permission from the role. Accepts a Permission model, slug string, enum, or array of any of these. Pass `null` to revoke all permissions.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `$permission` | `mixed` | `null` | Permission model instance, slug (string), enum, or array |
| `$touch` | `bool` | `true` | Whether to touch the model's timestamps |

> [!NOTE]
> Returns the number of permissions detached.

**Examples:**

```php
$role = Role::find(1);

// Revoke by model ID
$detached = $role->revokePermission(1);

// Revoke by Permission model
$permission = Permission::find(1);
$detached = $role->revokePermission($permission);

// Revoke by slug
$detached = $role->revokePermission('create-post');

// Revoke multiple
$role->revokePermission([1, 2]);
$role->revokePermission(['create-post', 'view-post']);

// Revoke all (equivalent to revokeAllPermissions())
$detached = $role->revokePermission();
```

<a name="revoke-slug"></a>

### `revokePermissionBySlug($slug, $touch)`

Revoke permission(s) from the role by slug. Accepts a single slug string or an array of slugs.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `$slug` | `string \| array<string>` | — | Permission slug(s) to revoke |
| `$touch` | `bool` | `true` | Whether to touch the model's timestamps |

**Examples:**

```php
$role = Role::find(1);

// Revoke single permission by slug
$role->revokePermissionBySlug('create-post');

// Revoke multiple permissions by slug
$role->revokePermissionBySlug(['create-post', 'view-post']);
```

<a name="revoke-resource"></a>

### `revokePermissionByResource($resource, $touch)`

Revoke all permissions associated with the given resource(s) from the role.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `$resource` | `string \| array<string>` | — | Resource name(s) to revoke permissions for |
| `$touch` | `bool` | `true` | Whether to touch the model's timestamps |

**Examples:**

```php
$role = Role::find(1);

// Revoke all permissions for a single resource
$role->revokePermissionByResource('Posts');

// Revoke all permissions for multiple resources
$role->revokePermissionByResource(['Users', 'Posts']);
```

<a name="revoke-all"></a>

### `revokeAllPermissions()`

Revoke all permissions from the role.

**Returns:** Number of permissions detached.

**Example:**

```php
$role = Role::find(1);

$detached = $role->revokeAllPermissions();
```

## Syncing Permissions

<a name="sync"></a>

### `syncPermissions($ids, $detaching)`

Sync the role's permissions. Replaces all current permissions with the given permissions.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `$ids` | `Collection \| Model \| array<int>` | — | Permission ID(s) or Permission model(s) to sync |
| `$detaching` | `bool` | `true` | Whether to detach permissions not in the given list |

> [!TIP]
> Returns `['attached' => [], 'detached' => [], 'updated' => []]` with permission IDs.

**Examples:**

```php
$role = Role::find(1);

// Sync with replacement (revokes any not in list)
$result = $role->syncPermissions([1, 2, 3]);
// ['attached' => [1, 2, 3], 'detached' => [], 'updated' => []]

// Sync without detaching (keeps existing permissions)
$result = $role->syncPermissions([1, 2], detaching: false);

// Sync with Collection
$permissions = Permission::where('resource', 'Posts')->get();
$result = $role->syncPermissions($permissions);

// Sync with single Permission model
$permission = Permission::find(1);
$result = $role->syncPermissions($permission);
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

$role = Role::find(1);

// All valid usages:
$role->can(PermissionEnum::CreatePost);                 // valid
$role->can(PermissionLevel::Admin);                      // valid
$role->grantPermission(PermissionEnum::CreatePost);     // valid
$role->revokePermission(PermissionLevel::Write);       // valid
```

## Quick Reference

> [!TIP]
> Bookmark this section for a quick overview!

### Method Signatures

```php
// Checking Permissions
can(string|array $permission): bool
cannot(string|array $permission): bool
canAtLeast(string|array $permission): bool
getPermissions(): array

// Granting Permissions
grantPermission(mixed $permission, array $attributes = [], bool $touch = true): void
grantPermissionBySlug(string|array $slug): void
grantPermissionByResource(string|array $resource): void

// Revoking Permissions
revokePermission(mixed $permission = null, bool $touch = true): int
revokePermissionBySlug(string|array $slug, bool $touch = true): void
revokePermissionByResource(string|array $resource, bool $touch = true): void
revokeAllPermissions(): int

// Syncing Permissions
syncPermissions(Collection|Model|array $ids, bool $detaching = true): array
```
