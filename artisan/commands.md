# Artisan Commands

## Contents

- [Sessions](#sessions)
- [Migrations](#migrations)
- [Bundles](#bundles)
- [Unit Tests](#unit-tests)
- [Routing](#routing)

<a name="sessions"></a>
## Sessions

Description  | Command
------------- | -------------
Create a session table  | `php artisan session:table`

<a name="migrations"></a>
## Migrations

Description  | Command
------------- | -------------
Create the Laravel migration table  | `php artisan migrate:install`
Creating a migration  | `php artisan migrate:make create_users_table`
Creating a migration for a bundle  |  `php artisan migrate:make bundle::tablename`
Running outstanding migrations  |  `php artisan migrate`
Running outstanding migrations in the application |  `php artisan migrate application`
Running all outstanding migrations in a bundle  |  `php artisan migrate bundle`
Rolling back the last migration operation | `php artisan migrate:rollback`
Roll back all migrations that have ever run  |  `php artisan migrate:reset`

<a name="bundles"></a>
## Bundles

Description  | Command
------------- | -------------
Install a bundle  |  `php artisan bundle:install eloquent`
Upgrade a bundle  |  `php artisan bundle:upgrade eloquent`
Publish a bundles assets  |  `php artisan bundle:publish`


> **Note:** After installing you need to [register the bundle](../bundles/#registering-bundles)

<a name="unit-tests"></a>
## Unit Tests

Description  | Command
------------- | -------------
Running the application tests  |  `php artisan test`
Running the bundle tests  |  `php artisan test bundle-name`

<a name="routing"></a>
### Routing

Description  | Command
------------- | -------------
Calling a route  |  `php artisan route get api/user/1`


> **Note:** You can replace get with post, put, delete, etc.
