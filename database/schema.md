# Schema Builder

## Contents

- [The Basics](#the-basics)
- [Creating & Dropping Tables](#creating-dropping-tables)
- [Adding Columns](#adding-columns)
- [Dropping Columns](#dropping-columns)
- [Adding Indexes](#adding-indexes)
- [Dropping Indexes](#dropping-indexes)

<a name="the-basics"></a>
## The Basics

Laravel includes a beautiful method of creating and modifying your database tables. Using a fluent syntax, you can work with your tables without using any vendor specific SQL. You'll love using it.

*Further Reading:*

- [Migrations](/docs/database/migrations)

<a name="creating-dropping-tables"></a>
## Creating & Dropping Tables

The **Schema** class is used to create and modify tables. Let's jump right into an example:

#### Creating a simple database table:

	Schema::create('users', function($table)
	{
		$table->increments('id');
	});

Let's go over this example. The **create** method tells the Schema builder that this is a new table, so it should be created. In the second argument, we passed a Closure which receives a Table instance. Using this Table object, we can fluently add and drop columns and indexes on the table.

#### Dropping a table from the database:

	Schema::drop('users');

Sometimes you may need to specify the database connection on which the schema operation should be performed.

#### Specifying the connection to run the operation on:

	Schema::create('users', function($table)
	{
		$table->on('connection');
	});

<a name="adding-columns"></a>
## Adding Columns

You may add columns to the table using a fluent, expressive syntax. The fluent table builder's methods allow you to add columns without using vendor specific SQL. Let's go over it's methods:

Command  | Description
------------- | -------------
`$table->increments('id');`  |  Incrementing ID to the table
`$table->string('email');`  |  VARCHAR equivalent column
`$table->string('name', 100);`  |  VARCHAR equivalent with a length
`$table->integer('votes');`  |  INTEGER equivalent to the table
`$table->float('amount');`  |  FLOAT equivalent to the table
`$table->boolean('confirmed');`  |  BOOLEAN equivalent to the table
`$table->date('created_at');`  |  DATE equivalent to the table
`$table->timestamp('added_on');`  |  TIMESTAMP equivalent to the table
`$table->timestamps();`  |  Adds **created\_at** and **updated\_at** columns
`$table->text('description');`  |  TEXT equivalent to the table
`$table->blob('data');`  |  BLOB equivalent to the table
`->nullable()`  |  Designate that the column allows NULL values

> **Note:** Laravel's "boolean" type maps to a small integer column on all database systems.

#### Example of creating a table and adding columns

	Schema::table('users', function($table)
	{
		$table->create();
		$table->increments('id');
		$table->string('username');
		$table->string('email');
		$table->string('phone')->nullable();
		$table->text('about');
		$table->timestamps();
	});

<a name="dropping-columns"></a>
## Dropping Columns

#### Dropping a column from a database table:

	$table->drop_column('name');

#### Dropping several columns from a database table:

	$table->drop_column(array('name', 'email'));

<a name="adding-indexes"></a>
## Adding Indexes

The Schema builder supports several types of indexes and they are dead simple to add to your table. There are two ways to add the indexes. Each type of index has its method; however, you can also fluently define an index on the same line as a column addition. Let's take a look:

#### Fluently creating a string column with an index:

	$table->string('email')->unique();

Pretty nice, huh? However, if defining the indexes on a separate line is more your style, here are example of using each of the index methods:

Command  | Description
------------- | -------------
`$table->primary('id');`  |  Adding a primary key
`$table->primary(array('fname', 'lname'));`  |  Adding composite keys
`$table->unique('email');`  |  Adding a unique index
`$table->fulltext('description');`  |  Adding a full-text index
`$table->index('state');`  |  Adding a basic index

<a name="dropping-indexes"></a>
## Dropping Indexes

Dropping indexes is just as easy as adding them. However, you must specify the name of the index when dropping them. Don't worry, Laravel assigns a reasonable name to all indexes. Simple concatenate the names of the columns in the index and append the type of the index. Let's take a look at some examples:

Command  | Description
------------- | -------------
`$table->drop_primary('id_primary');`  |  Dropping a primary key from the table
`$table->drop_unique('email_unique');`  |  Dropping a unique index from the table
`$table->drop_fulltext('description_fulltext');`  |  Dropping a full-text index from the table
`$table->drop_index('state_index');`  |  Dropping a basic index from the table