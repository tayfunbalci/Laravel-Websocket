<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"></a></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

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

Run the migrations with:
```
php artisan migrate
```

Next, you need to publish the WebSocket configuration file:
```
php artisan vendor:publish --provider="BeyondCode\LaravelWebSockets\WebSocketsServiceProvider" --tag="config"
```

## Pusher Replacement
### Requirements
To make use of the Laravel WebSockets package in combination with Pusher, you first need to install the official Pusher PHP SDK.

If you are not yet familiar with the concept of Broadcasting in Laravel, please take a look at the [Laravel documentation](https://laravel.com/docs/6.0/broadcasting).
```
composer require pusher/pusher-php-server
```

Next, you should make sure to use Pusher as your broadcasting driver. This can be achieved by setting the BROADCAST_DRIVER environment variable in your `.env` file:
```
BROADCAST_DRIVER=pusher
```

### Pusher Configuration
When broadcasting events from your Laravel application to your WebSocket server, the default behavior is to send the event information to the official Pusher server. But since the Laravel WebSockets package comes with its own Pusher API implementation, we need to tell Laravel to send the events to our own server.

To do this, you should add the host and port configuration key to your `config/broadcasting.php` and add it to the pusher section. The default port of the Laravel WebSocket server is 6001.
```
'pusher' => [
    'driver' => 'pusher',
    'key' => env('PUSHER_APP_KEY'),
    'secret' => env('PUSHER_APP_SECRET'),
    'app_id' => env('PUSHER_APP_ID'),
    'options' => [
        'cluster' => env('PUSHER_APP_CLUSTER'),
        'encrypted' => true,
        'host' => env('MIX_PUSHER_APP_SERVER','127.0.0.1'),
        'port' => env('MIX_PUSHER_APP_PORT',6001),
        'scheme' => 'http'
    ],
],
```

### Configuring WebSocket Apps
`config/websockets.php` Dosyanıza ek uygulamalar ekleyebilirsiniz.
```
'apps' => [
        [
            'id' => env('PUSHER_APP_ID'),
            'name' => env('APP_NAME'),
            'key' => env('PUSHER_APP_KEY'),
            'secret' => env('PUSHER_APP_SECRET'),
            'path' => env('PUSHER_APP_PATH'),
            'capacity' => null,
            'enable_client_messages' => false,
            'enable_statistics' => true,
        ],
    ],
```

## Client Side Installation
### Pusher Channels
Laravel Echo is a JavaScript library that makes it painless to subscribe to channels and listen for events broadcast by your server-side broadcasting driver. You may install Echo via the NPM package manager. In this example, we will also install the `pusher-js` package since we will be using the Pusher Channels broadcaster:
```
npm install --save-dev laravel-echo pusher-js
```

Once Echo is installed, you are ready to create a fresh Echo instance in your application's JavaScript. A great place to do this is at the bottom of the `resources/js/bootstrap.js` file that is included with the Laravel framework. By default, an example Echo configuration is already included in this file - you simply need to uncomment it:
```
import Echo from 'laravel-echo';

window.Pusher = require('pusher-js');

window.Echo = new Echo({
    broadcaster: 'pusher',
    key: process.env.MIX_PUSHER_APP_KEY,
    wsHost: process.env.MIX_PUSHER_APP_SERVER,
    wsPort: process.env.MIX_PUSHER_APP_PORT,
    forceTLS: false,
    disableStats: true
});
```

Once you have uncommented and adjusted the Echo configuration according to your needs, you may compile your application's assets:
```
npm run dev
```

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
