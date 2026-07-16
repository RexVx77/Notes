CH1: Introduction
# Welcome to Learn SQL

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/vuvJYcH-800x300.png)

Welcome to this comprehensive course on [SQL: **S**tructured **Q**uery **L**anguage](https://en.wikipedia.org/wiki/SQL)! Whether you're interested in working with PostgreSQL, MySQL, SQLite, or any other SQL database, this course will teach you the language you need to master them.

## Let's Build a Payment App

In this course, we'll work on the database for a make-believe PayPal-clone called _CashPal_! We'll write queries that interact with users, transactions, payouts, and so on.

# Select Single Column

Databases are made up of [tables](https://en.wikipedia.org/wiki/Table_\(database\)) which are made up of [columns](https://en.wikipedia.org/wiki/Column_\(database\)) (AKA "fields"). It's _just_ like an Excel spreadsheet.

Our CashPal database has a `users` table with these columns:

- `id` (integer)
- `name` (string)
- `age` (integer)
- `balance` (float)
- `is_admin` (boolean: true/false)

There can be many records (rows) in this table, each representing a single user. For example, here are three user rows:

|id|name|age|balance|is_admin|
|---|---|---|---|---|
|1|John Smith|28|450|1|
|2|Darren Walker|27|200|1|
|3|Jane Morris|33|496.24|0|

In the last lesson, we used the `*` wildcard to get _all_ the columns. To select only a _single_ column, we simply swap out the `*` for the name of the column.

**Returns _all_ columns from the `users` table:**

```sql
SELECT * FROM users;
```

**Returns _only_ the `name` column from the `users` table:**

```sql
SELECT name FROM users;
```
# Select Multiple Columns

As you probably guessed, if you can select _all_ columns with a `*`, and you can select a single column by name, you can probably also select multiple columns by name. Here's the syntax:

```sql
select column_one, column_two, column_three from table_name;
```

For example, if I had a table of monster records for a game, I might write:

```sql
select health, damage, defense from monsters;
```

All SQL statements must end with a semicolon `;`.

# What Is SQL?

Structured Query Language, or [SQL](https://en.wikipedia.org/wiki/SQL) (pronounced "squeel" by the in-crowd), is the primary programming language used to manage and interact with [relational databases](https://cloud.google.com/learn/what-is-a-relational-database). SQL can perform operations like **creating**, **updating**, **reading**, and **deleting** records within a database.

Click to hide video

Generally speaking, SQL is _extremely powerful_ and _programmable_. While spreadsheets are great for simple _manual_ data manipulation, SQL is designed for **automated** and **scalable** data operations. It can handle large datasets, complex queries, and integrate easily with general purpose programming languages like Python, TypeScript, and Go.

# Which Databases Use SQL?

SQL is just a query language. You typically use it to interact with a specific database technology. For example:

- [SQLite](https://www.sqlite.org/index.html)
- [PostgreSQL](https://www.postgresql.org/)
- [MySQL](https://www.mysql.com/)
- [CockroachDB](https://www.cockroachlabs.com/)
- [Oracle](https://www.oracle.com/database/)
- etc.

Although many different databases use the SQL _language_, most of them will have their own _dialect_. It's _critical_ to understand that _not_ all databases are created equal. Just because one SQL-compatible database does things a certain way, doesn't mean every SQL-compatible database will follow those exact same patterns.

## We're Using SQLite

In this course, we'll be using [SQLite](https://www.sqlite.org/index.html) specifically. SQLite is great for embedded projects, web browsers, and toy projects. It's lightweight, but has limited functionality compared to the likes of PostgreSQL or MySQL – two of the more common production SQL technologies.

We'll point out to you whenever some functionality we're working with is unique to SQLite!

One way in which SQLite is a bit different is that it stores Boolean values as integers – the integers `0` and `1`.

# NoSQL vs. SQL

When talking about SQL databases, we also have to mention the elephant in the room: [NoSQL](https://en.wikipedia.org/wiki/NoSQL).

To put it simply, a NoSQL database is a database that does _not_ use SQL (Structured Query Language). Each NoSQL database system typically has its own way of writing and executing queries. For example, [MongoDB](https://www.mongodb.com/) uses MQL (MongoDB Query Language), and [ElasticSearch](https://www.elastic.co/) simply has a JSON API.

While most relational databases are fairly similar, NoSQL databases tend to be fairly unique and are used for more niche purposes. Some of the main differences between SQL and NoSQL databases are:

1. NoSQL databases are usually non-relational; SQL databases are usually [relational](https://cloud.google.com/learn/what-is-a-relational-database) (we'll talk more about what this means later).
2. SQL databases usually have a defined schema; NoSQL databases usually have a dynamic schema.
3. SQL databases are table-based; NoSQL databases have a variety of different storage methods, such as document, key-value, graph, wide-column, and more.

Click to hide video

## Types of NoSQL Databases

- [Document Database](https://en.wikipedia.org/wiki/Document-oriented_database)
- [Key-Value Store](https://en.wikipedia.org/wiki/Key%E2%80%93value_database)
- [Wide-Column](https://en.wikipedia.org/wiki/Wide-column_store)
- [Graph](https://en.wikipedia.org/wiki/Graph_database)

A few of the most popular NoSQL databases are:

- [MongoDB](https://en.wikipedia.org/wiki/MongoDB)
- [Cassandra](https://en.wikipedia.org/wiki/Apache_Cassandra)
- [CouchDB](https://en.wikipedia.org/wiki/Apache_CouchDB)
- [DynamoDB](https://en.wikipedia.org/wiki/Amazon_DynamoDB)
- [ElasticSearch](https://www.elastic.co/)

# Comparing SQL Databases

Let's dive deeper and talk about some of the well-established SQL database systems and what makes them different from one another. The most popular SQL databases right now include:

- [PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL)
- [MySQL](https://en.wikipedia.org/wiki/MySQL)
- [Microsoft SQL Server](https://db-engines.com/en/system/Microsoft+SQL+Server)
- [SQLite](https://en.wikipedia.org/wiki/SQLite)
- [And many others](https://en.wikipedia.org/wiki/List_of_relational_database_management_systems)

_Source: [db-engines.com](https://db-engines.com/en/ranking)_

While all of these Databases use SQL, each database defines specific rules, practices, and strategies that separate them from their competitors.

## SQLite vs. PostgreSQL

Personally, SQLite and PostgreSQL are my favorites from the list above. Postgres is a very powerful, open-source, production-ready SQL database. SQLite is a lightweight, embeddable, open-source database. I usually choose one of these technologies if I'm doing SQL work.

SQLite is a serverless database management system (DBMS) that has the ability to run within applications, whereas PostgreSQL uses a client-server model and requires a server to be installed and listening on a network, similar to an HTTP server.

See a full [comparison here](https://db-engines.com/en/system/PostgreSQL%3BSQLite).

## We Use SQLite in This Course

In this course, we will be working with SQLite, a lightweight and simple database. For most backend web servers, PostgreSQL is a more production-ready option, but SQLite is great for learning and for small systems.

Let's take a look at how SQLite does _not_ enforce type-checking. Notice that within the `CREATE TABLE` statement, `name` is defined as a `TEXT` field.

1. [ ] Run the code and take a look at the results (don't submit yet!).
2. [ ] On line `3`, change the text string `'Montgomery Burns'` to the integer `1`, and run the code again.

Notice how even though we defined `name` as a `TEXT` field, SQLite allowed us to use an integer! Like Python and JavaScript, SQLite has a loose type system... You can store any type of data in any field, regardless of how you defined it. _Remember: just because you **can** do something, doesn't mean you **should**!_

To pass the assignment, **submit the code in the altered state**, where the record with an `id` of `2` has a `name` of `1`.

---

CH2: Tables

# Creating a Table

To create a new table in a database, use the `CREATE TABLE` statement followed by the name of the table and the fields you want in the table.

```sql
CREATE TABLE employees (id INTEGER, name TEXT, age INTEGER, is_manager BOOLEAN, salary INTEGER);
```

Each field name is followed by its _datatype_. We'll get to data types in a minute.

It's also acceptable and common to break up the `CREATE TABLE` statement with some whitespace like this:

```sql
CREATE TABLE employees(
  id INTEGER,
  name TEXT,
  age INTEGER,
  is_manager BOOLEAN,
  salary INTEGER
);
```

Use `INTEGER` instead of `INT` before submitting your code.

While the two notations are functionally identical, `INTEGER` is the fully standard-compliant keyword. We prefer `INTEGER` in this course for clarity, consistency, and better portability across SQL dialects.

If you're curious, the `PRAGMA TABLE_INFO(TABLENAME);` command in the test file returns information about a table and its fields.

# Altering Tables

We often need to alter our database schema without deleting it and re-creating it. Imagine if Twitter deleted its database each time it needed to add a feature, that would be a _disaster_! Your account and all your tweets would be wiped out on a daily basis.

Instead, we can use the `ALTER TABLE` statement to make changes in place without deleting any data.

## ALTER TABLE

With SQLite an `ALTER TABLE` statement allows you to:

### 1. Rename a Table or Column

```sql
ALTER TABLE employees
RENAME TO contractors;

ALTER TABLE contractors
RENAME COLUMN salary TO invoice;
```

### 2. Add or Drop a Column

```sql
ALTER TABLE contractors
ADD COLUMN job_title TEXT;

ALTER TABLE contractors
DROP COLUMN is_manager;
```

Unlike some SQL databases, SQLite does not support adding multiple columns in a single `ALTER TABLE` statement. Each column must be added in a separate `ALTER TABLE` command.

# Intro to Migrations

A database [migration](https://en.wikipedia.org/wiki/Schema_migration) is a change to the structure of a relational database. You can think of it like a commit in Git, but for your database schema. Every migration records how the structure of your data evolves over time.

For example, when we previously used an `ALTER TABLE` statement to add a new column, we were performing a **migration**.

Migrations are essential for adapting your database to changing requirements, fixing mistakes, and rolling out new features. In a team setting, migrations ensure everyone applies the same changes in the same order.

_Good_ migrations are small, incremental and ideally _reversible_ changes to a database. As you can imagine, when working with large databases, making changes can be scary! We have to be careful when writing database migrations so that we don't break any systems that depend on the old database schema.

Click to hide video

## Example of a Bad Migration

Let's say the CashPal backend runs this SQL regularly:

```sql
SELECT * FROM people;
```

If we rename the table from `people` to `users` in a migration but _forget to update the code_, this query will break because the `people` table no longer exists.

### The Right Approach

- Deploy the migration to rename the table.
- _Immediately_ deploy new code that uses a new query.

Migrations must be carefully coordinated with application changes to avoid outages.

# Up Migration

To manage migrations, we use a simple system based on **up** and **down** directions.

- The **up** migration applies changes to move your schema forward.
- The **down** migration rolls those changes back to the previous state.

This allows developers to safely move between versions of the schema during development and production rollouts.

# Down Migration

The migration we applied ended up causing trouble in production! It's possible the app wasn't ready for the new columns.

In situations like this, we need to **roll back** the changes safely using a **down** migration.

## Why Down Migrations Matter

Down migrations allow us to:

- **Undo changes** introduced by an up migration
- Quickly recover from bugs or compatibility issues in production
- Keep our schema consistent across environments (local, staging, production)

A well-written down migration should completely reverse the changes made in the up migration. In our case, that means removing the two columns we just added.

# Migration Review

Let's look at a more realistic migration that reflects a common evolution.

## Example

The `projects` table is being renamed to `initiatives` to better reflect how teams plan and track long-term work.

We also want to _record when each initiative officially launched_.

Up migration:

```sql
ALTER TABLE projects RENAME TO initiatives;

ALTER TABLE initiatives
ADD COLUMN launched_at TIMESTAMP;
```

Down migration:

```sql
ALTER TABLE initiatives DROP COLUMN launched_at;

ALTER TABLE initiatives RENAME TO projects;
```

This pair of migrations is **reversible** and **safe**. If something breaks, we can undo it.

## Real World Migration Tools

In real-world projects, we don't run raw SQL migrations. We use tools that help:

- Track which migrations have been applied.
- Organize migrations in files.
- Apply and roll back safely.

## Popular Tools

|Tool|Language|Notes|
|---|---|---|
|Goose|Go|Native Go tool|
|Flyway|Java, etc.|Simple file-based|
|Liquibase|Java|More config-heavy|
|Alembic|Python|For SQLAlchemy|
|Prisma Migrate|TypeScript|Works with Prisma ORM|
|Drizzle Kit|TypeScript|Works with Drizzle ORM|

## Example Workflow With a Tool

_This will vary according to the tool you use._

1. Write migration files.
    - `001_add_columns_to_transactions.up.sql`
    - `001_add_columns_to_transactions.down.sql`
2. Apply them using a CLI:
    
    ```sh
    migrate up
    ```
    
3. Your tool logs which migrations ran, and prevents duplicate migrations.

## Version Control for Your Schema

Migration files are committed like code. They travel with your project, so your teammates and [CI systems](https://en.wikipedia.org/wiki/Continuous_integration) always apply the same schema changes in the right order.

# SQL Data Types

SQL as a language can support many different data types. However, the types that _your_ [database management system](https://en.wikipedia.org/wiki/Database#:~:text=A%20database%20management%20system%20\(DBMS\)) (DBMS) supports will depend on which database you choose.

As for SQLite, it supports only the most _basic_ types, and SQLite is what we're using in this course!

## SQLite Data Types

Let's go over the [data types supported by SQLite](https://www.sqlite.org/datatype3.html) and how they're stored.

1. `NULL` – Null value.
2. `INTEGER` – Signed integer stored in 0, 1, 2, 3, 4, 6, or 8 bytes.
3. `REAL` – Floating-point value stored as a 64-bit [IEEE floating-point number](https://en.wikipedia.org/wiki/IEEE_754).
4. `TEXT` – Text string stored using the database encoding, most commonly [UTF-8](https://en.wikipedia.org/wiki/UTF-8).
5. `BLOB` – Short for [Binary Large Object](https://en.wikipedia.org/wiki/Object_storage) and typically used for images, audio, or other multimedia.
6. `BOOLEAN` – Boolean values are written in SQLite queries as `true` or `false`, but are recorded as `1` or `0`.

You may notice that we use the `REAL` data type in this course for some fields representing currency amounts. This is for simplicity.

In the real world, to avoid problems with floating-point math, the best practice is to use `INTEGER` for currency amounts. The value then represents the smallest denomination. For example, $42.67 would be stored as `4267` (cents).

## Boolean Values

It's important to note, SQLite does not have a separate `BOOLEAN` storage class. Instead, boolean values are stored as integers:

- `0` = `false`
- `1` = `true`

_It's not actually all that weird – boolean values are just binary bits after all!_

SQLite will let you write your queries using `boolean` expressions and `true`/`false` keywords, but it will convert the booleans to integers under-the-hood.

---

CH3: Constraints

# Null Values

In SQL, a cell with a `NULL` value indicates that the value is _missing_. A `NULL` value is _very_ different from a _zero_ value.

## Constraints

When creating a table, we can define whether or not a field _can_ or _cannot_ be `NULL`, and that's a kind of `constraint`.

# Constraints

A `constraint` is a rule we create on a database that _enforces_ some specific behavior. For example, setting a `NOT NULL` constraint on a column ensures that the column will not accept `NULL` values.

If we try to insert a `NULL` value into a column with the `NOT NULL` constraint, the insert will fail with an error message. Constraints are extremely useful when we need to _ensure_ that certain kinds of data exist within our database.

## Defining a NOT NULL Constraint

The `NOT NULL` constraint can be added directly to the `CREATE TABLE` statement.

```sql
CREATE TABLE employees(
  id INTEGER PRIMARY KEY,
  -- The PRIMARY KEY constraint uniquely identifies each row in the table
  name TEXT UNIQUE,
  -- The UNIQUE constraint ensures that no two rows can have the same value in the 'name' column
  title TEXT NOT NULL
  -- The NOT NULL constraint ensures that the 'title' column cannot have NULL values
);
```

## SQLite Limitation

In other dialects of SQL you can `ADD CONSTRAINT` within an `ALTER TABLE` statement. SQLite does _not_ support this feature so when we create our tables we need to make sure we specify all the constraints we want! Here's a [list of SQL Features](https://www.sqlite.org/omitted.html) SQLite does not implement in case you're curious.

# Primary Keys

A _key_ defines and protects relationships between tables. A [primary key](https://en.wikipedia.org/wiki/Primary_key) is a special column that _uniquely_ identifies records within a table. Each table can have one, and only one primary key.

## The Primary Key Is Usually an ID

It's _very_ common to have a column named `id` on each table in a database, and that `id` is the primary key for that table. No two rows in that table can share an `id`.

A `PRIMARY KEY` constraint can be explicitly specified on a column to ensure uniqueness, rejecting any inserts where you attempt to create a duplicate ID.

# Foreign Keys

Foreign keys are what make relational databases relational! Foreign keys define the relationships _between_ tables. Simply put, a `FOREIGN KEY` is a field in one table that references another table's `PRIMARY KEY`.

## Creating a Foreign Key in SQLite

Creating a `FOREIGN KEY` in SQLite happens at table creation! After we define the table fields and constraints we add a named `CONSTRAINT` where we define the `FOREIGN KEY` column and its `REFERENCES`.

Here's an example:

```sql
CREATE TABLE departments (
  id INTEGER PRIMARY KEY,
  department_name TEXT NOT NULL
);

CREATE TABLE employees (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  department_id INTEGER,
  CONSTRAINT fk_departments
    FOREIGN KEY (department_id)
    REFERENCES departments(id)
);
```

In this example, an `employee` has a `department_id`. The `department_id` must be the same as the `id` field of a record from the `departments` table. `fk_departments` is the specified name of the [constraint](https://www.sqlite.org/lang_createtable.html#constraint_enforcement).

1. `CONSTRAINT fk_departments`: create a constraint called `fk_departments`
2. `FOREIGN KEY (department_id)`: make this constraint a foreign key assigned to the `department_id` field
3. `REFERENCES departments(id)`: link the foreign field `id` from the `departments` table

Another syntax: Creating a foreign key without the `CONSTRAINT` keyword means the name of the constraint is auto-assigned.
```sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  age INTEGER,
  country_code TEXT NOT NULL,
  username TEXT ,
  password TEXT,
  is_admin BOOLEAN,
  FOREIGN KEY (country_code)
  REFERENCES countries(code)
);

```

# Schema

We've used the word _schema_ a few times now; let's talk about what it means. A database's [schema](https://www.ibm.com/think/topics/database-schema) describes how data is organized within it.

Data types, table names, field names, constraints, and the relationships between all of those entities are part of a database's schema.

## There Is No Perfect Schema

When designing a database schema, there typically isn't a "correct" solution. We do our best to choose a reasonable set of tables, fields, constraints, etc. that will accomplish our project's goals.

Like many things in programming, different schema designs come with different trade-offs.

## How to Decide on a Sane Schema

Let's use CashPal as an example. One important decision that needs to be made is which table will store a user's balance! As you can imagine, ensuring our data is accurate when dealing with money is _critical_. We want to be able to:

- Keep track of a user's current balance
- See the historical balance at any point in the past
- See a log of which transactions changed the balance over time

# Relational Databases

We have been using the term _relational_ quite a bit. It's time we actually go over what that means!

A _relational_ database is a type of database that stores data so that it can be easily related to other data. For example, a `user` can have many `tweets`. There's a relationship between a `user` and their `tweet`.

In a relational database:

1. Data is typically represented in "tables."
2. Each table has "columns" or "fields" that hold attributes related to the record.
3. Each row or entry in the table is called a [record](https://en.wikipedia.org/wiki/Record_\(computer_science\)).
4. Typically, each record has a unique `Id` called the [primary key](https://en.wikipedia.org/wiki/Primary_key).

## Example Relational Database

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/ZESnmkp-1145x720.png)

Here is an example of a small relational database. This database has 3 tables, `Students`, `Courses`, and `StudentCourses`. The `StudentCourses` table manages the relationship between the `Students` and `Courses` tables.

## Example 1: Curly

- Curly has an `Id` of `2`.
- We can find Curly's courses by looking in the `StudentCourses` table for the records that match his `StudentId`.

## Example 2: Haskell Monads

- "Haskell Monads" has an `Id` of `3`.
- We can find all the students enrolled in the Haskell Monads course by checking the `CourseId` column in the `StudentCourses` table.

# Relational vs. Non-Relational DBs

The big difference between relational and non-relational databases is that non-relational databases _nest_ their data. Instead of keeping records in separate tables, they store records _within other records_.

To over-simplify it, you can think of non-relational databases as giant JSON blobs. If a user can have multiple courses, you might just add all the courses to the user record.

```json
{
  "users": [
    {
      "id": 0,
      "name": "Elon",
      "courses": [
        {
          "name": "Biology",
          "id": 0
        },
        {
          "name": "Biology",
          "id": 0
        }
      ]
    }
  ]
}
```

This often results in _duplicate data_ within the database. That's obviously less than ideal, but it does have some benefits that we'll talk about later in the course.

## Relational Database

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/ZESnmkp-1145x720.png)

## Non-Relational Database

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/nyG4JBN-1033x720.png)

---

CH4: CRUD

# CRUD

CRUD is an acronym that stands for `CREATE`, `READ`, `UPDATE`, and `DELETE`. These four operations are the bread and butter of nearly every database you will create.

## HTTP and CRUD

The CRUD operations correlate nicely with the HTTP methods we learned in the [Learn HTTP Clients](https://www.boot.dev/courses/learn-http-clients-golang) course.

- `HTTP POST` – `CREATE`
- `HTTP GET` – `READ`
- `HTTP PUT` – `UPDATE`
- `HTTP DELETE` – `DELETE`

# Insert Statement

Tables are pretty useless without data in them! In SQL we can add records to a table using an `INSERT INTO` statement. When using an `INSERT` statement we must first specify the `table` we are inserting the record into, followed by the `fields` within that table we want to add `VALUES` to.

Example `INSERT INTO` statement:

```sql
INSERT INTO employees(id, name, title)
VALUES (1, 'Allan', 'Engineer');
```

# HTTP CRUD Database Lifecycle

It's important to understand how data _flows_ through a typical web application.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/WoHxHvG-1280x460.png)

1. The frontend processes some data from user input – maybe a form is submitted.
2. The frontend sends that data to the server through an HTTP request – maybe a `POST`.
3. The server makes a SQL query to its database to create an associated record – probably using an `INSERT` statement.
4. Once the server has determined that the DB query was successful, it responds to the frontend with a status code. Hopefully a `200`-level code (success)!

# Auto Increment

Many dialects of SQL support an `AUTO INCREMENT` feature. When inserting records into a table with `AUTO INCREMENT` enabled, the database will assign the next value _automatically_. In SQLite, an integer `id` field that has the `PRIMARY KEY` constraint will auto-increment by default!

## IDs

Depending on how your database is set up, you may be using traditional `id`s or you may be using [UUIDs](https://en.wikipedia.org/wiki/Universally_unique_identifier). SQL doesn't support auto-incrementing a `uuid`, so if your database is using them your server will have to handle the changing `uuid`s for each record.

## Using `AUTO INCREMENT` in SQLite

We are using traditional `id`s in our database, so we can take advantage of the auto-increment feature. Different dialects of SQL will implement this feature differently, but in SQLite any column that has the `INTEGER PRIMARY KEY` constraint will auto-increment! So we can omit the `id` field within the `INSERT` statement and allow the database to automatically add that field for us!

# Manual Entry

Manually `INSERT`ing every single record in a database would be an _extremely_ time-consuming task! Working with raw SQL as we are now is _not_ super common when designing backend systems.

When working with SQL within a software system, like a backend web application, you'll typically have access to a programming language such as Go or Python. For example, a backend server written in Go can use string concatenation to dynamically create SQL statements, and that's usually how it's done!

```go
sqlQuery := fmt.Sprintf(`
INSERT INTO users(name, age, country_code)
VALUES ('%s', %v, '%s');
`, user.Name, user.Age, user.CountryCode)
```

## SQL Injection

The example above is an oversimplification of what _really_ happens when you access a database using Go code. In essence, it's correct. String interpolation is how production systems access databases. That said, it must be done _carefully_ to not be a [security vulnerability](https://en.wikipedia.org/wiki/SQL_injection). We'll talk more about that later!

# Count

We can use a `SELECT` statement to get a _count_ of the records within a table. This can be very useful when we need to know _how many_ records there are, but we don't particularly care what's in them.

Here's an example in SQLite:

```sql
SELECT COUNT(*) FROM employees;
```

The `*` in this case refers to a column name. We don't care about the count of a _specific column_ – we want to know the number of _total records_, so we can use the wildcard (`*`).

# HTTP CRUD Database Lifecycle

We talked about how a "create" operation flows through a web application. Let's talk about a "read."

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/Z9G8kY8-1280x435.png)

We'll use an example scenario from the CashPal app. Our product manager wants to show profile data on a user's settings page. Here's how we could engineer that feature request:

1. First, the front-end webpage loads.
2. The front-end sends an HTTP `GET` request to a `/users` endpoint on the back-end server.
3. The server receives the request.
4. The server uses a `SELECT` statement to retrieve the user's record from the `users` table in the database.
5. The server converts the row of SQL data into a JSON object and sends it back to the front-end.

# WHERE Clause

In order to keep learning about CRUD operations in SQL, we need to learn how to make the instructions we send to the database more specific. SQL accepts a `WHERE` statement within a query that allows us to be very specific with our instructions.

If we were unable to specify the record we wanted to `READ`, `UPDATE`, or `DELETE` making queries to a database would be very frustrating, and very inefficient.

## Using a WHERE Clause

Say we had _over 9000_ records in our `users` table. We often want to look at specific user data within that table without retrieving _all_ the other records in the table. We can use a `SELECT` statement followed by a `WHERE` clause to specify which records to retrieve. The `SELECT` statement stays the same, we just _add_ the `WHERE` clause to the end of the `SELECT`. Here's an example:

```sql
SELECT name FROM users WHERE power_level >= 9000;
```

This will select only the `name` field of any user within the `users` table `WHERE` the `power_level` field is greater than or equal to `9000`.

# Finding NULL Values

You can use a `WHERE` clause to filter values by whether or not they're `NULL`.

## IS NULL

```sql
SELECT name FROM users WHERE first_name IS NULL;
```
## IS NOT NULL

```sql
SELECT name FROM users WHERE first_name IS NOT NULL;
```

# DELETE

When a user deletes their account on Twitter, or deletes a comment on a YouTube video, that data needs to be removed from its respective database.

## DELETE Statement

A `DELETE` statement removes all records from a table that match the `WHERE` clause. As an example:

```sql
DELETE FROM employees
WHERE id = 251;
```

This `DELETE` statement removes all records from the `employees` table that have an id of `251`!

# Danger of Deleting Data

Deleting data can be a dangerous operation. Once removed, data can be really hard if not _impossible_ to restore! Let's talk about a couple of common ways back-end engineers protect against losing valuable customer data.

Click to hide video

## Strategy 1 – Backups

If you're using a cloud-service like GCP's [Cloud SQL](https://cloud.google.com/sql) or AWS's [RDS](https://aws.amazon.com/rds/) you should _always_ turn on automated backups. They take an automatic snapshot of your entire database on some interval, and keep it around for some length of time.

The Boot.dev database has a backup snapshot taken daily, and we retain those backups for 30 days. If I ever accidentally run a query that deletes valuable data, I can restore it from the backup.

**You should have a backup strategy for production databases.**

## Strategy 2 – Soft Deletes

A "soft delete" is when you don't _actually_ delete data from your database, but instead just "mark" the data as deleted. For example, you might set a `deleted_at` date on the row you want to delete. Then, in your queries you ignore anything that has a `deleted_at` date set. The idea is that this allows your application to behave as if it's deleting data, but you can always go back and restore any data that's been removed.

You should probably only soft-delete if you have a specific reason to do so. Automated backups should be "good enough" for most applications that are just interested in protecting against developer mistakes.

# Update Query in SQL

Whenever you update your profile picture or change your password online, you are changing the data in a field on a table in a database! Imagine if every time you accidentally messed up a tweet on Twitter, you had to delete the entire tweet and post a new one instead of just editing it...

... well, that's a bad example.

## Update Statement

The `UPDATE` statement in SQL allows us to update the fields of a record. We can even update many records depending on how we write the statement.

An `UPDATE` statement specifies the table that needs to be updated, followed by the fields and their new values by using the `SET` keyword. Lastly a `WHERE` clause indicates the record(s) to update.

```sql
UPDATE employees
SET job_title = 'Backend Engineer', salary = 150000
WHERE id = 251;
```

# Object-Relational Mapping (ORMs)

An [Object-Relational Mapping](https://en.wikipedia.org/wiki/Object%E2%80%93relational_mapping) or an _ORM_ for short, is a tool that allows you to perform CRUD operations on a database using a traditional programming language. These typically come in the form of a library or framework that you would use in your backend code.

The primary benefit an ORM provides is that it maps your database records to in-memory objects. For example, in Go we might have a struct that we use in our code:

```go
type User struct {
    ID int
    Name string
    IsAdmin bool
}
```

This struct definition conveniently represents a database table called `users`, and an _instance_ of the struct represents a row in the table.

## Example: Using an ORM

Using an ORM we might be able to write simple code like this:

```go
user := User{
    ID: 10,
    Name: "Lane",
    IsAdmin: false,
}

// generates a SQL statement and runs it,
// creating a new record in the users table
db.Create(user)
```
## Example: Using Straight SQL

Using straight SQL we typically need to write more code and handle things a bit more manually:

```go
user := User{
    ID: 10,
    Name: "Lane",
    IsAdmin: false,
}

db.Exec("INSERT INTO users (id, name, is_admin) VALUES (?, ?, ?);",
    user.ID, user.Name, user.IsAdmin)
```

## Should You Use an ORM?

That depends! An ORM typically _trades control for simplicity_.

Using straight SQL you can take full advantage of the power of the SQL language. Using an ORM, you're limited by whatever functionality the ORM has. If you run into issues with a specific query, it can be harder to debug with an ORM because you have to dig through the framework's code and documentation to figure out how the underlying queries are being generated.

I recommend doing projects _both ways_ so that you can learn about the trade-offs. At the end of the day, when you're working on a team of developers it will be a team decision.

---

CH5: Basic Queries

# AS Clause in SQL

Sometimes we need to structure the data we return from our queries in a specific way. An `AS` clause allows us to "alias" a piece of data in our query. The alias exists only for the duration of the query.

## AS Keyword

The following queries return the same data:

```sql
SELECT employee_id AS id, employee_name AS name
FROM employees;
```

```sql
SELECT employee_id, employee_name
FROM employees;
```

The difference is that the results from the aliased query would have column names `id` and `name` instead of `employee_id` and `employee_name`.

# SQL Functions

SQL is a programming language, and like nearly all programming languages, it supports functions. We can use functions and aliases to _calculate_ new columns in a query. This is similar to how you might use formulas in Excel.

A calculated column is a new column that doesn't exist in the original table but is created on the fly when you run a query.

## The IIF Function

In SQLite, the `IIF` function works like a [ternary expression](https://book.pythontips.com/en/latest/ternary_operators.html). For example:

```sql
IIF(carA > carB, 'Car A is bigger', 'Car B is bigger')
```

If `carA` is greater than `carB`, this statement evaluates to the string `'Car A is bigger'`. Otherwise, it evaluates to `'Car B is bigger'`.

Here's how we can use `IIF()` and a `directive` alias to add a new calculated column to our result set:

```sql
SELECT quantity,
  IIF(quantity < 10, 'Order more', 'In Stock') AS directive
FROM products;
```

# Between

We can check if values are `between` two numbers using the `WHERE` clause in an intuitive way! The `WHERE` clause doesn't always have to be used to specify specific IDs or values. We can also use it to help narrow down our result set. Here's an example:

```sql
SELECT employee_name, salary
FROM employees
WHERE salary BETWEEN 30000 AND 60000;
```

This query returns all the employees `name` and `salary` fields for any rows where the `salary` is `BETWEEN` 30,000 and 60,000 inclusively! We can also query results that are `NOT BETWEEN` two specified values.

```sql
SELECT product_name, quantity
FROM products
WHERE quantity NOT BETWEEN 20 AND 100;
```

This query returns all the product names and quantities where the quantity was not between `20` and `100` (meaning it excludes `20` and `100`). We can use conditionals to make the results of our query as specific as we need them to be.

# Distinct

Sometimes we want to retrieve records from a table without getting back any duplicates.

For example, we may want to know all the different companies our employees have worked at previously, but we don't want to see the same company multiple times in the report.

## SELECT DISTINCT

SQL offers us the `DISTINCT` keyword that removes duplicate records from the resulting query.

```sql
SELECT DISTINCT previous_company
FROM employees;
```

This only returns one row for each unique `previous_company` value.

# Logical Operators – AND

We often need to use _multiple_ conditions to retrieve the exact information we want. We can begin to structure much more complex queries by using multiple conditions together to narrow down the search results of our query.

The logical `AND` operator can be used to narrow down our result sets even more!

## AND Operator

```sql
SELECT product_name, quantity, shipment_status
FROM products
WHERE shipment_status = 'pending'
  AND quantity BETWEEN 0 and 10;
```

This only retrieves records where _both_ the `shipment_status` is "pending" _AND_ the `quantity` is between `0` and `10`.

## Comparison Operators

All of the following operators are supported in SQL. The `=` is the main one to watch out for – it's not `==` like in many other languages! SQLite does allow for `==`, but it's not a good habit to get into, as other dialects of SQL will not recognize `==` as valid syntax.

- `=`
- `<`
- `>`
- `<=`
- `>=`
- `<>` or `!=`

# OR

As you've probably guessed, if the logical `AND` operator is supported, the `OR` operator is probably supported as well.

```sql
SELECT product_name, quantity, shipment_status
FROM products
WHERE shipment_status = 'out of stock'
  OR quantity BETWEEN 10 and 100;
```

This query retrieves records where _either_ the `shipment_status` condition _or_ the `quantity` condition is met.

## Order of Operations

You can group logical operations with parentheses to specify the [order of operations](https://www.mathsisfun.com/operation-order-pemdas.html).

```sql
(this AND that) OR the_other
```

# In

Another variation to the `WHERE` clause we can utilize is the `IN` operator. `IN` returns `true` or `false` if the first operand matches _any_ of the values in the second operand. The `IN` operator is a shorthand for multiple `OR` conditions.

These two queries are equivalent:

```sql
SELECT product_name, shipment_status
FROM products
WHERE shipment_status IN ('shipped', 'preparing', 'out of stock');
```

```sql
SELECT product_name, shipment_status
FROM products
WHERE shipment_status = 'shipped'
  OR shipment_status = 'preparing'
  OR shipment_status = 'out of stock';
```

Hopefully, you're starting to see how querying specific data using fine-tuned SQL clauses helps reveal important insights! The larger a table becomes, the harder it becomes to analyze without proper queries.

# Like

Sometimes we don't have the luxury of knowing _exactly_ what it is we need to query. Have you ever wanted to look up a song or a video but you only remember _part_ of the name? SQL offers us an option for when we're in situations `LIKE` this.

The `LIKE` keyword allows for the use of the `%` and `_` wildcard operators. Let's focus on `%` first.

## % Operator

The `%` operator will match zero or more characters. We can use this operator within our query string to find more than just exact matches, depending on where we place it.

## Product Starts With “banana”

```sql
SELECT * FROM products
WHERE product_name LIKE 'banana%';
```
## Product Ends With “banana”

```sql
SELECT * FROM products
WHERE product_name LIKE '%banana';
```
## Product Contains “banana”

```sql
SELECT * FROM products
WHERE product_name LIKE '%banana%';
```

## Tip

The `LIKE` operator expects a string value. Make sure the statement you are comparing against is wrapped in quotes, or SQL will think you're referring to a column!

# Underscore Operator

As discussed, the `%` wildcard operator matches _zero or more_ characters. The `_` wildcard operator, on the other hand, matches only a _single_ character.

```sql
SELECT * FROM products
WHERE product_name LIKE '_oot';
```

The query above matches products like:

- boot
- root
- foot

```sql
SELECT * FROM products
WHERE product_name LIKE '__oot';
```

The query above matches products like:

- shoot
- groot

---

CH6: Structuring

# LIMIT

Sometimes we don't want to retrieve _every_ record from a table. For example, it's common for a production database table to have millions of rows, and `SELECT`ing all of them might crash your system! _The `LIMIT` keyword has entered the chat._

`LIMIT` can be used at the end of a `SELECT` statement to set a cap on the number of records returned.

```sql
SELECT * FROM products
WHERE product_name LIKE '%berry%'
LIMIT 50;
```

The query above retrieves all the records from the `products` table where the name contains the word _berry_. If we ran this query on the Amazon database, it would almost certainly return a _lot_ of records.

The `LIMIT` clause only allows the database to return _up to_ 50 records matching the query. This means that if there aren't that many records matching the query, `LIMIT` will not have an effect.

# ORDER BY

SQL also offers us the ability to sort the results of a query using `ORDER BY`. By default, the `ORDER BY` keyword sorts records by the given field in ascending order, or `ASC` for short. However, `ORDER BY` does support descending order as well with the keyword `DESC`.

## Examples

This query returns the `name`, `price`, and `quantity` fields from the `products` table sorted by `price` in _ascending_ order:

```sql
SELECT name, price, quantity FROM products
ORDER BY price;
```

This query returns the `name`, `price`, and `quantity` of the products ordered by `quantity` in _descending_ order:

```sql
SELECT name, price, quantity FROM products
ORDER BY quantity DESC;
```

# ORDER BY and LIMIT

When using both `ORDER BY` and `LIMIT`, the `ORDER BY` clause must come _first_.

---

CH7: Aggregations

# What Are Aggregations?

An "aggregation" is a _single_ value that's derived by combining _several_ other values. We performed an aggregation earlier when we used the `COUNT` statement to count the number of records in a table.

## Why Aggregations?

Data stored in a database should generally be stored [raw](https://wagslane.dev/posts/keep-your-data-raw-at-rest/). When we need to calculate some additional data from the raw data, we can use an _aggregation_.

Take the following `COUNT` aggregation as an example:

```sql
SELECT COUNT(*)
FROM products
WHERE quantity = 0;
```

This query returns the number of products that have a `quantity` of `0`. We _could_ store a count of the products in a separate database table, and increment/decrement it whenever we make changes to the `products` table – but that would be _redundant_.

It's _much simpler_ to store the products in a single place (we call this a [single source of truth](https://en.wikipedia.org/wiki/Single_source_of_truth)) and run an aggregation when we need to derive additional information from the raw data.

# SUM

The `SUM` aggregation function returns the sum of a set of values.

For example, the query below returns a single record containing a single field. The returned value is equal to the _total_ salary being collected by all of the `employees` in the `employees` table.

```sql
SELECT SUM(salary)
FROM employees;
```

Which returns:

| SUM(SALARY) |
| ----------- |
| 2483        |

# MAX

As you might expect, the `MAX` function retrieves the _largest_ value from a set of values. For example:

```sql
SELECT MAX(price)
FROM products;
```

This query looks through all the rows in the `products` table and returns the largest `price` value. Remember, it only returns the `price`, not the rest of the record! You always need to specify each field you want a query to return.

# MIN

The `MIN` function works the same as the `MAX` function but finds the _lowest_ value instead of the _highest_ value.

```sql
SELECT product_name, MIN(price)
FROM products;
```

This query returns the `product_name` and the `price` fields of the record with the lowest `price`.

# GROUP BY

There are times when we need to group data based on specific values.

SQL offers the `GROUP BY` clause, which can group rows that have similar values into "summary" rows. It returns one row for each group. The interesting part is that each group can have an aggregation function applied to it that operates only on the grouped data.

## Example of GROUP BY

Imagine that we have a database with songs and albums:

|song_id|title|album_id|
|---|---|---|
|1|Crawl|10|
|2|Oakland|10|
|3|Bonfire|11|
|4|Fire Fly|11|
|5|Heartbeat|11|
|6|Sober|12|

If we want to see how many songs are on each album, we can use a query like this:

```sql
SELECT album_id, COUNT(song_id) AS song_count
FROM songs
GROUP BY album_id;
```

This query retrieves a count of all the songs on each album. One record is returned per album, and they each have their own `count`:

|album_id|song_count|
|---|---|
|10|2|
|11|3|
|12|1|

# Average

Just like we may want to find the minimum or maximum values within a dataset, sometimes we need to know the [average](https://en.wikipedia.org/wiki/Arithmetic_mean)!

SQL offers us the `AVG()` function. Similar to `MAX()`, `AVG()` calculates the average of all non-`NULL` values.

```sql
SELECT AVG(song_length)
FROM songs;
```

This query returns the average `song_length` in the `songs` table.

# HAVING

When we need to filter the results of a `GROUP BY` query even further, we can use the `HAVING` clause. `HAVING` specifies a search condition for a group.

The `HAVING` clause is similar to the `WHERE` clause, but it operates on groups _after_ they've been grouped, rather than rows _before_ they've been grouped.

```sql
SELECT album_id, COUNT(id) AS count
FROM songs
GROUP BY album_id
HAVING COUNT(id) > 5;
```

This query returns the `album_id` and count of its songs, but only for albums with more than `5` songs.

# HAVING vs. WHERE in SQL

It's common for developers to get confused about the difference between the `HAVING` and `WHERE` clauses – they're pretty similar after all.

The difference, though, is straightforward enough:

- A `WHERE` condition is applied to _all_ the data in a query _before_ it's grouped by a `GROUP BY` clause.
- A `HAVING` condition is applied only to the _grouped rows_ that are returned _after_ a `GROUP BY` is applied.

This means that if you want to filter based on the result of an aggregation, you need to use `HAVING`. If you want to filter on a value that's present in the raw data, you should use a simple `WHERE` clause.

# ROUND

Sometimes we need to [round](https://en.wikipedia.org/wiki/Rounding) some numbers, particularly when working with the results of an aggregation. We can use the `ROUND()` function to get the job done.

The SQL `ROUND()` function allows you to specify both the value you wish to round and the degree of precision to be applied:

```sql
ROUND(value, precision)
```

If no precision is given, SQL will round the value to the nearest _whole_ value:

```sql
SELECT ROUND(AVG(song_length))
FROM songs;
```

This query returns the average `song_length` from the `songs` table, rounded to the nearest whole number.

If we _do_ provide a precision, SQL will round to that many decimal places:

```sql
SELECT ROUND(AVG(song_length), 1)
FROM songs;
```

The same query, but rounded to a single decimal place.

---

CH8: Subqueries

# Subqueries

Sometimes a single query is not enough to retrieve the specific records we need.

It is possible to run a query on the _result set_ of another query – a query within a query! This is called "query-ception"... erm... I mean a "subquery."

Subqueries can be very useful in a number of situations when trying to retrieve specific data that wouldn't be accessible by simply querying a single table.

## Querying Multiple Tables

Here is an example of a subquery:

```sql
SELECT id, song_name, artist_id
FROM songs
WHERE artist_id IN (
  SELECT id
  FROM artists
  WHERE artist_name LIKE 'Rick%'
);
```

In this hypothetical database, the query above selects all of the `id`s, `song_name`s, and `artist_id`s from the `songs` table that are written by artists whose name starts with `Rick`. Notice that the subquery allows us to use information from a different table – in this case the `artists` table.

## Subquery Syntax

The only syntax unique to a subquery is the parentheses surrounding the nested query. The `IN` operator could be different; for example, we could use the `=` operator if we expect a single value to be returned.

# Subqueries

Sometimes a single query is not enough to retrieve the specific records we need.

It is possible to run a query on the _result set_ of another query – a query within a query! This is called "query-ception"... erm... I mean a "subquery."

Subqueries can be very useful in a number of situations when trying to retrieve specific data that wouldn't be accessible by simply querying a single table.

## Querying Multiple Tables

Here is an example of a subquery:

```sql
SELECT id, song_name, artist_id
FROM songs
WHERE artist_id IN (
  SELECT id
  FROM artists
  WHERE artist_name LIKE 'Rick%'
);
```

In this hypothetical database, the query above selects all of the `id`s, `song_name`s, and `artist_id`s from the `songs` table that are written by artists whose name starts with `Rick`. Notice that the subquery allows us to use information from a different table – in this case the `artists` table.

## Subquery Syntax

The only syntax unique to a subquery is the parentheses surrounding the nested query. The `IN` operator could be different; for example, we could use the `=` operator if we expect a single value to be returned.

# No Tables

When working on a back-end application, this doesn't come up often, but it's important to remember that **SQL is a full programming language**. We usually use it to interact with data stored in tables, but it's quite flexible and powerful.

For example, you can `SELECT` information that's simply calculated, with no tables necessary.

```sql
SELECT 5 + 10 AS sum;
-- 15
```

---

CH9: Normalization

# Table Relationships

Relational databases are powerful because of the relationships between the tables. These relationships help us to keep our databases clean and efficient. A relationship between tables assumes that one of these tables has a `foreign key` that references the `primary key` of another table.

Click to hide video

## Types of Relationships

There are 3 primary types of relationships in a relational database:

1. One-to-one
2. One-to-many
3. Many-to-many

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/u4i6XdL-763x340.png)

## One-to-One

A `one-to-one` relationship most often manifests as a field or set of fields on a row in a table. For example, a `user` will have exactly one `password`.

Settings fields might be another example of a one-to-one relationship. A user will have exactly one `email_preference` and exactly one `birthday`.

# One to Many

When talking about the relationships between _tables_, a one-to-many relationship is probably the most commonly used relationship.

A one-to-many relationship occurs when a single record in one table is related to potentially many records in another table.

The one → many relation only goes **one way**; a record in the second table **cannot** be related to multiple records in the first table!

## Examples

- A `customers` table and an `orders` table. Each customer has `0`, `1`, or many orders that they've placed.
- A `users` table and a `transactions` table. Each `user` has `0`, `1`, or many transactions that they've taken part in.

```sql
CREATE TABLE customers (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL
);

CREATE TABLE orders (
  id INTEGER PRIMARY KEY,
  amount REAL NOT NULL,
  customer_id INTEGER,
  CONSTRAINT fk_customers
    FOREIGN KEY (customer_id)
    REFERENCES customers(id)
);
```

# Many to Many

A many-to-many relationship occurs when multiple records in one table can be related to multiple records in another table.

## Examples

- A `products` table and a `suppliers` table – Products may have `0` to many suppliers, and suppliers can supply `0` to many products.
- A `classes` table and a `students` table – Students can take potentially many classes and classes can have many students enrolled.

## Joining Table

Joining tables help define many-to-many relationships among data in a database. As an example, when defining the relationship above between products and suppliers, we would define a joining table called `products_suppliers` that contains the primary keys from the tables to be joined.

Then, when we want to see if a supplier offers a specific product, we can look in the joining table to see if the IDs share a row.

## Unique Constraint Across Two Fields

When enforcing specific schema constraints, we may need to enforce the `UNIQUE` constraint across two different fields.

```sql
CREATE TABLE product_suppliers (
  product_id INTEGER,
  supplier_id INTEGER,
  UNIQUE(product_id, supplier_id),
  FOREIGN KEY (product_id) REFERENCES products (id),
  FOREIGN KEY (supplier_id) REFERENCES suppliers (id)
);
```

This lets multiple rows share the same `product_id` **or** `supplier_id`, but it prevents any two rows from having both the same `product_id` **and** `supplier_id`.

# Database Normalization

Database normalization is a method for structuring your database schema in a way that helps:

- Improve data integrity
- Reduce data redundancy

Click to hide video

## What Is Data Integrity?

"Data integrity" refers to the accuracy and consistency of data. For example, if a user's _age_ is stored in a database, rather than their _birthday_, that data becomes incorrect automatically with the passage of time.

It would be better to _store_ a birthday and _calculate_ the age as needed.

## What Is Data Redundancy?

"Data redundancy" occurs when the same piece of data is stored in multiple places. For example: saving the same file multiple times to different hard drives.

Data redundancy can be problematic, especially when data in one place is changed such that the data is no longer consistent across all copies of that data.

# Normal Forms

The creator of "database normalization," [Edgar F. Codd](https://en.wikipedia.org/wiki/Edgar_F._Codd) described different "normal forms" a database can adhere to. We'll talk about the most common ones.

- First normal form (1NF)
- Second normal form (2NF)
- Third normal form (3NF)
- Boyce-Codd normal form (BCNF)

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/nBWCyHA-938x720.png)

In short, first normal form is the _least_ "normalized" form, and Boyce-Codd is the _most_ "normalized" form.

The more normalized a database, the better its data integrity, and the less duplicate data you'll have.

## “Primary Key” in Normal Forms

In the context of database normalization, we're going to use the term "primary key" slightly differently. When we're talking about SQLite, a "primary key" is a single column that uniquely identifies a row.

When we're talking more generally about data normalization, the term "primary key" means the _collection_ of columns that uniquely identify a row. That _can be_ a single column, but it can actually be any number of columns that form a [composite key](https://en.wikipedia.org/wiki/Composite_key). A primary key is the minimum number of columns needed to uniquely identify a row in a table.

If you think back to the many-to-many joining table `product_suppliers`, that table's "primary key" was actually a _combination_ of the two IDs, `product_id` and `supplier_id`:

```sql
CREATE TABLE product_suppliers (
  product_id INTEGER,
  supplier_id INTEGER,
  UNIQUE(product_id, supplier_id)
);
```

# First Normal Form (1NF)

To be compliant with [first normal form](https://en.wikipedia.org/wiki/First_normal_form) (1NF for short), a database table simply needs to follow two rules:

- It must have a unique primary key.
- A cell can't have a nested table as its value (depending on the database system you're using, this may not even be _possible_).

## Example of _Not_ 1NF

|name|age|email|
|---|---|---|
|Lane|27|`lane@example.com`|
|Lane|27|`lane@example.com`|
|Allan|27|`allan@example.com`|

This table does _not_ adhere to 1NF. It has two identical rows, so there isn't a unique primary key for each row.

## Example of 1NF

The simplest way (but not the only way) to get into first normal form is to add a unique `id` column.

|id|name|age|email|
|---|---|---|---|
|1|Lane|27|`lane@example.com`|
|2|Lane|27|`lane@example.com`|
|3|Allan|27|`allan@example.com`|

It's worth noting that if you create a "primary key" by ensuring that two columns are always "unique together," that works too.

## _Almost Always_ Adhere to 1NF

First normal form is simply _a good idea_.

I've _never_ built a database schema where each table isn't _at least_ in first normal form.

# Second Normal Form (2NF)

A table in [second normal form](https://en.wikipedia.org/wiki/Second_normal_form) (2NF) follows all the rules of first normal form, and one _additional_ rule that applies only to composite primary keys:

- All columns that are _not_ part of the primary key are dependent on the _entire_ primary key, and not just one of the columns _in_ the primary key.

## Example of 1NF but _Not_ 2NF

In this table, the primary key is a combination of `first_name` + `last_name`.

|first_name|last_name|first_initial|
|---|---|---|
|Lane|Wagner|l|
|Lane|Small|l|
|Allan|Wagner|a|

This table does _not_ adhere to 2NF. The `first_initial` column is _entirely_ dependent on the `first_name` column, rendering it _redundant_.

## Example of 2NF

One way to convert the table above to 2NF is to add a new table that maps a `first_name` directly to its `first_initial`. This removes any duplicates!

|first_name|last_name|
|---|---|
|Lane|Wagner|
|Lane|Small|
|Allan|Wagner|

|first_name|first_initial|
|---|---|
|Lane|l|
|Allan|a|

## 2NF Is _Usually_ a Good Idea

You should probably _default_ to keeping your tables in second normal form. That said, there are good reasons to deviate from it, particularly for _performance_ reasons. The reason being that when you have to query a second table to get additional data, it can take a bit longer.

My rule of thumb is: optimize for data integrity and data de-duplication _first_. If you have speed issues, de-normalize accordingly.

# Third Normal Form (3NF)

A table in [third normal form](https://en.wikipedia.org/wiki/Third_normal_form) (3NF) follows all the rules of second normal form, and one _additional_ rule:

- All columns that aren't part of the primary key are dependent _solely_ on the primary key.

Notice that this is only _slightly_ different from second normal form. In second normal form we can't have a column completely dependent on only _part_ of the primary key, and in third normal form we can't have a column that is entirely dependent on _anything_ that isn't the primary key.

## Example of 2NF but _Not_ 3NF

In this table, the primary key is simply the `id` column.

|id|name|first_initial|email|
|---|---|---|---|
|1|Lane|l|`lane.works@example.com`|
|2|Breanna|b|`breanna@example.com`|
|3|Lane|l|`lane.right@example.com`|

This table _is_ in second normal form because `first_initial` is _not_ dependent on a part of the primary key. However, because it _is_ dependent on the `name` column, it doesn't adhere to third normal form.

## Example of 3NF

The way to convert the table above to 3NF is to add a new table that maps a `name` directly to its `first_initial`. Notice how similar this solution is to 2NF.

|id|name|email|
|---|---|---|
|1|Lane|`lane.works@example.com`|
|2|Breanna|`breanna@example.com`|
|3|Lane|`lane.right@example.com`|

|name|first_initial|
|---|---|
|Lane|l|
|Breanna|b|

## 3NF Is _Usually_ a Good Idea

The same exact rule of thumb applies to the second and third normal forms.

Optimize for data integrity and data de-duplication _first_ by adhering to 3NF. If you have speed issues, de-normalize accordingly.

# Boyce-Codd Normal Form (BCNF)

A table in [Boyce-Codd normal form](https://en.wikipedia.org/wiki/Boyce%E2%80%93Codd_normal_form) (created by [Raymond Boyce](https://en.wikipedia.org/wiki/Raymond_F._Boyce) and [Edgar Codd](https://en.wikipedia.org/wiki/Edgar_F._Codd)) follows all the rules of third normal form, plus one _additional_ rule:

- A column that's part of a primary key can _not_ be entirely dependent on a column that's _not_ part of that primary key.

This only comes into play when there are multiple possible primary key combinations that overlap. Another name for this is "overlapping candidate keys."

Only in rare cases does a table in third normal form _not_ meet the requirements of Boyce-Codd normal form!

## Example of 3NF but _Not_ Boyce-Codd

|release_year|release_date|sales|name|
|---|---|---|---|
|2001|2001-01-02|100|Kiss me tender|
|2001|2001-02-04|200|Bloody Mary|
|2002|2002-04-14|100|I wanna be them|
|2002|2002-06-24|200|He got me|

The interesting thing here is that there are three possible primary keys:

- `release_year` + `sales`
- `release_date`
- `name`

This means that by definition, this table _is_ in the second and third normal forms because those forms only restrict how dependent a column that is _not_ part of a primary key can be.

However, this table is _not_ in Boyce-Codd's normal form because `release_year` is entirely dependent on `release_date`.

## Example of BCNF

The easiest way to fix the table in our example is to remove the duplicate data from `release_date`. Let's make that column `release_day_and_month`.

|release_year|release_day_and_month|sales|name|
|---|---|---|---|
|2001|01-02|100|Kiss me tender|
|2001|02-04|200|Bloody Mary|
|2002|04-14|100|I wanna be them|
|2002|06-24|200|He got me|

## BCNF Is _Usually_ a Good Idea

The same exact rule of thumb applies to the second, third, and Boyce-Codd normal forms. That said, it's unlikely you'll see BCNF-specific issues in practice.

> Optimize for data integrity and data de-duplication _first_ by adhering to Boyce-Codd normal form. If you have speed issues, de-normalize accordingly.

# Normalization Review

In my opinion, the _exact_ definitions of 1st, 2nd, 3rd and Boyce-Codd normal forms simply are _not all that important_ in your work as a back-end developer.

However, what _is important_ is to understand the basic principles of data integrity and data redundancy that the normal forms teach us. Let's go over some rules of thumb that you should commit to memory – they'll serve you well when you design databases and even just in coding interviews.

## Rules of Thumb for Database Design

1. Every table should always have a unique identifier (primary key)
2. 90% of the time, that unique identifier will be a single column named `id`
3. Avoid duplicate data
4. Avoid storing data that is completely dependent on other data. Instead, compute it on the fly when you need it.
5. Keep your schema as simple as you can. Optimize for a _normalized_ database first. Only denormalize for speed's sake when you start to run into performance problems.

We'll talk more about speed optimization in a later chapter.

---

CH10: Joins

# Joins

Joins are one of the most important features that SQL offers. Joins allow us to make use of the relationships we have set up between our tables. In short, joins allow us to query multiple tables at the _same time._

## Inner Join

The simplest and most common type of join in SQL is the `INNER JOIN`. By default, a `JOIN` command is an `INNER JOIN`. An `INNER JOIN` returns all of the records in `table_a` that have matching records in `table_b` as demonstrated by the following Venn diagram.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/DS7U62Q-689x400.png)

## ON

To perform a table join, we need to tell the database how to "match up" the rows from each table. The `ON` clause specifies the columns from each table that should be compared.

When the same column name exists in both tables, we have to **specify which table each column comes from** using the table name (or an alias) followed by a dot `.` before the column name.

```sql
SELECT *
FROM employees
INNER JOIN departments
  ON employees.department_id = departments.id;
```

In this query:

- `employees.department_id` refers to the `department_id` column from the `employees` table.
- `departments.id` refers to the `id` column from the `departments` table.

The `ON` clause ensures that rows are matched based on these columns, creating a relationship between the two tables.

The query above returns _all_ the fields from _both_ tables. The `INNER` keyword only affects the number of _rows_ returned, not the number of _columns_. The `INNER JOIN` filters rows based on matching `department_id` and `id`, while the `SELECT *` ensures all columns from both tables are included.

## Why Is This Important?

In many databases, different tables might share the same column names, such as `id`. If you don't specify the table name (or alias) for a column, the database won't know which column to use for the join. For example, writing `ON id = id` won't work because the database can't distinguish between the `id` columns in each table.

# Namespacing on Tables

When working with multiple tables, you can specify which table a field belongs to using a `.`. For example:

> `table_name.column_name`

```sql
SELECT students.name, classes.name
FROM students
INNER JOIN classes ON classes.class_id = students.class_id;
```

The above query returns the `name` field from the `students` table _and_ the `name` field from the `classes` table.

# Left Join

A `LEFT JOIN` will return every record from `table_a` regardless of whether or not any of those records have a match in `table_b`. A left join will _also_ return any matching records from `table_b`. Here's a Venn diagram to help visualize the effect of a `LEFT JOIN`:

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/JoZYX5L-679x400.png)

A small trick you can do to make writing the SQL query easier is to define an [alias](https://en.wikipedia.org/wiki/Alias_\(SQL\)) for each table. Here's an example:

```sql
SELECT e.name, d.name
FROM employees e
LEFT JOIN departments d
  ON e.department_id = d.id;
```

Notice the simple alias declarations `e` and `d` for `employees` and `departments`, respectively.

Some developers do this to make their queries less verbose. That said, I personally _hate_ it because single-letter variables are harder to grok, so don't use them in this course!

# Right Join

A `RIGHT JOIN` is, as you may expect, the opposite of a `LEFT JOIN`. It returns all records from `table_b` regardless of matches, and all matching records between the two tables.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/Cy2v9q5-679x400.png)

A `RIGHT JOIN` is just a `LEFT JOIN` with the order of the tables switched, so in most cases `LEFT JOIN` is preferred for readability.

# Full Join

A `FULL JOIN` combines the result set of the `LEFT JOIN` and `RIGHT JOIN` commands. It returns _all_ records from both `table_a` and `table_b` regardless of whether or not they have matches.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/uAXzpID-679x400.png)

---

CH11: Performance

# SQL Indexes

An index is an in-memory structure that ensures that queries we run on a database are _performant_, that is to say, they run _quickly_.

If you can remember back to the data structures course, most database indexes are just [binary trees](https://en.wikipedia.org/wiki/Binary_tree) or [B-trees](https://en.wikipedia.org/wiki/B-tree)! The binary tree can be stored in [RAM](https://en.wikipedia.org/wiki/Random-access_memory) as well as on [disk](https://en.wikipedia.org/wiki/Computer_data_storage), and it makes it easy to look up the location of an entire row.

`PRIMARY KEY` columns are indexed by default, ensuring you can look up a row by its `id` very quickly. However, if you have other columns that you want to be able to do quick lookups on, you'll need to _index_ them.

## CREATE INDEX

```sql
CREATE INDEX index_name ON table_name (column_name);
```

It's fairly common to name an index after the column it's created on with a suffix of `_idx`.

# Index Review

As we discussed, an index is a data structure that can perform _quick lookups_.

By indexing a column, we create a new in-memory structure, usually a B-tree, where the values in the indexed column are sorted into the tree to keep lookups fast. In terms of [Big-O complexity](https://en.wikipedia.org/wiki/Big_O_notation), a B-tree index ensures that lookups are `O(log(n))`.

## Shouldn't We Index Everything?

While indexes make specific kinds of lookups much faster, they also add performance overhead – they can slow down a database in other ways.

Think about it: if you index every column, you could have hundreds of B-trees in memory! That needlessly bloats the memory usage of your database. It also means that each time you _insert_ a record, that record needs to be added to _many_ trees, slowing down your insert speed.

The rule of thumb is simple:

> Add indexes to columns you _know_ you'll be doing frequent lookups on. Leave everything else un-indexed. You can always add another index later.

# Multi-Column Indexes

Multi-column indexes are useful for the exact reason you might think – they speed up lookups that depend on _multiple_ columns.

## CREATE INDEX

```sql
CREATE INDEX first_name_last_name_age_idx
  ON users (first_name, last_name, age);
```

A multi-column index is sorted by the first column first, the second column next, and so forth. A lookup on _only_ the first column in a multi-column index gets almost all of the performance improvements that it would get from its own single-column index. However, lookups on only the second or third column will have very degraded performance.

## Rule of Thumb

Unless you have specific reasons to do something special, only add multi-column indexes if you're doing frequent lookups on a specific combination of columns.

Difference between multiple indexes and multi column indexes:
- Multi-column index = one index across multiple columns, e.g. `(user_id, recipient_id)`
- Separate indexes = individual indexes on each column, e.g. `(user_id)` and `(recipient_id)`
- Multi-column indexes are best for queries that filter by that column combination
- Column order matters: `(user_id, recipient_id)` helps `user_id` alone and both together, but not `recipient_id` alone very well
- Separate indexes help single-column lookups on each indexed column
- For `WHERE user_id = ? AND recipient_id = ?`, a multi-column index is usually better than two separate indexes

# Denormalizing for Speed

We left you with a cliffhanger in the "normalization" chapter. As it turns out, data integrity and deduplication come at a cost, and that cost is _usually_ speed.

Joining tables together, using subqueries, performing aggregations, and running post-hoc calculations _take time_. At very large scales these advanced techniques can actually become a _huge_ performance toll on an application – sometimes grinding the database server to a halt.

Storing duplicate information can drastically speed up an application that needs to look it up in _different ways_. For example, if you store a user's country information right on their user record, no expensive join is required to load their profile page!

That said, _denormalize at your own risk_! Denormalizing a database incurs a large risk of inaccurate and buggy data.

In my opinion, it should be used as a kind of "last resort" in the name of speed.

# SQL Injection

SQL is a _very_ common way hackers attempt to cause damage or breach a database. One of my [favorite XKCD comics](https://xkcd.com/327/) of all time demonstrates the problem:

![](https://bobby-tables.com/img/xkcd.png)

The joke here is that if someone was using this query:

```sql
INSERT INTO students(name) VALUES (?);
```

And the "name" of a student was `Robert'); DROP TABLE students;--` then the resulting SQL query would look like this:

```sql
INSERT INTO students(name) VALUES ('Robert'); DROP TABLE students;--');
```

As you can see, this is actually two queries! The first one inserts "Robert" into the database, and the second one _deletes the `students` table_!

## Protecting Against SQL Injection

You need to be aware of SQL injection attacks, but to be honest, the solution these days is simply to use a modern SQL library that sanitizes inputs. We don't often need to sanitize inputs by hand at the application level anymore.

For example, the Go standard library's SQL package automatically protects against SQL injection attacks if you [use it properly](https://go.dev/doc/database/sql-injection).

In short, don't interpolate user input into raw query strings yourself – make sure your database library has a way to sanitize inputs, and pass user-provided values into that.