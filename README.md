# Laravel Websocket
WebSockets for Laravel

## Installation
Laravel WebSockets can be installed via composer:
```
composer require beyondcode/laravel-websockets
```
The package will automatically register a service provider.

This package comes with a migration to store statistic information while running your WebSocket server. You can publish the migration file using:
```
php artisan vendor:publish --provider="BeyondCode\LaravelWebSockets\WebSocketsServiceProvider" --tag="migrations"
```
