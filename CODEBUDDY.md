# CODEBUDDY.md This file provides guidance to CodeBuddy Code when working with code in this repository.

Project overview
- Stack: Laravel 12 (PHP ^8.4) with Fortify, Livewire (Volt, Flux), Vite + TailwindCSS.
- Frontend assets: resources/css/app.css, resources/js/app.js bundled via Vite (vite.config.js:7-18).
- Auth and settings flows are wired via Volt routes (routes/web.php:18-31) and Blade/Livewire views in resources/views/livewire/**.

Common commands
- Install deps:
  - PHP: composer install
  - Node: npm install
- Run full dev environment (Laravel server + queue + logs + Vite):
  - composer run dev
- Run only the Laravel server:
  - php artisan serve
- Run Vite dev server (frontend hot reload):
  - npm run dev
- Build frontend assets for production:
  - npm run build
- Run test suite:
  - composer run test
  - or: php artisan test
- Run a single test:
  - By file: php artisan test tests/Feature/ExampleTest.php
  - By name filter: php artisan test --filter "ExampleTest"
- Code style (Pint):
  - vendor/bin/pint

Testing and quality setup
- Tests use Pest with Laravel plugin. Global setup extends Tests\TestCase, refreshes DB, fakes time/sleep, and scopes to Browser/Feature/Unit (tests/Pest.php:18-28).
- Architectural presets enforced (tests/Unit/ArchTest.php:5-8).

High-level architecture
- HTTP layer and routing:
  - Primary routes and settings navigation defined in routes/web.php (routes/web.php:9-35). Auth-guarded dashboard and settings pages use Volt route helpers.
- Application code:
  - Controllers, models, providers in app/** (e.g., app/Http/Controllers/Auth/VerifyEmailController.php; app/Models/User.php; app/Providers/*).
  - Service providers register app-specific services (e.g., AppServiceProvider, FortifyServiceProvider, VoltServiceProvider).
- Views and Livewire/Volt:
  - Blade layouts/components under resources/views/components/** and page views under resources/views/** (e.g., dashboard.blade.php, livewire/settings/**). Volt routes map to these component/page definitions.
- Assets and tooling:
  - Vite drives asset compilation with laravel-vite-plugin and TailwindCSS (vite.config.js:7-18). Entry points are resources/css/app.css and resources/js/app.js.
- Persistence:
  - Migrations and factories under database/** (e.g., database/migrations/*, database/factories/UserFactory.php). Tests use RefreshDatabase.

Notes
- Use composer run dev for the typical local workflow; it concurrently starts server, queue listener, Laravel logs (pail), and Vite.
- When adding frontend assets, ensure they’re referenced by laravel-vite-plugin inputs (vite.config.js:10).
