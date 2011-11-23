## Models & Libraries

- [Models](#models)
- [Libraries](#libraries)

<a name="models"></a>
## Models

Models are the meat of your application, and they live in the **application/models** directory. A model is simply a class. Nothing more. It should contain business logic related to your application. Models may also be [Eloquent models](/docs/database/eloquent), which allow you to work with your database elegantly and beautifully. Models are auto-loaded by Laravel, so there is no need to **require** them before using them.

<a name="libraries"></a>
## Libraries

Libraries can be considered the "helper" classes of your application. Usually, they are classes that do not contain logic specific to your application; however, they are important to its functionality. For example, a Twitter library may be used to fetch your recent tweets and display them on a view. Fetching Tweets from Twitter isn't specific to your application, but it is still important. Place your generic, helper libraries in the **application/libraries** directory. Like models, libraries are also auto-loaded by Laravel.

The PSR-0 naming standard is becoming more popular among PHP developers. Because of this, full support for PSR-0 auto-loading is provided by Laravel. To use a PSR-0 library, simply drop it in the **application/libraries** directory and Laravel will take care of the rest!