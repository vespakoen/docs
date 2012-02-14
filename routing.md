# Routing

## Contents

- [The Basics](#the-basics)
- [Wildcards](#wildcards)
- [Filters](#filters)
- [Route Groups](#route-groups)
- [Named Routes](#named-routes)
- [HTTPS Routes](#https-routes)

<a name="the-basics"></a>
## The Basics

Laravel uses the latest features of PHP 5.3 to make routing simple and expressive. It's a joy to build everything from simple JSON APIs to complex web applications. Routes are typically defined in **application/routes.php**.

**Registering a route that responds to "GET /":**

	Route::get('/', function()
	{
		return "Hello World!";
	});

**Registering a route that is valid for any HTTP verb:**

	Route::any('/', function()
	{
		return "Hello World!";
	});

**Registering routes for other request methods:**

	Route::post('user', function()
	{
		//
	});
	
	Route::put('user/(:num)', function($id)
	{
		//
	});

	Route::delete('user/(:num)', function($id)
	{
		//
	});

<a name="wildcards"></a>
## Wildcards

**Forcing a URI segment to be any digit:**

	Route::get('user/(:num)', function($id)
	{
		//
	});

**Allowing a URI segment to be any alpha-numeric string:**

	Route::get('post/(:any)', function($title)
	{
		//
	});

**Allowing a URI segment to be optional:**

	Route::get('page/(:any?)', function($page = 'index')
	{
		//
	});

<a name="filters"></a>
## Filters

Route filters may be run before or after a route is executed. If a "before" filter returns a value, that value is considered the response to the request and the route is not executed, making it a breeze to implement authentication filters, etc.

**Registering a filter:**

	Route::filter('filter', function()
	{
		return Redirect::to('home');
	});

**Attaching a filter to a route:**

	Route::get('blocked', array('before' => 'filter', function()
	{
		return View::make('blocked');
	}));

**Attaching an "after" filter to a route:**

	Route::get('download' array('after' => 'log', function()
	{
		//
	}));

**Attaching multiple filters to a route:**

	Route::get('create', array('before' => 'auth|csrf', function()
	{
		//
	}));

**Passing parameters to filters:**

	Route::get('panel', array('before' => 'role:admin', function()
	{
		//
	}));

<a name="route-groups"></a>
## Route Groups

Route groups allow you to attach a set of attributes to a group of routes, allowing you to keep your code neat and tidy.

	Route::group(array('before' => 'auth'), function()
	{
		Route::get('panel', function()
		{
			//
		});

		Route::get('dashboard', function()
		{
			//
		});
	});

<a name="named-routes"></a>
## Named Routes

Constantly generating URLs or redirects using a route's URI leads to brittle code. Assining the route a name gives you a convenient way to refer to the route throughout your application.

**Registering a named route:**

	Route::get('/', array('as' => 'home', function()
	{
		return "Hello World";
	});

**Generating a URL to a named route:**

	$url = URL::to_route('home');

**Redirecting to the named route:**

	return Redirect::to_route('home');

<a name="https-routes"></a>
## HTTPS Routes

When defining routes, you may use the "https" attribute to indicate that the HTTPS protocol should be used when generating a URL or Redirect to that route.

**Defining an HTTPS route:**

	Route::get('login', array('https' => true, function()
	{
		return View::make('login');
	});

**Using the "secure" short-cut method:**

	Route::secure('GET', 'login', function()
	{
		return View::make('login');
	});