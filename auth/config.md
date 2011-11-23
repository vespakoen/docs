## Authentication Configuration

Most interactive applications have the ability for users to login and logout. Obvious, right? Laravel provides a simple class to help you validate user credentials and retrieve information about the current user of your application.

The quickest way to get started is to create an [Eloquent model](/docs/database/eloquent) in your **application/models** directory:

	class User extends Eloquent {}

Next, let's define **email** and **password** columns on your user database table. The password column should hold 60 alpha-numeric characters.

Great job! You're ready to start using the Auth class. However, there are more advanced configuration options available if you wish to use them.

Let's dig into the **application/config/auth.php** file. In this file you will find three closures: **user**, **attempt**, and **logout**:

	'user' => function($id)
	{
		if ( ! is_null($id) and filter_var($id, FILTER_VALIDATE_INT) !== false)
		{
			return User::find($id);
		}
	}

The **user** function is called when the Auth class needs to retrieve a user by their primary key. As you can see, the default implementation of this function uses an "User" Eloquent model to retrieve the user by ID. However, if you are not using Eloquent, you are free to modify this function to meet the needs of your application.

	'attempt' => function($username, $password, $config)
	{
		$user = User::where($config['username'], '=', $username)->first();

		if ( ! is_null($user) and Hash::check($password, $user->password))
		{
			return $user;
		}
	}


The **attempt** function is called when the Auth class needs to retrieve a user by their username, such as when using the **Auth::attempt** method to login a user. The default implementation of this function uses an "User" Eloquent model to retrieve the user, then verifies that the given password matches the password stored in the database. The authentication configuration array is passed to the function, giving you convenient access to the **username** option you have specified.

If the user is authenticated, you should return the user object. Just make sure the object you return has an "id" property. The ID will be stored in the session by the Auth class so the user can be identified on subsequent requests to your application.

	'logout' => function($user) {}

The **logout** function is called when the **Auth::logout** method is called. This function gives you a convenient location to interact with any third-party authentication providers you may be using.