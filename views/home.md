# Views & Responses

## Contents

- [The Basics](#the-basics)
- [Binding Data To Views](#binding-data-to-views)
- [Nesting Views](#nesting-views)
- [Named Views](#named-views)
- [View Composers](#view-composers)
- [Redirects](#redirects)
- [Redirecting With Flash Data](#redirecting-with-flash-data)
- [Downloads](#downloads)
- [Errors](#errors)

<a name="the-basics"></a>
## The Basics

Views contain the HTML that is sent to the person using your application. By separating your view from the business logic of your application, your code will be cleaner and easier to maintain.

All views are stored within the **application/views** directory and use the PHP file extension. The **View** class provides a simple way to retrieve your views and return them to the client. Let's look at an example!

**Creating the view:**

	<html>
		I'm stored in views/home/index.php!
	</html>

**Returning the view from a route:**

	Route::get('/', function()
	{
		return View::make('home.index');
	});

Sometimes you will need a little more control over the response sent to the browser. For example, you may need to set a custom header on the response, or change the HTTP status code. Here's how:

**Returning a custom response:**

	Route::get('/', function()
	{
		$headers = array('foo' => 'bar');

		return Response::make('Hello World!', 200, $headers);
	});

**Returning a custom response containing a view:**

	return Response::view('home', 200, $headers);

<a name="binding-data-to-views"></a>
## Binding Data To Views

Typically, a route or controller will request data from a model that the view needs to display. So, we need a way to pass the data to the view. It's a breeze. There are several ways to bind data to the view, so just pick the way you like best!

**Binding data to a view:**

	Route::get('/', function()
	{
		return View::make('home')->with('name', 'James');
	});

**Accessing the bound data within a view:**

	<html>
		Hello, <?php echo $name; ?>.
	</html>

**Chaining the binding of data to a view:**

	View::make('home')
		->with('name', 'James')
		->with('votes', 25);

**Passing an array of data to bind data:**

	View::make('home', array('name' => 'James'));

**Using magic methods to bind data:**

	$view->name  = 'James';
	$view->email = 'example@example.com';

**Using the ArrayAccess interface methods to bind data:**

	$view['name']  = 'James';
	$view['email'] = 'example@example.com';

<a name="nesting-views"></a>
## Nesting Views

Often you will want to nest views within views. Nested views are sometimes called "partials", and help you keep views small and modular.

**Binding a nested view using the "nest" method:**

	View::make('home')->nest('footer', 'partials.footer');

**Passing data to a nested view:**

	$view = View::make('home');

	$view->nest('content', 'orders', array('orders' => $orders));

<a name="named-views"></a>
## Named Views

Named views make your code more expressive and beautiful. Using them is simple:

**Registering a named view:**

	View::name('layouts.default', 'layout');

**Getting an instance of the named view:**

	return View::of('layout');

**Binding data to a named view:**

	return View::of('layout', array('orders' => $orders));

<a name="view-composers"></a>
## View Composers

Each time a view is created, its "composer" event will be fired. You can listen for this event and use it to bind assets and common data to the view each time it is created. Here's an example:

**Register a view composer for the "home" view:**

	View::composer('home', function($view)
	{
		$view->nest('footer', 'partials.footer');
	});

Now each time the "home" view is created, an instance of the View will be passed to the registered Closure, allowing you to prepare the view however you wish.

> **Note:** A view can have more than one composer. Go wild!

<a name="redirects"></a>
## Redirects

**Redirecting to another URI:**

	return Redirect::to('user/profile');

**Redirecting with a specific status:**

	return Redirect::to('user/profile', 301);

**Redirecting to a secure URI:**

	return Redirect::to_secure('user/profile');

**Redirecting to the root of your application:**

	return Redirect::home();

**Redirecting to a named route:**

	return Redirect::to_route('profile');

**Redirecting to a controller action:**

	return Redirect::to_action('home@index');

Sometimes you may need to redirect to a named route, but also need to specify the values that should be used instead of the route's URI wildcards. It's easy to replace the wildcards with proper values:

**Redirecting to a named route with wildcard values:**

	return Redirect::to_route('profile', array($username));

**Redirecting to an action with wildcard values:**

	return Redirect::to_action('user@profile', array($username));

<a name="redirecting-with-flash-data"></a>
## Redirecting With Flash Data

After a user creates an account or signs into your application, it is common to display a welcome or status message. But, how can you set the status message so it is available for the next request? It's actually a walk in the park:

	return Redirect::to('profile')->with('status', 'Welcome Back!');

You can access your message from the view with the Session get method:

	$status = Session::get('status');

*Further Reading:*

- *[Sessions](/docs/session/config)*

<a name="downloads"></a>
## Downloads

**Sending a file download response:**

	return Response::download('file/path.jpg');

**Sending a file download and assigning a file name:**

	return Response::download('file/path.jpg', 'photo.jpg');

<a name="errors"></a>
## Errors

Generating the proper responses for error views is simple. Simply specify the code you wish to return. The corresponding view stored in **views/error** will be returned.

**Generating a 404 error response:**

	return Response::error('404');

**Generating a 500 error response:**

	return Response::error('500');