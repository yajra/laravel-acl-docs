# Blade Directives

Laravel ACL supports the built-in function of Laravel's [`Authorization`](https://laravel.com/docs/authorization) directives like `@can` and `@cannot`.

<a name="can"></a>
## @can($permission)

```php
@can('posts.update')
    <!-- The Current User Can Update The Post -->
@endcan
```

<a name="cannot"></a>
## @cannot($permission)

```php
@cannot('posts.update')
    <!-- The Current User Can't Update The Post -->
@endcannot
```

## Additional Directives

In addition to the built-in directives, Laravel ACL provides `@canAtLeast`,  `@isRole` and `@hasRole` directives:

<a name="can-at-least"></a>
## @canAtLeast([$permissions])

```php
@canAtLeast(['posts.update', 'posts.view'])
    <!-- The Current User Can Update or View The Post -->
@endCanAtLeast
```

<a name="is-role"></a>
## @role($roleSlug)

```php
@role('administrator')
    <!-- The Current User is an Administrator -->
@endRole
```

<a name="has-role"></a>
## @hasRole($roleSlug)

```php
@hasRole('administrator')
    <!-- The Current User have an administrator role -->
@endHasRole
```