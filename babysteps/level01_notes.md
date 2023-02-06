
SQLite3 is a C library that provides a software implementation of a SQL database. It is designed to be simple, lightweight, and fast, making it well-suited for use in embedded systems, mobile devices, and other resource-constrained environments.

In PHP, SQLite3 is a class that provides an interface for working with SQLite databases. The class can be used to open and query SQLite databases, and provides methods for executing SQL commands, fetching data, and managing transactions. SQLite3 is a popular choice for small to medium-sized applications, as it requires no separate server and can be embedded directly into the application.

The sqlite_master table is a special table in SQLite that contains metadata about the database. It stores information about all tables, views, indices, and triggers in the database.

Here is a list of columns in the sqlite_master table:

`type`: The type of object (e.g., 'table', 'view', 'index', or 'trigger').
`name`: The name of the object.
`tbl_name`: The name of the table to which the object belongs (for indices and triggers).
`rootpage`: The page number in the database file that holds the root of the object (for tables and indices).
`sql`: The SQL statement that was used to create the object.

Every SQLite database contains a single "schema table" that stores the schema for that database. The schema for a database is a description of all of the other tables, indexes, triggers, and views that are contained within the database. The schema table looks like this:

```sql
CREATE TABLE sqlite_schema(
  type text,
  name text,
  tbl_name text,
  rootpage integer,
  sql text
);
```
The sqlite_schema table contains **one row for each table, index, view, and trigger (collectively "objects") in the schema**, except there is **no entry for the sqlite_schema table itself.** See the schema storage subsection of the file format documentation for additional information on how SQLite uses the sqlite_schema table internally.

## alternative names 

sqlite_schema => sqlite_master "work anywhere" => sqlite_temp_schema => sqlite_temp_master

the last 2 only work for the TEMP database associated with each database connection

SQLite creates the schema table upon database creation and modifies its content as SQLite users submit DDL statements for execution. There is no need for users to modify it under normal circumstances, and they bear the risk of database corruption if they do modify it.


