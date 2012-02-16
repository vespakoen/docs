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

Controllers are responsible for handling incoming requests to your application. Typically, they will ask a model for data, and then return a view that presents that data to the user.

Controller classes should be stored in **application/controllers** and should extend the Base\_Controller class. A Home\_Controller class is included with Laravel.

**Creating a simple controller:**

	class Admin_Controller extends Base_Controller {

		public function action_index()
		{
			//
		}

	}

Methods that you want to be web-accessible should be prefixed with "action\_". All other methods, regardless of scope, will not be web-accessible.

The Base\_Controller class extends the main Laravel Controller class, and gives you a convenient place to put methods that are common to many controllers.

<a name="controller-routing"></a>
## Controller Routing

All routes in Laravel must be explicitly defined, including routes to controllers. However, a helpful short-cut is provided to make routing to controllers a breeze.

**Registering the "home" controller with the Router:**

	Route::controller('home');

**Registering several controllers with the router:**

	Route::controller(array('admin', 'dashboard.panel'));

Once a controller is registered, you may access its methods using a simple URI convention:

	http://localhost/controller/method/arguments

This convention is similar to that employed by CodeIgniter and other popular frameworks, where the first segment is the controller name, the second is the method, and the remaining segments are passed to the method as arguments. If no method segment is present, the "index" method will be used.

This routing convention may not be desirable for every situation, so you may also explicitly route URIs to controller actions using a simple, intuitive syntax.

**Registering a route that points to a controller action:**

	Route::get('welcome', 'home@index');

**Registering a filtered route that points to a controller action:**

	Route::get('welcome', array('after' => 'log', 'uses' => 'home@index'));

<a name="bundle-controllers"></a>
## Bundle Controllers

Creating controllers that belong to bundles is just as simple as creating your application controllers. Just prefix the controller calss name with the name of the bundle, so if your bundle is named "admin", your controller classes would look like this:

**Creating a bundle controller class:**

	class Admin_Home_Controller extends Base_Controller {

		public function action_index()
		{
			return "Hello Admin!";
		}

	}

But, how do you register a bundle controller with the router? It's simple. Here's what it looks like:

**Registering a bundle's controller with the router:**

	Route::controller('admin::home');

Great! Now we can access our "admin" bundle's home controller from the web!

<a name="action-filters"></a>
## Action Filters

You may assign "before" and "after" filters to controller actions within the controller's constructor.

**Attaching a filter to all actions:**

	$this->filter('before', 'auth');

**Attaching a filter to only some actions:**

	$this->filter('before', 'auth')->only(array('index', 'list'));

**Attaching a filter to all except a few actions:**

	$this->filter('before', 'auth')->except(array('add', 'posts'));

*Further Reading:*

- *[Route Filters](/docs/routes#filters)*

<a name="nested-controllers"></a>
## Nested Controllers

Controllers may be located within any number of sub-directories within the main **application/controllers** folder.

Define the controller class and store it in **controllers/admin/panel.php**.

	class Admin_Panel_Controller extends Base_Controller {

		public function action_index()
		{
			//
		}

	}

**Register the nested controller with the router using "dot" syntax:**

	Route::controller('admin.panel');

**Access the "index" action of the controller:**

	http://localhost/admin/panel

<a name="restful-controllers"></a>
## RESTful Controllers

Instead of prefixing controller actions with "action_", you may prefix them with the HTTP verb they should respond to.

**Adding the RESTful property to the controller:**

	class Home_Controller extends Base_Controller {

		public $restful = true;

	}

**Building RESTful controller actions:**

	class Home_Controller extends Base_Controller {

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

> **Note:** Before diving into controller dependency injection, you may wish to read the documentation on Laravel's beautiful [IoC container](/docs/start/ioc).