## IoC Container

- [Definition](/docs/start/ioc#definition)
- [Registering Objects](/docs/start/ioc#register)
- [Resolving Objects](/docs/start/ioc#resolve)

<a name="definition"></a>
## Definition

An IoC container is simply a way of managing the creation of objects. You can use it to define the creation of complex objects, allowing you to resolve them throughout your application using a single line of code. You may also use it to "inject" dependencies into your classes and controllers.

IoC containers help make your application more flexible and testable. Since you may register alternate implementations of an interface with the container, you may isolate the code you are testing from external dependencies using [stubs and mocks](http://martinfowler.com/articles/mocksArentStubs.html).

<a name="register"></a>
## Registering Objects

The resolvers for all items in the IoC container are defined in the **application/config/container.php** file. By default, there is nothing registered in the container. But, perhaps your application is using [SwiftMailer](http://swiftmailer.org/). The creation of a SwiftMailer instance can be a little cumbersome, so let's register it in the container:

	'mailer' => array('resolver' => function()
	{
		require_once LIBRARY_PATH.'SwiftMailer/lib/swift_required';

		$transport = Swift_MailTransport::newInstance();

		return Swift_Mailer::newInstance($transport);
	})

Great! Now we have registered a resolver for SwiftMailer in our container. But, what if we don't want the container to create a new mailer instance every time we need one? Maybe we just want the container to return the same instance after the intial instance is created. It's easy. Just tell the container the object should be a singleton:

	'mailer' => array('singleton' => true, 'resolver' => function()
	{
		...
	})

<a name="resolve"></a>
## Resolving Objects

Now that we have SwiftMailer registered in the container, we can resolve it using the **resolve** method on the **IoC** class:

	$mailer = IoC::resolve('mailer');

Of course, we could leverage the IoC container to accomplish dependency injection. Let's inject the mailer instance into another class:

	'repository.user' => array('resolver' => function()
	{
		return new User_Repository(IoC::resolve('mailer'));
	})

Note that we called the **resolve** method from within a resolver! This allows us to easily resolve deeply nested dependencies. This makes our repository more testable, since we can now inject a stub of SwiftMailer when testing the repository, allowing us to isolate the behavior of the repository from SwiftMailer.

> **Note:** You may also [register controllers in the container](/docs/start/controllers#di).