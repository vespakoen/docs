# Input & Cookies

## Contents

- [Input](#input)
- [Files](#files)
- [Old Input](#old-input)
- [Remembering Old Input](#remembering-old-input)
- [Cookies](#cookies)

<a name="input"></a>
## Input

The **Input** class handles input that comes into your application via GET, POST, PUT, or DELETE requests. Retrieving input using the Input class is effortless:

**Retrieve a value from the input array:**

	$email = Input::get('email');

> **Note:** The "get" method is used for all request types, not just GET requests.

**Retrieve all input from the input array:**

	$input = Input::get();

**Retrieve all input including the $_FILES array:**

	$input = Input::all();

By default, *null* will be returned if the input item does not exist. However, you may pass a different default value as a second parameter to the method:

**Returning a default value if the requested input item doesn't exist:**

	$name = Input::get('name', 'Fred');

**Using a Closure to return a default value:**

	$name = Input::get('name', function() {return 'Fred';});

**Determining if the input contains a given item:**

	if (Input::has('name')) ...

> **Note:** The "has" method will return *false* if the input item is an empty string.

<a name="files"></a>
## Files

**Retrieving an item from the $_FILES array:**

	$picture = Input::file('picture');

**Retrieving a specific item from a $_FILES array:**

	$size = Input::file('picture.size');

<a name="old-input"></a>
## Old Input

Have you ever tried to re-populate an input form after an invalid form submission? It can get pretty clunky. Not in Laravel. You can easily retrieve the input from the previous request. First, you need to flash the input data to the session:

**Flashing input to the session:**

	Input::flash();

**Flashing selected input to the session:**

	Input::flash('only', array('username', 'email'));

	Input::flash('except', array('password', 'credit_card'));

**Retrieving a flashed input item from the previous request:**

	$name = Input::old('name');

> **Note:** You must specify a sesion driver before using the "old" method.

*Further Reading:*

- *[Sessions](/docs/session/config)*

<a name="redirecting-with-old-input"></a>
## Remembering Old Input

Since you will typically want to flash input right before a redirect, there is a beautiful shortcut for you to use:

**Flashing input from a Redirect instance:**

	return Redirect::to('login')->with_input();

**Flashing selected input from a Redirect instance:**

	return Redirect::to('login')->with_input('only', array('username'));

<a name="cookies"></a>
## Cookies

Laravel provides a nice wrapper around the $_COOKIE array. However, there are a few things you should be aware of before using it. First, all Laravel cookies contain a "signature hash". This allows the framework to verify that the cookie has not been modified on the client. Secondly, when setting cookies, the cookies are not immediately sent to the browser, but are pooled until the end of the request and then sent together.

**Retrieving a cookie value:**

	$name = Cookie::get('name');

**Returning a default value if the requested cookie doesn't exist:**

	$name = Cookie::get('name', 'Fred');

**Setting a cookie that lasts for 60 minutes:**

	Cookie::put('name', 'Fred', 60);

**Creating a "permanent" cookie that lasts five years:**

	Cookie::forever('name', 'Fred');

**Deleting a cookie:**

	Cookie::forget('name');