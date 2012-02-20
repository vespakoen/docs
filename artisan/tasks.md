# Tasks

## Contents

- [The Basics](#the-basics)
- [Creating & Running Tasks](#creating-tasks)
- [Bundle Tasks](#bundle-tasks)
- [CLI Options](#cli-options)

<a name="the-basics"></a>
## The Basics

Artisan tasks allow you to easily call "task" classes from the command line. You can even call specific methods on the class, passing any arguments you want. They are great for building command-line tools or jobs. Let's dig deeper.

<a name="creating-tasks"></a>
## Creating & Running Tasks

To create a task, simply create a new class in your **application/tasks** directory. The class name should be suffixed with "_Task", and should at least have a "run" method, like this:

**Creating a task class:**

	class Notify_Task {

		public function run($arguments)
		{
			// Do awesome notifying...
		}

	}

Now you can call the "run" method of your task via the command-line. You can even pass arguments:

**Calling a task from the command line:**

	php artisan notify

**Calling a task and passing arguments:**

	php artisan notify taylor

Remember, you can call specific methods on your task, so, let's add an "urgent" method to the notify task:

**Adding a method to the task:**

	class Notify_Task {

		public function run($arguments)
		{
			// Do awesome notifying...
		}

		public function urgent($arguments)
		{
			// This is urgent!
		}

	}

Now we can easily call our "urgent" method:

**Calling a specific method on a task:**

	php artisan notify:urgent

<a name="bundle-tasks"></a>
## Bundle Tasks

Creating a task for your bundle is simple. Just prefix the bundle name to the clas name of your task. So, if your bundle was named "admin", a task might look like this:

**Creating a task class that belongs to a bundle:**

	class Admin_Generate_Task {

		public function run($arguments)
		{
			// Generate the admin!
		}

	}

Calling your task is just as easy. Just use the usual Laravel double-colon syntax to indicate the bundle:

**Running a task belonging to a bundle:**

	php artisan admin::generate

**Running a specific method on a task belonging to a bundle:**

	php artisan admin::generate:list

<a name="cli-options"></a>
## CLI Options

**Setting the Laravel environment:**

	php artisan foo --env=local

**Setting the default database connection:**

	php artisan foo --database=sqlite