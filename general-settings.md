# General Settings

You can change the `Role` and `Permission` model to be used by the package on this configuration.

The configuration file can be found at `config/acl.php`.

```php
/return [
    /**
     * Role class used for ACL.
     */
    'role'       => \Yajra\Acl\Models\Role::class,

    /**
     * Permission class used for ACL.
     */
    'permission' => \Yajra\Acl\Models\Permission::class,
];
```
