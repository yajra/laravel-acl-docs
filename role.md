# Role Permissions

The bundled `Role` model has easy to use methods to manage and assign permissions.

- [`can($permission)`](#can)
- [`canAtLeast([$permission])`](#can-at-least)
- [`getPermissions()`](#get-permissions)
- [`grantPermission($ids, array $attributes = [], $touch = true)`](#grant)
- [`grantPermissionBySlug($slug)`](#grant-slug)
- [`grantPermissionByResource($resource)`](#grant-resource)
- [`revokePermission($id = null, $touch = true)`](#revoke)
- [`revokePermissionBySlug($slug)`](#revoke-slug)
- [`revokePermissionByResource($resource)`](#revoke-resource)
- [`revokeAllPermissions()`](#revoke-all)
- [`syncPermissions($ids, $detaching = true)`](#sync)

<a name="can"></a>
## can($permission)
Checks if the role has the given permission.

```php
$role = Role::find(1);

return $role->can('users.create');
```

<a name="can-at-least"></a>
## canAtLeast([$permission])
Checks if the role has the given permission(s).
At least one permission must be accounted for in order for this to return `true`.

```php
$role = Role::find(1);

return $role->canAtleast(['users.create', 'users.view']);
```

<a name="get-permissions"></a>
## getPermissions()
Retrieves an array of assigned permission slugs for the role.

```php
$role = Role::find(1);

return $role->getPermissions();
```

<a name="grant"></a>
## grantPermission($ids, array $attributes = [], $touch = true)
Grant the given permission to the role.

```php
$role = Role::find(1);
$role->grantPermission(1);

$permissions = Permission::all();
$role->grantPermission($permissions);
```

<a name="grant-slug"></a>
## grantPermissionBySlug($slug)
Grant the given permission slug to the role.

```php
$role = Role::find(1);
$role->grantPermissionBySlug('create-post');

$permissions = ['create-post', 'view-post'];
$role->grantPermissionBySlug($permissions);
```

<a name="grant-resource"></a>
## grantPermissionByResource($resource)
Grant the given permission resource to the role.

```php
$role = Role::find(1);
$role->grantPermissionByResource('Posts');

$resources = ['Users', 'Posts'];
$role->grantPermissionByResource($resources);
```

<a name="revoke"></a>
## revokePermission($id = null, $touch = true)
Revokes the given permission from the role.

```php
$role = Role::find(1);

$role->revokePermission(1);
$role->revokePermission([1, 2]);

```

<a name="revoke-slug"></a>
## revokePermissionBySlug($slug)
Revokes the given permission slug from the role.

```php
$role = Role::find(1);

$role->revokePermissionBySlug('create-post');
```

<a name="revoke-resource"></a>
## revokePermissionByResource($resource)
Revokes the given permission resource from the role.

```php
$role = Role::find(1);

$role->revokePermissionByResource('Posts');
```

<a name="revoke-all"></a>
##revokeAllPermissions()
Revokes all permissions from the role.

```php
$role = Role::find(1);

$role->revokeAllPermissions();
```

<a name="sync"></a>
## syncPermissions($ids, $detaching = true)
Syncs the given permissions with the role. This will revoke any permissions not supplied.

```php
$role = Role::find(1);

$role->syncPermissions(1);
$role->syncPermissions([1, 2, 3]);
```