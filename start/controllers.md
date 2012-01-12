## Controllers

- [Creating Controllers](/docs/start/controllers#define)
- [Adding Actions](/docs/start/controllers#actions)
- [RESTful Actions](/docs/start/controllers#restful)
- [Action Filters](/docs/start/controllers#filters)
- [Layouts](/docs/start/controllers#layouts)
- [Dependency Injection](/docs/start/controllers#di)
- [Routes To Controllers](/docs/start/controllers#routes)

<a name="define"></a>
## Creating Controllers

Maybe you want to take a more conventional MVC approach and use controllers? No problem. Using controllers in Laravel is amazingly simple. You will love it.

A simple controller is included with Laravel. Just open up the **application/controllers/home.php** file. Notice that the controller has an **action\_index** method which returns the Laravel splash page. To make your application use this controller action instead of the default route, simply remove the default route from the **application/routes.php** file. Does the splash page still show up when you access your application? Great! You are now using controller's to drive your application.

All controllers should live in the **application/controllers** directory. Controller class names use a **Foo\_Controller** convention, where "Foo" is the name of your controller. So, for example, a "user" controller would look something like this:

	class User_Controller extends Controller {}

Notice that the controller extends the base **Controller** class. This class provides some core functionality used by every Laravel controller. Also, even though the class name is **User\_Controller**, the controller class should be stored in **application/controllers/user.php**.

When creating nested controllers, the controller's name should reflect its location within the **controllers** directory. So, a controller at **application/controllers/user/profile.php** should have a class name of **User_Profile_Controller**. This controller will handle all requests beginning with **http://example.com/user/profile**. You are free to nest controllers as deep as you wish!

<a name="actions"></a>
## Adding Actions

You may be wondering how Laravel knows which controller and action to call. It's simple. The first segment of the request URI is considered the controller name, and the second segment of the request URI is considered the action name. Any remaining URI segments are passed to the controller action as parameters.

For example, a request to **http://example.com/user/index** would call the **action\_index** method on the **User\_Controller**. But what about requests to **http://example.com**? These requests will use the default controller and action, which is **Home\_Controller** and **action\_index**. Of course, if the request only specifies the controller, the **action_index** will still be called.

Adding your own actions to controllers couldn't be easier. You may have noticed that each action method is prefixed with **action\_**. This allows you to specify actions which have the same name as native PHP functions, such as **list**. So, let's add a "profile" action to our user controller:

	public function action_profile($id) {}

Notice that this action accepts an **id** as an argument. Remember how any extra URI segments are passed to the action as parameters? That means a request to **http://example.com/user/profile/25** would call our **profile** method on the **User_Controller**, and would pass **25** into the action as a parameter.

<a name="restful"></a>
## RESTful Actions

You probably noticed that controller actions typically respond to all HTTP verbs. However, it's a breeze to setup controller actions to respond based on the request's HTTP verb. Just set the **restful** property on the controller:

	class User_Controller extends Controller {

		public $restful = true;

	}

Great! Now, instead of prefixing your actions with **action**, you can prefix them with the HTTP verb they should respond to:

	public function get_index() {}

	public function post_index() {}

It couldn't be simpler. RESTful controllers provide a beautiful compliment to typical controllers. Enjoy.

<a name="filters"></a>
## Action Filters

If you have used Laravel's RESTful routes, you probably love how easy it is to specify filters on the route. It's just as easy with controllers. Just use the **filter** method within your controller's constructor:

	public function __construct()
	{
		$this->filter('before', 'auth');
	}

This will attach the **auth** filter to all of the controller's actions. However, sometimes you may need to attach a filter to a few actions. It's a breeze using the **only** and **except** methods:

	public function __construct()
	{
		$this->filter('before', 'csrf')->only('login');
		$this->filter('before', 'auth')->except('logout');
	}

You may even attach filters to run only when a given HTTP verb is used for the request. Just use the **on** method:

	public function __construct()
	{
		$this->filter('before', 'csrf')->on('post');
	}

Of course, you may specify as many filters and methods as you wish using arrays:

	public function __construct()
	{
		$this->filter('before', array('auth', 'csrf'))
                         ->only(array('login', 'profile'));
	}

<a name="layouts"></a>
## Layouts

Your application probably uses a common layout across all of its pages. Manually creating this layout within every controller action can be a pain. Specifying a controller layout will make your development much more enjoyable. To get started, simply add a **layout** property to your controller:

	class User_Controller extends Controller {
		
		public $layout = 'layouts.common';

	}

Now you may access the layout view from within your controller actions:

	public function action_profile()
	{
		$this->layout->content = View::make('user.profile');
	}

It couldn't be simpler. Unlike other frameworks, there is no need for a separate, clunky template controller. Also, when using a layout, you do not need to explicitly return the layout at the end of your action. Laravel is smart enough to know you are using a layout, and will automatically consider the layout the response from the action.

<a name="di"></a>
## Dependency Injection

If you are focusing on writing testable code, you will probably want to inject dependencies into the constructor of your controller. No problem. Just register your controller in the [IoC container](/docs/start/ioc). When registering the controller with the container, prefix the key with **controllers.**. So, we could register our user controller like so:

	'controllers.user' => array('resolver' => function()
	{
		return new User_Controller;
	})

When a request to a controller enters your application, Laravel will automatically determine if the controller is registered in the container, and if it is, will use the container to resolve an instance of the controller.

> **Note:** Before diving into controller dependency injection, you may wish to read the documentation on Laravel's beautiful [IoC container](/docs/start/ioc).

<a name="routes"></a>
## Routes To Controllers

Sometimes you may wish to point a RESTful route to a controller. For example, perhaps you want to point several URIs to the same controller action. It couldn't be simpler. In your **application/routes.php** file, simply specify which controller and action should be used by the route:

	'GET /user/(:num)/edit' => 'user@edit'

Notice the **user@edit** pointer. This tells Laravel to call the **action_edit** method on the **User_Controller**. Of course, we can call a nested controller using "dot" notation.

	'GET /user/(:num)/edit' => 'user.profile@edit'

This would call the **action_edit** method on the **User_Profile_Controller**. The route wildcards will automatically be passed to the controller action as parameters. Isn't it simple?

Sometimes you may want to create a named route that points to a controller. You can use the **uses** key to specify the controller:

	'GET /user' => array('name' => 'profile', 'uses' => 'user@profile');