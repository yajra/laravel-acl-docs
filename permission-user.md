# User Permissions

User Permissions is an extended version of `HasRole` that allows us to directly add custom permissions to a `User`. To implement this setup, you need to extend `Yajra\Acl\Traits\HasRoleAndPermission` trait on your `User` model.

```php
class User extends Authenticatable
{
    use HasRoleAndPermission;
}

```

- [`getPermissions()`](#get-permissions)
- [`grantPermission($ids, array $attributes = [], $touch = true)`](#grant)
- [`grantPermissionBySlug($slug)`](#grant-slug)
- [`grantPermissionByResource($resource)`](#grant-resource)
- [`revokePermission($permissionId)`](#revoke)
- [`revokeAllPermissions()`](#revoke-all)
- [`syncPermissions([$permissionIds])`](#sync)


<a name="get-permissions"></a>
## getPermissions()
Retrieves an array of assigned permission slugs for the user.

```php
$user = User::find(1);

return $user->getPermissions();
```

<a name="grant"></a>
## grantPermission($ids, array $attributes = [], $touch = true)
Grant the given permission to the user.

```php
$user = User::find(1);
$user->grantPermission(1);

$permissions = Permission::all();
$user->grantPermission($permissions);
```

<a name="grant-slug"></a>
## grantPermissionBySlug($slug)
Grant the given permission slug to the user.

```php
$user = User::find(1);
$user->grantPermissionBySlug('create-post');

$permissions = ['create-post', 'view-post'];
$user->grantPermissionBySlug($permissions);
```

<a name="grant-resource"></a>
## grantPermissionByResource($resource)
Grant the given permission resource to the user.

```php
$user = User::find(1);
$user->grantPermissionByResource('Posts');

$resources = ['Users', 'Posts'];
$user->grantPermissionByResource($resources);
```

<a name="revoke"></a>
## revokePermission($permissionId)
Revokes the given permission from the user.

```php
$user = User::find(1);

$user->revokePermission(1);

```

<a name="revoke-all"></a>
##revokeAllPermissions()
Revokes all permissions from the user.

```php
$user = User::find(1);

$user->revokeAllPermissions();
```

<a name="sync"></a>
## syncPermissions([$permissionIds])
Syncs the given permissions with the user. This will revoke any permissions not supplied.

```php
$user = Role::find(1);

$user->syncPermissions([1, 2, 3]);
```