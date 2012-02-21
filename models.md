# Models & Libraries

## Contents

- [Models](#models)
- [Libraries](#libraries)
- [Best Practices](#best-practices)

<a name="models"></a>
## Models

The "models" library typically contains the heart of your application. Think of controllers, and even Laravel itself, as just a medium of accessing your application from the web. The heart of your application lives in the "models" directory.

For instance, perhaps you are writing a satellite tracking application. You might want to have a "Satellite" object. Perhaps users can enter their GPS coordinates to find satellites over their location. So, you may have a "Location" object, as well as a location validator that accepts an instance of that location. Get the idea?

*Further Reading:*

- [Auto Loading](/docs/loading)

<a name="libraries"></a>
## Libraries

Libraries are classes that perform tasks that aren't specific to your application. For instance, consider a PDF generation library that converts HTML. That task, although complicated, is not specific to your application, so it is considered a "library". Laravel makes it a breeze to drop in a library and start using it within your application. To learn how, check out the documentation on [auto loading](/docs/loading).

<a name="best-practices"></a>
## Best Practices

We've all head the mantra: "controllers should be thin!" But, how do we apply that in real life? It's possible that part of the problem is the word "model". What does it even mean? Is it even a useful term? Many associate "model" with "database", which leads to having very bloated controllers, with light models that access the database. Let's explore some alternatives.

What if we just totally scrapped the "models" directory? Let's name it something more useful. In fact, let's just give it the same as our application. Perhaps are our satellite tracking site is named "Trackler", so let's create a "trackler" directory within the application folder.

Great! Next, let's break our classes into "entities", "services", and "repositories". So, we'll create each of those three directories within our  "trackler" folder. Let's explore each one:

### Entities

Think of entities as the data containers of your application. They primarily just contain properties. So, in our application, we may have a "Location" entity which has "latitude" and "longitude" properties. It could look something like this:

	<?php namespace Trackler\Entities;
	
	class Location {

		public $latitude;
		public $longitude;

		public function __construct($latitude, $longitude)
		{
			$this->latitude = $latitude;
			$this->longitude = $longitude;
		}

	}

Looking good. Now that we have an entity, let's explore our other two folders.

### Services

Services contain the *processes* of your application. So, let's keep using our Trackler example. Our application might have a form on which a user may enter their GPS location. However, we need to validate that the coordinates are correctly formatted. We need to *validate* the *location entity*. So, within our "services" directory, we could create a "validators" folder with the following class:

	<?php namespace Trackler\Services\Validators;

	use Trackler\Entities\Location;

	class Location_Validator {

		public static function validate(Location $location)
		{
			// Validate the location instance...
		}

	}

Great! Now we have a great way to test our validation in isolation from our controllers and routes! So, we've validated the location and we're ready to store it. What do we do now?

### Repositories

Repositories are the data access layer of your application. They are responsible for storing and retrieving the *entities* of your application. So, let's continue using our *location* entity in this example. We need a location repository that can store them. We could store them using any mechanism we want, whether that is a relational database, Redis, or the next storage hotness. Let's look at an example:

	<?php namespace Trackler\Repositories;

	use Trackler\Entities\Location;

	class Location_Repository {

		public function save(Location $location, $user_id)
		{
			// Store the location for the given user ID...
		}

	}

Wonderful. We have a clean separation of concerns between our application's entities, services, and repositories. This means we can inject stub repositories into our services or controllers, and test those pieces of our application in isolation from the database. Also, we can entirely switch data store technologies without affecting our services, entities, or controllers. We've achieved a good *separation of concerns*.

*Further Reading:*

- [IoC Container](/docs/ioc)