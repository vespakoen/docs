# Schema Builder

## Contents

- [The Basics](#the-basics)
- [Creating & Dropping Tables](#creating-tables)
- [Adding Columns](#adding-columns)
- [Dropping Columns](#dropping-columns)
- [Adding Indexes](#adding-indexes)
- [Dropping Indexes](#dropping-indexe)
- [Dropping Tables](#dropping-tables)

<a name="the-basics"></a>
## The Basics

Laravel includes a beautiful method of creating and modifying your database tables. Using a fluent syntax, you can work with your tables without using any vendor specific SQL. You'll love using it.

<a name="creating-tables"></a>
## Creating Tables

The **Schema** class is used to create and modify tables. Let's jump right into an example:

**Creating a simple database table:**

	Schema::create('users', function($table)
	{
		$table->increments('id');
	});

Let's go over this example. The **create** method tells the Schema builder that this is a new table, so it should be created. In the second argument, we passed a Closure which receives a Table instance. Using this Table object, we can fluently add and drop columns and indexes on the table.

Dropping the table is just as easy:

**Dropping a table from the database:**

	Schema::drop('users');

Sometimes you may need to specify the database connection on which the schema operation should be performed.

**Specifying the connection to run the operation on:**

	Schema::create('users', function($table)
	{
		$table->on('connection');
	});

<a name="adding-columns"></a>
## Adding Columns

You may add columns to the table using a fluent, expressive syntax. The fluent table builder's methods allow you to add columns without using vendor specific SQL. Let's go over it's methods:

**Adding an incrementing ID to the table:**

	$table->increments('id');

**Adding a VARCHAR equivalent column:**

	$table->string('email');

**Adding a VARCHAR equivalent with a length:**

	$table->string('name', 100);

**Adding an INTEGER equivalent to the table:**

	$table->integer('votes');

**Adding a FLOAT equivalent to the table:**

	$table->float('amount');

**Adding a BOOLEAN equivalent to the table:**

	$table->boolean('confirmed');

> **Note:** Laravel's "boolean" type maps to a small integer column on all database systems.

**Adding a DATE equivalent to the table:**

	$table->date('created_at');

Often you may want to specify **created\_at** and **updated\_at** columns on the database. It's simple:

	$table->timestamps();

**Adding a TIMESTAMP equivalent to the table:**

	$table->timestamp('added_on');

**Adding a TEXT equivalent to the table:**

	$table->text('description');

**Adding a BLOB equivalent to the table:**

	$table->blob('data');

<a name="adding-indexes"></a>
## Adding Indexes

