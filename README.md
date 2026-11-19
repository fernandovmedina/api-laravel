# Laravel E-commerce API

A custom JSON API project for learning Laravel and PHP by building a small online store. The roadmap covers product and order endpoints, request validation, middleware, rate limiting, CORS, and customer and admin roles. OAuth 2.0 and JWTs are later learning goals.

## Current status

The repository currently contains a Laravel 13 application with the React and Inertia starter kit. Laravel Fortify handles browser login, registration, password resets, email verification, two-factor authentication, and passkeys. The app has a dashboard, account settings, and a `GET /up` health check.

The store endpoints, role-based authorization, and API token flows are planned. Laravel already includes CORS middleware. The store’s allowed browser origins can be configured when a separate frontend needs access.

## Run locally

Use PHP 8.5 with the PDO SQLite extension, Composer, and Node.js 22.18+ on the 22.x release line with npm. On a fresh clone, run:

```sh
composer install
cp .env.example .env
php artisan key:generate --no-interaction
touch database/database.sqlite
php artisan migrate --no-interaction
npm ci
composer run dev
```

Open [the local app](http://localhost:8000). To run the existing Pest suite, build the frontend assets and run the tests:

```sh
npm run build
php artisan test --compact --no-interaction
```

## How the API fits the project

Keep the existing starter kit and create `routes/api.php` by hand when the first endpoint is ready. Register it with the `api` argument to `withRouting()` in [`bootstrap/app.php`](bootstrap/app.php), then add a `v1` route group for endpoints under `/api/v1`. Laravel applies the `/api` prefix and API middleware group automatically. [Laravel routing](https://laravel.com/docs/13.x/routing#api-routes)

Use the existing `app/Http/Controllers`, `app/Http/Requests`, `app/Models`, and `database/migrations` directories. Add API resources for JSON responses and policies for rules such as who can edit a product or view an order. Put HTTP feature tests in `tests/Feature`.

## Learning roadmap

1. **Product catalog.** Start with `GET /api/v1/products` and `GET /api/v1/products/{product}`. Write the product migration, Eloquent model, controller, JSON resource, and feature tests. Add category filtering and pagination once listing works.
2. **Admin and customer access.** Reuse Fortify’s session login for the starter kit and add product management for admins. Protect those routes with authentication middleware and use policies to decide what each user can do. Set up rate limits and test CORS from a browser on a separate origin.
3. **Orders and inventory.** Let customers place an order by submitting product IDs and quantities. Calculate prices on the server, save the unit price on each order item, and update stock in the same transaction that creates the order. Test insufficient stock and verify that customers can only see their own orders. Use simulated payments for the first version.
4. **API tokens, OAuth 2.0, and JWTs.** Use Sanctum if you need personal access tokens or a separate first-party SPA. Try Passport in a separate exercise where a third-party client requests permission to read a customer’s orders. OAuth 2.0 defines how a client obtains delegated access, while JWT is a signed token format in this exercise. Use a maintained library to study JWT signing, verification, and expiry. [Laravel Sanctum](https://laravel.com/docs/13.x/sanctum) · [Laravel Passport](https://laravel.com/docs/13.x/passport)

Write the routes, validation, authorization rules, and order logic yourself. Test the API with `curl` and Pest before building a storefront.
