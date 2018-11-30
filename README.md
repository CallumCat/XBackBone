
# XBackBone 📤 [![Build Status](https://travis-ci.org/SergiX44/XBackBone.svg?branch=master)](https://travis-ci.org/SergiX44/XBackBone)
XBackBone is a simple, self-hosted, lightweight PHP backend for the instant sharing tool ShareX. It supports uploading and displaying images, GIF, video, code, formatted text, and file downloading and uploading. Also have a web UI with multi user management and past uploads history.

## Features

+ Supports every upload type from ShareX.
+ Code uploads syntax highlighting.
+ Video uploads player.
+ Files upload download page.
+ Multi language support.
+ User management, multi user features and roles.
+ Public and private uploads.
+ Web UI for each user.
+ Logging system.
+ Auto config generator for ShareX.
+ Share to Telegram.

## How to Install
XBackBone require PHP >= `7.1`, writable storage path and PDO, with installed the required extensions (ex. `php-sqlite3` for SQLite, `php-gd` and `php-json`).

### Web installation
+ **[release, stable]** Download latest release from GitHub: [Latest Release](https://github.com/SergiX44/XBackBone/releases/latest)
+ Extract the release zip to your document root.
+ Navigate to the webspace root (ex. `http://example.com/xbackbone`, this should auto redirect your browser to the install page `http://example.com/xbackbone/install/`)
+ Follow the instructions.

### Manual installation
+ **[release, stable]** Download latest release from GitHub: [Latest Release](https://github.com/SergiX44/XBackBone/releases/latest)
+ Extract the release zip to your document root.
+ Copy and edit the config file:
```sh
cp config.example.php config.php && nano config.php
```
By default, XBackBone will use Sqlite3 as DB engine, and a `storage` dir in the main directory. You can leave these settings unchanged for a simple personal installation.
You must set the `base_url`, or remove it for get dynamically the url from request (not recommended).

```php
return [
	'base_url' => 'https://example.com', // no trailing slash
	'storage_dir' => 'storage',
	'db' => [
		'connection' => 'sqlite',
		'dsn' => 'resources/database/xbackbone.db',
		'username' => null, // username and password not needed for sqlite
		'password' => null,
	]
];
```
+ Finally, run the migrate script to setup the database

```sh
php bin/migrate --install
```
+ Now just login with `admin/admin`, **be sure to change these credentials after your first login**.

#### Changing themes
XBackBone supports all [bootswatch.com](https://bootswatch.com/) themes.

From the web UI:
+ Navigate to the web interface as admin -> System Menu -> Choose a theme from the dropdown.

From the CLI:
+ Run the command `php bin/theme` to see the available themes.
+ Use the same command with the argument name (`php bin/theme <THEME-NAME>`) to choose a theme.
+ If you want to revert back to the original bootstrap theme, run the command `php bin/theme default`.

*Clear the browser cache once you have applied.*

#### Change app install name
Add to the `config.php` file an array element like this:
```php
return array(
    'app_name' => 'This line will overwrite "XBackBone"',
    ...
);
```

## How to update
+ Download and extract the release zip to your document root, overwriting any file.
+ Navigate to the `/install` path (es: `http://example.com/` -> `http://example.com/install/`)
+ Click the update button.
+ Done.


#### Docker deployment
+ [Docker container](https://hub.docker.com/r/pe46dro/xbackbone-docker)

## ShareX Configuration
Once you are logged in, just go in your profile settings and download the ShareX config file for your account.

## Linux Support
Since ShareX does not support Linux, XBackBone can generate a script that allows you to share an item from any tool:
+ Login into your account
+ Navigate to your profile and download the Linux script for your account.
+ Place the script where you want (ex. in your user home: `/home/<username>`).
+ Add execution permissions (`chmod +x xbackbone_uploader_XXX.sh`)
+ Run the script for the first time to create the desktop entry: `./xbackbone_uploader_XXX.sh -desktop-entry`.

Now, to upload a media, just use the right click on the file > "Open with ..." > search XBackBone Uploader (XXX) in the app list.
You can use this feature in combination with tools like [Flameshot](https://github.com/lupoDharkael/flameshot), just use the "Open with ..." button once you have done the screenshot.

*Note: XXX is the username of your XBackBone account.*

## Notes
If you do not use Apache, or the Apache `.htaccess` is not enabled, set your web server so that the `static/` folder is the only one accessible from the outside, otherwise even private uploads and logs will be accessible!
The NGINX configuration should be something like this:
```
# nginx configuration

location /app {
  return 403;
}

location /bin {
  return 403;
}

location /bootstrap {
  return 403;
}

location /resources {
  return 403;
}

location /storage {
  return 403;
}

location /vendor {
  return 403;
}

location /logs {
  return 403;
}

autoindex off;

location / {
  if (!-e $request_filename){
    rewrite ^(.*)$ /index.php break;
  }
}
```
## Screenshots
<p align="center">
  	<img src="https://i.imgur.com/zEHL5U3.png" width="400" title="Installation">
	<img src="https://i.imgur.com/aNeMhaU.png" width="400" title="Login">
	<img src="https://i.imgur.com/Sh1btnH.png" width="400" title="Admin view">
	<img src="https://i.imgur.com/GN6a9AG.png" width="400" title="User management">
	<img src="https://i.imgur.com/Cz6dkIP.png" width="400" title="User view">
	<img src="https://i.imgur.com/nFu9waN.png" width="400" title="User profile page">
	<img src="https://i.imgur.com/aQPDFDY.png" width="400" title="System settings">
	<img src="https://i.imgur.com/2ZRd27y.png" width="400" title="Public image upload view">
</p>

## Built with
+ Slim 3, since `v2.0` (https://www.slimframework.com/)
+ FlightPHP, up to `v1.x` (http://flightphp.com/)
+ Bootstrap 4 (https://getbootstrap.com/)
+ Font Awesome 5 (http://fontawesome.com)
+ ClipboardJS (https://clipboardjs.com/)
+ HighlightJS (https://highlightjs.org/)
+ JQuery (https://jquery.com/)
