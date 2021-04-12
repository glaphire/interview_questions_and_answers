# DQL vs DBAL

## DBAL 

[Original documentation](https://www.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/introduction.html)

The Doctrine database abstraction & access layer (DBAL) offers 
a lightweight and thin runtime layer around a PDO-like API and 
a lot of additional, horizontal features like database schema 
introspection and manipulation through an OO API.

The following database vendors are currently supported:

- MySQL
- Oracle
- Microsoft SQL Server
- PostgreSQL
- SQLite

The Doctrine 2 database layer can be used independently of the object-relational mapper.

The DBAL consists of two layers: drivers and a wrapper. 

Each layer is mainly defined in terms of 3 components: Connection, Statement and Result. 

A `Doctrine\DBAL\Connection` wraps a `Doctrine\DBAL\Driver\Connection`, 
a `Doctrine\DBAL\Statement` wraps a `Doctrine\DBAL\Driver\Statement` 
and a `Doctrine\DBAL\Result` wraps a `Doctrine\DBAL\Driver\Result`.

**What do the wrapper components add to the underlying driver implementations? 
The enhancements include SQL logging, 
events and control over the transaction isolation level in a portable manner, among others.**

The DBAL is separated into several different packages that 
separate responsibilities of the different RDBMS layers.

[Important topic - security in DBAL (documentation)](https://www.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/security.html#security)

## DQL

[Documentation](https://www.doctrine-project.org/projects/doctrine-orm/en/2.8/reference/dql-doctrine-query-language.html)

DQL stands for Doctrine Query Language and is an Object Query Language derivative 
that is very similar to the Hibernate Query Language (HQL) or the Java Persistence Query Language (JPQL).

DQL provides powerful querying capabilities over your object model. 
Imagine all your objects lying around in some storage (like an object database). 
When writing DQL queries, think about querying that storage to pick a certain subset of your objects.

**Notes**:
- A common mistake for beginners is to mistake DQL for being just some form of SQL and 
therefore trying to use table names and column names or join arbitrary 
tables together in a query. You need to think about DQL as a query language 
for your object model, not for your relational schema.
- DQL is case in-sensitive, except for namespace, class and field names, which are case sensitive.

**Types of DQL queries**

DQL as a query language has SELECT, UPDATE and DELETE constructs 
that map to their corresponding SQL statement types. 
**INSERT statements are not allowed in DQL**, because entities and their 
relations have to be introduced into the persistence context through EntityManager#persist() 
to ensure consistency of your object model.

DQL SELECT statements are a very powerful way of retrieving parts 
of your domain model that are not accessible via associations. 
Additionally they allow you to retrieve entities and their associations 
in one single SQL select statement which can make a huge difference 
in performance compared to using several queries.

DQL UPDATE and DELETE statements offer a way to execute 
bulk changes on the entities of your domain model.
This is often necessary when you cannot load
all the affected entities of a bulk update into memory.

**Joins**

A SELECT query can contain joins. 

There are 2 types of JOINs: Regular Joins and Fetch Joins:
- Regular Joins: Used to limit the results and/or compute aggregate values.
- Fetch Joins: In addition to the uses of regular joins: 
Used to fetch related entities and include them in the hydrated result of a query.

## TLDR
DBAL is used for creating abstraction layer over PDO to work with database (includes QueryBuilder).
DQL is a part of ORM possibilities and allows to work with entities as 
tables in some kind of queries (SELECT, UPDATE, DELETE + JOIN).