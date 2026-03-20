---
title: "Blade Directives"
description: "Use Laravel ACL Blade directives for role and permission checks in your views."
---

# Blade Directives

> **Quick Summary**: Laravel ACL registers Blade directives for role and permission checks in views. It supports Laravel's built-in `@can` and `@cannot` directives, plus custom directives.

## Getting Started

Blade directives are automatically registered in the `AclServiceProvider`. You can use them directly in your Blade templates.

## Built-in Directives

Laravel ACL supports Laravel's built-in authorization directives:

<a name="can"></a>

### `@can($permission)`

Display content if the user has the given permission.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$permission` | `string` | Permission slug to check |

**Example:**

```php
@can('posts.update')
    <!-- The Current User Can Update The Post -->
@endcan
```

<a name="cannot"></a>

### `@cannot($permission)`

Display content if the user does NOT have the given permission.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$permission` | `string` | Permission slug to check |

**Example:**

```php
@cannot('posts.update')
    <!-- The Current User Can't Update The Post -->
@endcannot
```

## Custom Directives

Laravel ACL provides additional directives beyond Laravel's built-in ones:

<a name="can-at-least"></a>

### `@canAtLeast($permissions)`

Display content if the user has **at least one** of the given permissions through their roles.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$permissions` | `array<string> \| string` | Permission slug(s) to check |

> [!TIP]
> Use an array to check multiple permissions - returns true if user has **any** of them.

**Example:**

```php
@canAtLeast(['posts.update', 'posts.view'])
    <!-- The Current User Can Update or View The Post -->
@endCanAtLeast
```

<a name="role-directive"></a>

### `@role($roleSlug)`

Display content if the user has the given role.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `$roleSlug` | `string \| array<string>` | Role slug(s) to check |

> [!TIP]
> Use an array to check if user has **any** of the specified roles.

**Examples:**

```php
// Single role check
@role('administrator')
    <!-- The Current User is an Administrator -->
@endRole

// Multiple roles (OR logic)
@role(['administrator', 'moderator'])
    <!-- User is admin OR moderator -->
@endRole
```

## Quick Reference

> [!TIP]
> Bookmark this section for a quick overview!

### Directive Reference

| Directive | Parameter | Description |
|-----------|-----------|-------------|
| `@can` | `string` | Check single permission |
| `@cannot` | `string` | Check if lacks permission |
| `@canAtLeast` | `array\|string` | Check if has any permission |
| `@role` | `array\|string` | Check if has any role |

### Usage Examples

```php
// Permission checks
@can('posts.create')
    <button>Create Post</button>
@endcan

@cannot('posts.delete')
    <p>You cannot delete posts</p>
@endcannot

// At least one permission
@canAtLeast(['posts.update', 'posts.view'])
    <a href="/posts">Manage Posts</a>
@endCanAtLeast

// Role checks
@role('admin')
    <a href="/admin">Admin Panel</a>
@endRole
```
