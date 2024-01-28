# Laravel api stater kit

## Installation

you may create a new Laravel project via the Composer create-project command:

```
    composer create-project jeremieflahaut/laravel-api-starter-kit <project name>
```

if you are on Docker you can use official [Composer](https://hub.docker.com/_/composer) Image : 

```
    docker run --rm --interactive --tty --volume $PWD:/app --user $(id -u):$(id -g) \ 
        composer create-project jeremieflahaut/laravel-api-starter-kit <project name>
```

migrations to database (by default it's sqlite see your `.env`)

```
    cd <project name> && php artisan migrate
```

Start Laravel's local development server using the Laravel's Artisan CLI serve command:

```
    php artisan serve
```

## Features

### Github workflow for laravel test

Laravel tests are run in Github runner . The coverage badge will be pushed to a Badges branch. To do this, you must provide read and write access to the workflow.<br>

To do this, go to settings >> Actions >> General >> Workflow permissions in your repository.

[![Tests](https://github.com/jeremieflahaut/laravel-api-starter-kit/actions/workflows/tests.yml/badge.svg)](https://github.com/jeremieflahaut/laravel-api-starter-kit/actions/workflows/tests.yml)
![Coverage](https://github.com/jeremieflahaut/laravel-api-starter-kit/blob/badges/coverage.svg?raw=true&sanitize=true&branch=badges)

### Enforcing Strict Behavior in Development with shouldBeStrict

```php
    Model::shouldBeStrict(! $this->app->isProduction());
```

```php
public static function shouldBeStrict(bool $shouldBeStrict = true)
{
    static::preventLazyLoading($shouldBeStrict);
    static::preventSilentlyDiscardingAttributes($shouldBeStrict);
    static::preventAccessingMissingAttributes($shouldBeStrict);
}
```

In our starter kit, we've adopted the shouldBeStrict method to optimize application performance and enforce better coding practices, especially in a production environment. This method, located in the boot() method of app/Providers/AppServiceProvider.php, is defined as follows:

1. preventLazyLoading($shouldBeStrict)
- Functionality: When set to true, this function throws an exception if lazy loading is attempted on Eloquent models. Lazy loading, while useful in development, can lead to numerous unoptimized database queries in production, impacting performance.
- Benefit: Encourages the use of eager loading strategies, thus reducing database load and enhancing application efficiency.

2. preventSilentlyDiscardingAttributes($shouldBeStrict)
- Functionality: This ensures that any attempt to set attributes that do not exist on the model will throw an exception. Normally, Laravel silently discards such assignments, which can mask typos or logic errors.
- Benefit: Increases code robustness by avoiding silent failures and ensuring data integrity.

3. preventAccessingMissingAttributes($shouldBeStrict)
- Functionality: When enabled, trying to access undefined or non-existent attributes on a model will result in an exception. By default, Laravel returns null for undefined attributes, which can lead to subtle bugs.
- Benefit: Enhances code safety by making attribute access more explicit and preventing unnoticed errors due to undefined attributes.

### Force Https for all generated url in Production

We force https on with the url generator in `boot()` method of `app/Providers/AppServiceProvider.php`.

```php
    if ($this->app->isProduction()) {
        URL::forceScheme('https');
    }
```

### laravel pint
We added a git pre hook before each commit to run laravel pint on the php files that will be in the commit. 
For this we execute this command in `post-autoload-dump` composer script: 

```bash
    [ -d .git ] && git config core.hooksPath .hooks || true
```

We invite you to see the `.hooks/pre-commit` and `pint.json` files for configuring [laravel pint](https://laravel.com/docs/10.x/pint).

## Installed packages
Here are the dependencies added after installing laravel 10.x

- [pestphp/pest](https://pestphp.com/).

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
