# Controllers

## Contents

- [The Basics](#the-basics)
- [Controller Routing](#controller-routing)
- [Bundle Controllers](#bundle-controllers)
- [Action Filters](#action-filters)
- [Nested Controllers](#nested-controllers)
- [RESTful Controllers](#restful-controllers)
- [Dependency Injection](#dependency-injection)

<a name="the-basics"></a>
## The Basics

Unlike many other frameworks, Laravel makes it possible to manage application logic in two different ways. The most common method of organizing application logic amongst PHP frameworks is the controller. A controller is responsible for handling incoming requests to your application. Typically, they will ask a model for data, and then return a view that presents that data to the user.

It's also possible to embed your application logic directly into routes. All functionality types can be implemented with either methodology so, it's up to you to determine which solution is best for your team and your project. We'll go over [routes in detail](/docs/routing) in another document.

Controller classes should be stored in **application/controllers** and should extend the Base\_Controller class. A Home\_Controller class is included with Laravel.

#### Creating a simple controller:

	class Admin_Controller extends Base_Controller
	{

		public function action_index()
		{
			//
		}

	}

Methods that you want to be web-accessible should be prefixed with "action\_". All other methods, regardless of scope, will not be web-accessible.

The Base\_Controller class extends the main Laravel Controller class, and gives you a convenient place to put methods that are common to many controllers.

<a name="controller-routing"></a>

It is important to be aware that all routes in Laravel must be explicitly defined, including routes to controllers. This means that controller methods that have not been exposed through route registration **cannot** be accessed. It's possible to automatically expose all methods within a controller using controller route registration. Controller route registrations are typically defined in **application/routes.php**.

Check [the routing page](/docs/routing#controller-routing) for more information on routing to controllers.

<a name="bundle-controllers"></a>
## Bundle Controllers

Bundles are Laravel's modular package system. Bundles can easily configured to handle requests to your application. We'll be going over [bundles in more detail](/docs/bundles) in another document.

Creating controllers that belong to bundles is almost identical to creating your application controllers. Just prefix the controller class name with the name of the bundle, so if your bundle is named "admin", your controller classes would look like this:

#### Creating a bundle controller class:

	class Admin_Home_Controller extends Base_Controller
	{

		public function action_index()
		{
			return "Hello Admin!";
		}

	}

But, how do you register a bundle controller with the router? It's simple. Here's what it looks like:

#### Registering a bundle's controller with the router:

	Route::controller('admin::home');

Great! Now we can access our "admin" bundle's home controller from the web!

<a name="action-filters"></a>
## Action Filters

You may assign "before" and "after" filters to controller actions within the controller's constructor.

#### Attaching a filter to all actions:

	$this->filter('before', 'auth');

#### Attaching a filter to only some actions:

	$this->filter('before', 'auth')->only(array('index', 'list'));

#### Attaching a filter to all except a few actions:

	$this->filter('before', 'auth')->except(array('add', 'posts'));

You may also limit a filter to run only on certain HTTP request methods:

#### Attaching a filter to run on POST:

	$this->filter('before', 'csrf')->on('post');

*Further Reading:*

- *[Route Filters](/docs/routing#filters)*

<a name="nested-controllers"></a>
## Nested Controllers

Controllers may be located within any number of sub-directories within the main **application/controllers** folder.

Define the controller class and store it in **controllers/admin/panel.php**.

	class Admin_Panel_Controller extends Base_Controller
	{

		public function action_index()
		{
			//
		}

	}

#### Register the nested controller with the router using "dot" syntax:

	Route::controller('admin.panel');

> **Note:** When using nested controllers, always register your controllers from most nested to least nested in order to avoid shadowing controller routes.

#### Access the "index" action of the controller:

	http://localhost/admin/panel

<a name="restful-controllers"></a>
## RESTful Controllers

Instead of prefixing controller actions with "action_", you may prefix them with the HTTP verb they should respond to.

#### Adding the RESTful property to the controller:

	class Home_Controller extends Base_Controller
	{

		public $restful = true;

	}

#### Building RESTful controller actions:

	class Home_Controller extends Base_Controller
	{

		public $restful = true;

		public function get_index()
		{
			//
		}

		public function post_index()
		{
			//
		}

	}

<a name="dependency-injection"></a>
## Dependency Injection

If you are focusing on writing testable code, you will probably want to inject dependencies into the constructor of your controller. No problem. Just register your controller in the [IoC container](/docs/ioc). When registering the controller with the container, prefix the key with **controller**. So, in our **application/start.php** file, we could register our user controller like so:

	IoC::register('controller: user', function()
	{
		return new User_Controller;
	});

When a request to a controller enters your application, Laravel will automatically determine if the controller is registered in the container, and if it is, will use the container to resolve an instance of the controller.

> **Note:** Before diving into controller dependency injection, you may wish to read the documentation on Laravel's beautiful [IoC container](/docs/ioc).