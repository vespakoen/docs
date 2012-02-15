# Installation & Setup

## Contents

- [Requirements](#requirements)
- [Installation](#installation)
- [Basic Configuration](#basic-configuration)
- [Environments](#environments)
- [Cleaner URLs](#cleaner-urls)

<a name="requirements"></a>
## Requirements

- Apache, nginx, or another compatible web server.
- PHP 5.3+

<a name="installation"></a>
## Installation

1. [Download Laravel](https://github.com/laravel/laravel/zipball/master)
2. Extract the Laravel archive and upload the contents to your web server.
3. Navigate to your application in a web browser.

If all is well, you should see a pretty Laravel splash page. Get ready, there is lots more to learn!

### Extra Goodies

Installing the following goodies will help you take full advantage of Laravel, but they are not required:

- SQLite, MySQL, PostgreSQL, or SQL Server PDO drivers.
- Memcached or APC.

### Problems?

If you are having problems installing, try the following:

- Make sure the **public** directory is the document root of your web server.
- If you are using mod_rewrite, set the **index** option in **application/config/application.php** to an empty string.

<a name="config"></a>
## Basic Configuration

When starting a new project, you shouldn't be bombarded with loads of confusing configuration decisions. For that reason, Laravel is intelligently configured out of the box. The **application/config/application.php** file contains the basic configuration options for your application.

> **Note:** If you are using mod_rewrite, you should set the index option to an empty string.

<a name="environments"></a>
### Environments

Most likely, the configuration options you need for local development are not the same as the options you need on your production server. Laravel provides a convenient way to manage these differences using the **LARAVEL_ENV** environment variable. To get started, set the environment variable in your **httpd.conf** file:

	SetEnv LARAVEL_ENV local

> **Note:** Using a web server other than Apache? Check your server's documentation to learn how to set environment variables.

Great! Next, create an **application/config/local** directory. Any files and options you place in this directory will override the options in the base **application/config** directory. For example, you may wish to create an **application.php** file within your new **local** configuration directory:

	return array(

		'url' => 'http://localhost/laravel/public',

	);

In this example, the local **URL** option will override the **URL** option in **application/config/application.php**. Notice that you only need to specify the options you wish to override. Remember, development doesn't have to be a pain.

If you do not have access to your server's configuration files, you may manually set the **LARAVEL_ENV** variable at the top of Laravel's **paths.php** file:

	$_SERVER['LARAVEL_ENV'] = 'local';

<a name="cleaner-urls"></a>
### Cleaner URLs

Most likely, you do not want your application URLs to contain "index.php". You can remove it using HTTP rewrite rules. If you are using Apache to serve your application, make sure to enable mod_rewrite and create a **.htaccess** file like this one in your **public** directory:

	<IfModule mod_rewrite.c>
	     RewriteEngine on

	     RewriteCond %{REQUEST_FILENAME} !-f
	     RewriteCond %{REQUEST_FILENAME} !-d

	     RewriteRule ^(.*)$ index.php/$1 [L]
	</IfModule>

Is the .htaccess file above not working for you? Try this one:

	Options +FollowSymLinks
	RewriteEngine on

	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteCond %{REQUEST_FILENAME} !-d

	RewriteRule . index.php [L]

After setting up HTTP rewriting, you should set the **index** configuration option in **application/config/application.php** to an empty string.

> **Note:** Each web server has a different method of doing HTTP rewrites, and may require a slightly different .htaccess file.