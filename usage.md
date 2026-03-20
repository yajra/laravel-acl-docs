# Usage

<a name="has-role-trait"></a>
## HasRole trait

Laravel ACL package comes bundled with a `HasRole` trait to be used within your `App\Models\User` model.
This trait provides all the necessary functions to tie your users in with roles and permissions.

<a name="example-user-model"></a>
### Example User Model

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Yajra\Acl\Traits\HasRole;

class User extends Authenticatable
{
    use HasFactory;
    use Notifiable;
    use HasRole;

    /**
     * The attributes that are mass assignable.
     *
     * @var list<string>
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * The attributes that should be hidden for serialization.
     *
     * @var list<string>
     */
    protected $hidden = [
        'password',
        'remember_token',
    ];
}
```

<a name="has-role-and-permission-trait"></a>
## HasRoleAndPermission trait

Laravel ACL package comes bundled with a `HasRoleAndPermission` trait that can be used within your `App\Models\User` model.
This trait includes all the abilities from the `HasRole` trait with added functionalities that allow the `User` to have its own custom set of permissions.

<a name="example-user-model-permissions"></a>
### Example User Model

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;
use Yajra\Acl\Traits\HasRoleAndPermission;

class User extends Authenticatable
{
    use HasFactory;
    use Notifiable;
    use HasRoleAndPermission;

    /**
     * The attributes that are mass assignable.
     *
     * @var list<string>
     */
    protected $fillable = [
        'name',
        'email',
        'password',
    ];

    /**
     * The attributes that should be hidden for serialization.
     *
     * @var list<string>
     */
    protected $hidden = [
        'password',
        'remember_token',
    ];
}
```
