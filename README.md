# Custom PHP Framework

A minimal, hand-rolled MVC micro-framework built from scratch in pure PHP — no Composer, no third-party packages, no external dependencies. Created as a learning project to explore how routing, autoloading, and front-controller architecture work under the hood.

The repository contains two iterations of the framework:

| Project | Description |
|---|---|
| [`mvc-project2/`](./mvc-project2) | Current / more complete version — regex-based routing with URL parameters, view rendering, database wrapper scaffold |
| [`mvs-project/`](./mvs-project) | Earlier, simpler version — exact-match routing only, no views or database layer |

## Features

- **Front controller** — every request is routed through a single `index.php` entry point
- **Custom autoloader** — namespace-to-file resolution via `spl_autoload_register`, no Composer required
- **Router** — static route registration (`Route::get()`, `Route::post()`) with `{param}` placeholder support and regex matching (`mvc-project2`)
- **Controllers** — plain PHP classes under the `App\Controllers` namespace
- **Views** — simple PHP template rendering via a global `view()` helper (`mvc-project2`)
- **Database wrapper** — a lightweight PDO/MySQL connection class (`Core\DB`) scaffolded for future use

## Requirements

- PHP 8.0 or higher (uses typed properties and union types)
- Apache with `mod_rewrite` enabled (for clean URLs via `.htaccess`)
- `pdo_mysql` extension if the database layer is used

## Getting Started

1. Clone the repository and point your Apache document root (or a virtual host) at the project you want to run, e.g. `mvc-project2/`.
2. Ensure `mod_rewrite` is enabled so the included `.htaccess` can route all requests through `index.php`.
3. If you plan to use the database layer, update the connection constants in `mvc-project2/app/config.php`:

   ```php
   const DB_HOST = 'localhost';
   const DB_NAME = 'your_database';
   const DB_USERNAME = 'root';
   const DB_PASSWORD = '';
   ```

4. Open the site in your browser — requests are dispatched by `core/App.php` through `core/RouteService.php`.

## Project Structure

```
mvc-project2/
├── .htaccess              # Rewrites all requests to index.php
├── index.php               # Front controller / entry point
├── autoload.php             # Namespace-to-file autoloader
├── app/
│   ├── config.php            # Database connection constants
│   ├── helper.php             # Global view() helper
│   ├── routes.php              # Route definitions
│   └── controllers/             # App\Controllers classes
├── core/
│   ├── App.php                  # Bootstraps config, routes, and helpers
│   ├── DB.php                    # PDO/MySQL connection wrapper
│   └── RouteService.php           # Route registration and dispatch
└── views/                          # PHP view templates
```

## Defining Routes

Routes are registered in `app/routes.php` and support named `{param}` segments that are passed as arguments to the target controller method:

```php
use Core\RouteService as Route;
use App\Controllers\HomeController;

Route::get('sliders/category/{path}/{id}', [HomeController::class, 'about']);
```

Closures are also supported as route handlers.

## Known Limitations

This project is intended for learning and experimentation rather than production use:

- `view()` does not currently extract passed parameters into the template scope, so data passed to views must be accessed differently or the helper extended.
- `Core\DB` is a scaffold — it is not yet wired into any controller and silently swallows connection exceptions.
- Most routes in `mvc-project2/app/routes.php` are commented out and serve as examples.
- No dependency injection container, middleware pipeline, or request/response abstraction — controllers interact with PHP superglobals directly.
- No automated test suite.

## License

No license has been specified for this project.
