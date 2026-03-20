---
title: "Permission Role"
description: "Attach, revoke, and sync roles to users using the InteractsWithRole trait."
---

# Permission Role

> **Quick Summary**: The `InteractsWithRole` trait provides easy-to-use methods to manage and assign roles to any model.

## Getting Started

All methods are available on any model that uses the `InteractsWithRole` trait:

```php
use App\Models\User;
use Yajra\Acl\Traits\InteractsWithRole;

class User extends Model
{
    use InteractsWithRole;
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
if ($user->hasRole('admin')) {
    // User is an admin
}

// Multiple roles (OR logic - true if user has ANY of the roles)
if ($user->hasRole(['admin', 'editor'])) {
    // User is an admin OR editor
}

// Using enums
enum RoleEnum: string
{
    case Admin = 'admin';
    case Editor = 'editor';
}

if ($user->hasRole(RoleEnum::Admin)) {
    // User is an admin
}
```

<a name="getroleslugs"></a>

### `getRoleSlugs()`

Get all role slugs assigned to the user.

**Returns:** Array of role slugs.

**Example:**

```php
$user = User::find(1);

$slugs = $user->getRoleSlugs();
// ['admin', 'editor', 'moderator']
```

## Attaching Roles

<a name="attach-role"></a>

### `attachRole($role, $attributes, $touch)`

Attach a role to the user.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `$role` | `mixed` | — | Role model instance, role slug (string), or enum |
| `$attributes` | `array` | `[]` | Pivot table attributes (e.g., `['expires_at' => now()->addMonth()]`) |
| `$touch` | `bool` | `true` | Whether to touch the model's timestamps |

**Examples:**

```php
$user = User::find(1);

// Attach by role model
$role = Role::find(1);
$user->attachRole($role);

// Attach by slug
$user->attachRole('admin');

// Attach by enum
enum RoleEnum: string
{
    case Admin = 'admin';
}

$user->attachRole(RoleEnum::Admin);

// Attach with pivot attributes
$user->attachRole('admin', ['expires_at' => now()->addMonth()]);
```

<a name="attach-role-slug"></a>

### `attachRoleBySlug($slug)`

Attach a role using only the role slug (alias for `attachRole($slug)`).

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$slug` | `string` | Role slug to attach |

**Example:**

```php
$user = User::find(1);
$user->attachRoleBySlug('admin');
```

## Revoking Roles

<a name="revoke-role"></a>

### `revokeRole($role, $touch)`

Revoke a role from the user.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `$role` | `mixed` | — | Role model instance, role slug (string), or enum |
| `$touch` | `bool` | `true` | Whether to touch the model's timestamps |

> [!NOTE]
> Returns the number of roles detached.

**Examples:**

```php
$user = User::find(1);

// Revoke by role model
$role = Role::find(1);
$detached = $user->revokeRole($role);

// Revoke by slug
$detached = $user->revokeRole('admin');

// Revoke by enum
enum RoleEnum: string
{
    case Admin = 'admin';
}

$detached = $user->revokeRole(RoleEnum::Admin);
```

<a name="revoke-role-slug"></a>

### `revokeRoleBySlug($slug, $touch)`

Revoke a role using the role slug.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `$slug` | `string \| array<string>` | — | Role slug(s) to revoke |
| `$touch` | `bool` | `true` | Whether to touch the model's timestamps |

**Examples:**

```php
$user = User::find(1);

// Revoke a single role
$detached = $user->revokeRoleBySlug('admin');

// Revoke multiple roles at once
$detached = $user->revokeRoleBySlug(['admin', 'editor']);
```

<a name="revoke-all"></a>

### `revokeAllRoles()`

Revoke all roles from the user.

**Returns:** Number of roles detached.

**Example:**

```php
$user = User::find(1);

$detached = $user->revokeAllRoles();
```

## Syncing Roles

<a name="sync-roles"></a>

### `syncRoles($roles, $detaching)`

Sync the user's roles. Replaces all current roles with the given roles.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `$roles` | `Collection \| Model \| array<int>` | — | Role(s) to sync (IDs or model instances) |
| `$detaching` | `bool` | `true` | Whether to detach roles not in the given list |

> [!TIP]
> Returns `['attached' => [], 'detached' => [], 'updated' => []]` with role IDs.

**Examples:**

```php
$user = User::find(1);

// Sync with role IDs
$result = $user->syncRoles([1, 2, 3]);
// ['attached' => [1, 2, 3], 'detached' => [], 'updated' => []]

// Sync with role models
$roles = Role::whereIn('slug', ['admin', 'editor'])->get();
$result = $user->syncRoles($roles);

// Sync without detaching (keeps existing roles)
$result = $user->syncRoles([1, 2], detaching: false);
```

## Query Scopes

The `InteractsWithRole` trait provides query scopes for filtering models by their roles.

<a name="scopehavingroles"></a>

### `scopeHavingRoles($roles)`

Filter records that have the specified roles.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$roles` | `array<int \| string \| BackedEnum \| UnitEnum \| Model>` | Role IDs, slugs, enums, or model instances |

**Examples:**

```php
// Filter by role ID
$users = User::havingRoles([1, 2])->get();

// Filter by role slugs
$users = User::havingRoles(['admin', 'editor'])->get();

// Filter by enums
enum RoleEnum: string
{
    case Admin = 'admin';
    case Editor = 'editor';
}

$users = User::havingRoles([RoleEnum::Admin, RoleEnum::Editor])->get();

// Filter by role models
$roles = Role::where('slug', 'admin')->get();
$users = User::havingRoles($roles)->get();

// Mixed filter
$users = User::havingRoles([
    1,                          // Role ID
    'admin',                   // Role slug
    RoleEnum::Editor,          // Enum
    Role::find(2),             // Role model
])->get();
```

<a name="scopehavingrolesbyslugs"></a>

### `scopeHavingRolesBySlugs($slugs)`

Filter records by role slugs.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$slugs` | `array<string>` | Array of role slugs |

**Example:**

```php
$users = User::havingRolesBySlugs(['admin', 'editor'])->get();
```

## Enum Support

> [!INFO]
> All methods that accept a role parameter support enums out of the box.

### Supported Enum Types

| Enum Type | Value Used | Example |
|-----------|------------|---------|
| `BackedEnum` | `->value` | `RoleEnum::Admin->value` = `'admin'` |
| `UnitEnum` | `->name` | `PermissionLevel::Admin->name` = `'Admin'` |

**Examples:**

```php
// BackedEnum - Uses the enum's value
enum RoleEnum: string
{
    case Admin = 'admin';
    case Editor = 'editor';
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
$user->hasRole(RoleEnum::Admin);           // valid
$user->hasRole(PermissionLevel::Admin);   // valid
$user->attachRole(RoleEnum::Editor);       // valid
$user->revokeRole(PermissionLevel::Write); // valid
```

## Quick Reference

> [!TIP]
> Bookmark this section for a quick overview!

### Method Signatures

```php
// Checking Roles
hasRole(string|array|BackedEnum|UnitEnum $role): bool
getRoleSlugs(): array

// Attaching Roles
attachRole(mixed $role, array $attributes = [], bool $touch = true): void
attachRoleBySlug(string $slug): void

// Revoking Roles
revokeRole(mixed $role, bool $touch = true): int
revokeRoleBySlug(string|array $slug, bool $touch = true): int
revokeAllRoles(): int

// Syncing Roles
syncRoles(Collection|Model|array $roles, bool $detaching = true): array

// Query Scopes
scopeHavingRoles(Builder $query, array $roles): Builder
scopeHavingRolesBySlugs(Builder $query, array $slugs): Builder
```
