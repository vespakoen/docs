# Raw Queries

## Contents

- [Selects](#queries)
- [Inserts & Statements](#inserts-and-statements)
- [Updates & Deletes](#updates-and-deletes)
- [PDO Connections](#pdo-connections)

<a name="selects"></a>
## Selects

The **query** method is used for retrieving an array of database results. Each result is a PHP stdClass instance.

**Running a SELECT query on the database:**

	$users = DB::query('select * from users');

**Running a SELECT query with bindings:**

	$users = DB::query('select * from users where name = ?', array('test'));

**Running a SELECT query and returning the first result:**

	$user = DB::first('select * from users where id = 1');

**Running a SELECT query and getting the value of a single column:**

	$email = DB::only('select email from users where id = 1');

<a name="inserts-and-statements"></a>
## Inserts & Statements

The **statement** method returns the boolean result of the PDO operation, and may be used to insert records into the database:

**Inserting a record into the database**

	DB::statement('insert into users values (?, ?)', $bindings);

> **Note:** The **statement** method may also be used for ALTER and CREATE statements.

<a name="updates-and-deletes"></a>
## Updates & Deletes

**Updating table records and getting the number of affected rows:**

	DB::update('update users set name = ?', $bindings);

**Deleting from a table and getting the number of affected rows:**

	DB::delete('delete from users where id = ?', array(1));

<a name="pdo-connections"></a>
## PDO Connections

Sometimes you may wish to access the raw PDO connection behind the Laravel Connection object. It's simple:

**Get the raw PDO connection for a database:**

	$pdo = DB::connection('sqlite')->pdo;

> **Note:** If no connection name is specified, the **default** connection will be returned.