# Laravel api stater kit

## Installation

    

## Features

### Github workflow for laravel test

Laravel tests are run in Github runner . The coverage badge will be pushed to a Badges branch. To do this, you must provide read and write access to the workflow.<br>

To do this, go to settings >> Actions >> General >> Workflow permissions in your repository.

[![Tests](https://github.com/jeremieflahaut/laravel-api-starter-kit/actions/workflows/tests.yml/badge.svg)](https://github.com/jeremieflahaut/laravel-api-starter-kit/actions/workflows/tests.yml)
![Coverage](https://github.com/jeremieflahaut/laravel-api-starter-kit/blob/badges/coverage.svg?raw=true&sanitize=true&branch=badges)

### Preventing Lazy Loading in development
In this starter kit, we've included a useful feature to optimize your application's performance when running in a production environment. The line of code `Model::preventLazyLoading(!$this->app->isProduction());` in `boot()` method of `app/Providers/AppServiceProvider.php` is responsible for this behavior.
Lazy loading is a feature in Laravel that loads related data from the database only when it's actually needed. While this can be convenient during development, it can lead to unnecessary database queries and decreased performance in a production environment.
To address this, we've added a conditional check in our models. When your application is in production mode (as determined by `isProduction()`), lazy loading is disabled by calling `preventLazyLoading(true)` on all Eloquent models. This means that related data will be loaded eagerly, reducing the number of database queries and improving overall performance.
You can modify or extend this behavior to suit your specific requirements by adjusting the condition or adding more logic to the `preventLazyLoading` call in your models.
This optimization helps ensure that your Laravel API operates efficiently in a production environment, providing a smoother experience for your users.

```php
    Model::preventLazyLoading(! $this->app->isProduction());
```

### Force Https for all generated url in Production

We force https on with the url generator in `boot()` method of `app/Providers/AppServiceProvider.php`.

```php
    if ($this->app->isProduction()) {
        URL::forceScheme('https');
    }
```

### laravel pint
We added a git pre hook before each commit to run laravel pint on the php files that will be in the commit. 
For this I execute this command : 

```bash
    git config core.hooksPath .hooks in the script
```

We invite you to see the `.hooks/pre-commit` and `pint.json` files for configuring [laravel pint](https://laravel.com/docs/10.x/pint).

## Installed packages
Here are the dependencies added after installing laravel 10.x

- [pestphp/pest](https://pestphp.com/).

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
