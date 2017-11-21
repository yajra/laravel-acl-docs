# Route Middleware


<a name="role"></a>
## role
Check if the user has the given role to access the route.

```php
Route::get('users/index', function() {
	// ...
})->middleware('role:administrator');
```

For multiple roles, use `|` as the delimiter.

```php
Route::get('users/index', function() {
	// ...
})->middleware('role:administrator|cashier');
```

<a name="permission"></a>
## permission
Check if the user has the given permission to access the route.

```php
Route::get('users/index', function() {
	// ...
})->middleware('permission:users.view');
```

<a name="can-at-least"></a>
## canAtLeast
Check if the user has at least one of the given permission(s) to access the route.

> {tip} Use comma separated list for multiple permissions.

Checking multiple permissions on a given route:

```php
Route::get('users/index', function() {
	// ...
})->middleware('canAtLeast:users.view,users.create');
```

