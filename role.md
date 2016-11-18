# Role Permissions

The bundled `Role` model has easy to use methods to manage and assign permissions.

- [`can($permission)`](#can)
- [`canAtLeast([$permission])`](#can-at-least)
- [`getPermissions()`](#get-permissions)
- [`assignPermission($permissionId)`](#assign)
- [`revokePermission($permissionId)`](#revoke)
- [`revokeAllPermissions()`](#revoke-all)
- [`syncPermissions([$permissionIds])`](#sync)

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

<a name="assign"></a>
## assignPermission($permissionId)
Assigns the given permission to the role.

```php
$role = Role::find(1);

$role->assignPermission(1);

$role->save();
```

<a name="revoke"></a>
## revokePermission($permissionId)
Revokes the given permission from the role.

```php
$role = Role::find(1);

$role->revokePermission(1);

$role->save();
```

<a name="revoke-all"></a>
##revokeAllPermissions()
Revokes all permissions from the role.

```php
$role = Role::find(1);

$role->revokeAllPermissions();

$role->save();
```

<a name="sync"></a>
## syncPermissions([$permissionIds])
Syncs the given permissions with the role. This will revoke any permissions not supplied.

```php
$role = Role::find(1);

$role->syncPermissions([1, 2, 3]);

$role->save();
```