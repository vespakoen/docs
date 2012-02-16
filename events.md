# Events

## Contents

- [The Basics](#the-basics)
- [Firing Events](#firing-events)
- [Listening To Events](#listening-to-events)
- [Laravel Events](#laravel-events)

<a name="the-basics"></a>
## The Basics

Events provide a great away to build de-coupled applications, and allow plug-ins to tap into the core of your application without modifying its code. Let's jump right in.

<a name="firing-events"></a>
## Firing Events

Firing an event couldn't be any easy, just tell the **Event** class the name of the event you want to fire:

**Firing an event:**

	$responses = Event::fire('loaded');

Notice that we assigned the result of the **fire** method to a variable. This method will return an array containing the responses of all the event's listeners.

Sometimes you may want to fire an event, but just get the first response. Here's how:

**Firing an event and retrieving the first response:**

	$response = Event::first('loaded');

> **Note:** The **first** method will still fire all of the handlers listening to the event, but will only return the first response.

<a name="listening-to-events"></a>
## Listening To Events

So, what good are events if nobody is listening? It's simple to register event handlers. Let's take a look:

**Registering an event handler:**

	Event::listen('loaded', function()
	{
		// I'm executed on the "loaded" event!
	});

The Closure we provided to the method will be executed each time the "loaded" event is fired. Simple, huh?

<a name="laravel-events"></a>
## Laravel Events

There are several events that are fired by the Laravel core. Here they are:

**Event fired when a bundle is started:**

	Event::listen('started: bundle', function() {});

**Event fired when a database query is executed:**

	Event::listen('query', function($sql, $bindings, $time) {});