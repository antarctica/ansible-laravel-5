# Laravel 5 (`laravel5`)

**Part of the BAS Ansible Role Collection (BARC)**

Configures version 5 of the Laravel framework

## Overview

* Optionally an alias for `php artisan` is added to an app users `.bash_aliases` file, this is enabled by default.

## Availability

This role is designed for internal use but if useful can be shared publicly.

## Usage

### Requirements

#### BAS Ansible Role Collection (BARC)

* `dot-env`

#### Other

##### Laravel

Laravel 5 is required to use this role. For older versions please see the `laravel` role (now deprecated).

* For new projects [use Composer to create a new Laravel application](http://laravel.com/docs/5.0/installation).

* For existing [1] projects you shouldn't need to make any changes. (i.e. where an `app` directory exists and )

[1] You can tell if a project is already using Laravel by checking for `"laravel/framework"` in the project's `composer.json` file and the presence of an `app/` directory both within the path specified by `laravel5_app_root`.

### Variables

* `laravel5_app_user_username`
    * The username for the user which is safe for the application to use and will own the application directory
    * Ideally this user account should be a *standard* user, i.e. without access to `sudo` or similar commands or to sensitive directories such as `/etc/ssl/private`.
    * This variable **MUST** be a valid UNIX user.
    * Default: "app"
* `laravel5_app_user_add_artisan_bash_aliases`
    * Whether or not to add an alias for `php artisan` to the app users `.bash_aliases` file, if used/enabled
    * Default: "true"
* `laravel5_app_root`
    * Path to the directory holding the laravel app (i.e. containing `app/` and `composer.json`)
    * This variable **MUST** point to a valid directory and **MUST NOT** include a trailing `/`.
    * By default this variable will use the `app_root` variable if available. If not, a fall back value will be used.
    * The `app_root` variable **SHOULD** be set within a project, either in a playbook or group/host vars file.
    * Default: "{{ app_root }}" if defined otherwise, "/app"

#### Environment file variables

These variables are used to override the items passed to the `dot-env` role in order to create the Laravel `.env` and `.env.example` files.

By default this role will populate these environment files with the items listed in the environment file shipped with Laravel, though some item values have been changed. The values of these items can be controlled using the variables described in this section. 

To add addition items to these environment files it is recommended to call the `dot-env` in your application playbooks (or roles) with these additional items. This ensure default items are added by this role, allowing you to concentrate only on these additional items.

To remove an item set by default you will need to override the `dot_env_key_value_items` variable.

Note: If you override the `dot_env_key_value_items` variable, you will need to ensure you include any items you wish to keep that are set by this role.

* `laravel5_env_app_env`
    * The name of the environment this Laravel application instance is in
    * See the [Laravel documentation on environment configuration](http://laravel.com/docs/5.0/configuration#environment-configuration) for more information.
    * By default this variable will use the `app_environment` variable if available. If not, a fall back value will be used.
    * The `app_environment` variable **SHOULD** be set within a project, either in a playbook or group/host vars file.
    * Default: "{{ app_environment }}" if defined otherwise, "production"
* `laravel5_env_app_debug`
    * If "true" this Laravel application instance will display addition debugging information which **SHOULD** only be visible in non-production environments
    * See the [Laravel documentation on error handling configuration](http://laravel.com/docs/5.0/errors#configuration) for more information.
    * This is a binary variable and **MUST** be set to either "true" or "false" (without quotes).
    * Default: "false"
* `laravel5_env_app_key`
    * A unique and secret string used for securing user sessions and other encrypted data
    * See the [Laravel documentation on application configuration](http://laravel.com/docs/master#configuration) for more information.
    * By default this variable is set to *static* value that is the same for all instances of this role and therefore insecure. You **SHOULD** change this value to a unique 32 character string.
    * You can use `php /path/to/artisan key:generate` to generate a unique value and update your `.env` file. You **MUST** then override this variable to use the generated value to ensure it is not replaced if this role is re-applied.
    * Default: The sha512 hash of "some-random-string"
* `laravel5_env_db_host`
    * The hostname of the host providing the database of the default database connection for this Laravel application instance
    * See the [Laravel documentation on database connection configuration](http://laravel.com/docs/master/database) for more information.
    * By default this variable will use the `app_database_host` variable if available. If not, a fall back value will be used.
    * The `app_database_host` variable **SHOULD** be set within a project, either in a playbook or group/host vars file.
    * Default: "{{ app_database_host }}" if defined otherwise, "localhost"
* `laravel5_env_db_database`
    * The name of the database for the default database connection for this Laravel application instance
    * See the [Laravel documentation on database connection configuration](http://laravel.com/docs/master/database) for more information.
    * By default this variable will use the `app_database_db` variable if available. If not, a fall back value will be used.
    * The `app_database_db` variable **SHOULD** be set within a project, either in a playbook or group/host vars file.
    * Default: "{{ app_database_db }}" if defined otherwise, "app"
* `laravel5_env_db_username`
    * The username for the user of the database for the default database connection for this Laravel application instance
    * See the [Laravel documentation on database connection configuration](http://laravel.com/docs/master/database) for more information.
    * By default this variable will use the `app_database_username` variable if available. If not, a fall back value will be used.
    * The `app_database_username` variable **SHOULD** be set within a project, either in a playbook or group/host vars file.
    * Default: "{{ app_database_username }}" if defined otherwise, "app"
* `laravel5_env_db_password`
    * The password for the user of the database for the default database connection for this Laravel application instance
    * See the [Laravel documentation on database connection configuration](http://laravel.com/docs/master/database) for more information.
    * Naturally, this variable **SHOULD** be set to something unique and secure.
    * By default this variable will use the `app_database_password` variable if available. If not, a fall back value will be used.
    * The `app_database_password` variable **SHOULD** be set within a project, either in a playbook or group/host vars file.
    * Default: "{{ app_database_password }}" if defined otherwise, "password"

## Contributing

This project welcomes contributions, see `CONTRIBUTING` for our general policy.

## Developing

### Committing changes

The [Git flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow/) workflow is used to manage development of this package.

Discrete changes should be made within *feature* branches, created from and merged back into *develop* (where small one-line changes may be made directly).

When ready to release a set of features/changes create a *release* branch from *develop*, update documentation as required and merge into *master* with a tagged, [semantic version](http://semver.org/) (e.g. `v1.2.3`).

After releases the *master* branch should be merged with *develop* to restart the process. High impact bugs can be addressed in *hotfix* branches, created from and merged into *master* directly (and then into *develop*).

### Issue tracking

Issues, bugs, improvements, questions, suggestions and other tasks related to this package are managed through the BAS Web & Applications Team Jira project ([BASWEB](https://jira.ceh.ac.uk/browse/BASWEB)).

## License

Copyright 2015 NERC BAS. Licensed under the MIT license, see `LICENSE` for details.
