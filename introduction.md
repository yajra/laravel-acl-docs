# Introduction

Laravel ACL brings a simple and light-weight role-based permissions system to Laravel's built in Auth system.
This package was based on a great library [Caffeinated/Shinobi](https://github.com/caffeinated/shinobi) and was enhanced to be fully compatible with Laravel's built-in Gate/Authorization system.

The package follows the FIG standards PSR-1, PSR-2, and PSR-4 to ensure a high level of interoperability between shared PHP code.
At the moment the package is not unit tested, but is planned to be covered later down the road.

## Features

Laravel ACL brings support for the following ACL (Access Control List):

- Every user can have zero or more roles.
- Every role can have zero or more permissions.
- Permissions are then inherited to the user through the user's assigned roles.
- User can have zero or more permissions. (Available since v6)