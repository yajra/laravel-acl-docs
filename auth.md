# Authorization

The following methods will become available from your `User` model once you use `HasRole` trait.

<a name="is-role"></a>
## isRole($roleSlug)

Checks if the user is under the given role.

```php
auth()->user()->isRole('administrator');
```

You may also use magic methods:

```php
auth()->user()->isAdministrator();
```

<a name="has-role"></a>
## hasRole($roleSlug)

Check if user has the given role.
A user must have at least one role order for this to return `true`.

```php
auth()->user()->hasRole('administrator');
```

Or pass an array of roles.

```php
auth()->user()->hasRole(['administrator', 'registered']);
```

<a name="can"></a>
## can($permission)

Checks if the user has the given permission.

```php
auth()->user()->can('users.create');
```

<a name="can-at-least"></a>
## canAtLeast([$permissions])

Checks if the user has the given permission(s).
At least one permission must be accountable for in order for this to return `true`.

```php
auth()->user()->canAtLeast(['users.create', 'users.view']);
```

<a name="can-access"></a>
## canAccess([$permissionOrRole])

Checks if the user has the given permission(s) or role(s).
At least one permission or role must be accountable for in order for this to return `true`.

```php
auth()->user()->canAccess(['users.create', 'users.view', 'administrator']);
```
