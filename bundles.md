# Bundles

## Contents

- [The Basics](#the-basics)
- [Creating Bundles](#creating-bundles)
- [Starting Bundles](#starting-bundles)
- [Routing To Bundles](#routing-to-bundles)
- [Using Bundles](#using-bundles)
- [Installing Bundles](#installing-bundles)
- [Upgrading Bundles](#upgrading-bundles)

<a name="the-basics"></a>
## The Basics

Bundles are the heart of Laravel 3.0. They are a simple way to group code into convenient "bundles". A bundle can have it's own views, configuration, routes, migrations, tasks, and more. A bundle could be everything from a database ORM to a robust authentication system. Let your imagination run wild with what you can do with bundles.

<a name="creating-and-registering"></a>
## Creating Bundles

Creating a bundle is a walk in the park. Just create a folder for the bundle within your **bundles** directory. For this example, let's create an "admin" bundle, which  could house the administrator back-end to our application. Great, so we have our folder. Remember the **start.php** file **application** folder? You can have one of those for your bundle. It is run everytime the bundle is loaded. Let's create one:

**Creating a bundle start.php file:**

	<?php

	Autoloader::namespaces(array(
		'Admin' => 'path('bundle').'admin/models',
	));

Great! In this start file, we've told the auto-loader that classes that are namespaced to "Admin" should be loaded out of our bundle's models directory. You can do anything you want in your start file, but typically it is used for registering classes with the auto-loader. **In fact, you aren't required to create a start file for your bundle.**

Next, we'll look at how to register this bundle with our application!

<a name="registering-bundles"></a>
## Registering Bundles

OK, so we have our Admin bundle, but we need to register it with Laravel. Pop open your **application/bundles.php** file. This is where you register all bundles used by your application. Let's add ours:

**Registering a simple bundle:**

	return array('admin'),

Great! By convention, Laravel will assume that the Admin bundle is located at the root level of the bundle directory, but we can specify another location if we wish:

**Registering a bundle with a custom location:**

	return array(

		'admin' => array('location' => 'userscape/admin'),

	);

Now Laravel will look for our bundle in **bundles/userscape/admin**.

<a name="starting-bundles"></a>
## Starting Bundles

So our bundle is created and registered, but how can we start using it? First, we need to start it. It's simple:

**Starting a bundle:**

	Bundle::start('admin');

This tells Laravel to run the **start.php** file for the bundle, which will register its classes in the auto-loader. The start method will also load the **routes.php** file for the bundle if it is present.

> **Note:** The bundle will only be started once. Subsequent calls to the start method will be ignored.

Each time a bundle is started, it fires an event. You can easily listen for the starting of bundles like so:

**Listen for a bundle's start event:**

	Event::listen('started: admin', function()
	{
		// The "admin" bundle has started...
	});

<a name="routing-to-bundles"></a>
## Routing To Bundles

You can easily setup bundles to handle requests to your application. Let's go back to the **application/bundles.php** file and add something:

	return array(

		'admin' => array('handles' => 'admin'),

	);

Notice the new **handles** option in our array? This tells Laravel to load the Admin bundle on any requests where the URI begins with "admin". It's a snap to route to your bundles.

Refer to the documentation on [bundle routing](/docs/routing#bundle-routes) and [bundle controllers](/docs/controllers#controller-routing) for more information on routing and bundles.

<a name="using-bundles"></a>
## Using Bundles

As mentioned previously, bundles can have views, configuration, language files and more. Laravel uses a double-colon syntax for loading these items. So, let's look at some examples:

**Loading a bundle view:**

	return View::make('bundle::view');

**Loading a bundle configuration item:**

	return Config::item('bundle::file.option');

**Loading a bundle language line:**

	return Lang::line('bundle::file.line');

<a name="installing-bundles"></a>
## Installing Bundles

Of course, you may always install bundles manually; however, the "Artisan" CLI provides an awesome method of installing and upgrading your bundle. The framework uses simple Zip extraction to install the bundle. Here's how it works.

**Installing a bundle via Artisan:**

	php artisan bundle:install eloquent

Need a list of available bundles? Check out the Laravel [bundle directory](http://bundles.laravel.com)

<a name="upgrading-bundles"></a>
## Upgrading Bundles

When you upgrade a bundle, Laravel will automatically remove the old bundle and install a fresh copy.

**Upgrading a bundle via Artisan:**

	php artisan bundle:upgrade eloquent

Since the bundle is totally removed on an upgrade, you must be aware of any changes you have made to the bundle code before upgrading. You may need to change some configuration options in a bundle. Instead of modifying the bundle code directly, use the bundle start events to set them. Place something like this in your **application/start.php** file.

**Listening for a bundle's start event:**

	Event::listen('started: admin', function()
	{
		Config::set('admin::file.option', true);
	});