# [Fundamentals of Database Engineering](https://www.udemy.com/course/database-engines-crash-course)

- [ACID](#acid)
  - [Atomicity](#atomicity)
  - [Isolation](#isolation)
  - [Consistency](#consistency)
  - [Durability](#durability)
- [Database Internals](#database-internals)
- [Database Indexing](#database-indexing)
- [Database Partitioning](#database-partitioning)
- [Database Sharding](#database-sharding)

## ACID

> A **transaction** is a collection of queries which perform one unit of work. The lifespan of a transaction include the following states - BEGIN, COMMIT, ROLLBACK.

### Atomicity

- All queries in a transaction must succeed.
- If one query fails, all prior successful queries in the transaction should rollback.
- If the database went down prior to a commit of a transaction, all successful queries in the transaction should rollback.

### Isolation

#### Isolation Levels

- Read uncommitted - No isolation, any change from outside is visible to the transaction, committed or not
- Read committed - Each query in a transaction only sees committed changes by other transactions
- Repeatable read - The transaction will make sure that when a query reads a row, that row will remain unchanged while the transaction is running
- Snapshot - Each query in a transaction only sees changes that have been committed up to the start of the transaction. It's like a snapshot version of the database at that moment.
- Serializable - Transactions are run as if they are serialized one after the other (slowest)

Will run into this concept very rarely in practice. Good to know them.

#### DB Implementation of Isolation

- Each DBMS implements Isolation level differently
- Pessimistic - Row level locks, table locks, page locks to avoid lost updates
- Optimistic - No locks, just track if things changed and fail the transaction if so

### Consistency

The data should be consistent between different tables. This also refers to referential integrity, where the data between the table and the table to which the foreign key refers should be consistent.

### Durability

Even if I lost power or the database crashes after the commit, we should still see the change after the system is back online.
Changes made by committed transactions must be persisted in a durable non-volatile storage.

## Database Internals

### Page

- Depending on the storage model (row vs column store), the rows are stored and read in logical pages.
- The database doesn't read a single row, it reads a page or more in a single IO and we get a lot of rows in that IO.
- Each page has a fixed size
- Assume each page holds 3 rows. Then with 1001 rows we will have 1001/2 = 333 pages.

### IO

- IO operations are read requests to the disk
- We try to minimize this as much as possible
- An IO can fetch 1 page or more depending on the disk partitions and other factors
- An IO cannot read a single row, it reads a page with many rows in them, and you get those extra rows for free
- You want to minimize the number of IOs as they are expensive

### Heap

- The heap is a data structure where the table is stored with all its pages one after another
- This is where the actual data is stored including everything
- Traversing the heap is expensive as we need to read a lot of data to find what we want
- That is why we need indexes that help tell us exactly what part of the heap we need to read and what page(s) of the heap we need to pull

### Index

- An index is another data structure separate from the heap that has "pointers" to the heap
- It has part of the data and is used to quickly search for something
- You can index on one column or more
- Once you find a value of the index, you go to the heap to fetch more information
- Index tells you **exactly** which page to fetch in the heap instead of taking the hit to scan every page in the heap
- The index is also stored as pages and cost IO to pull the entries of the index
- The smaller the index, the more it can fit in memory the faster the search
- Popular data structure for index is b-trees

### Row-Oriented Database

- Tables are stored as rows in disk
- A single block IO read to the table fetches multiple rows with all their columns
- More IOs are required to find a particular row in a table scan but once you find the row, you get all the columns for that row

### Column-Oriented Database

- Tables are stored as columns first in disk
- A single block IO read to the table fetches multiple columns with all matching rows
- Less IOs are required to get more values of a given column. But working with multiple columns require more IOs.

## Database Indexing

We can use `explain` in postgres to view details of a particular query.
Here are the different parameters returned by the `explain` command:

- type of scan - The type of scan the database will perform to fetch the desired data.
- cost - The cost consists of two values. The first value is the time taken by the database to decide what index/table to fetch the data from and the second value is an approximation (in milliseconds) of the total time it will take to execute the query
- rows - This is an approximate value of the total number of rows which will be returned by the query.
- width - sum of the bytes of all the columns.

**Index Scan vs Index Only Scan** - If a database fetches all the rows for a particular query by only looking up the index and not the heap, then it is called an Index Only Scan.
Suppose in a table named `grades` which contains `name` and `id`, we want to create an index on `id` but we also want to store the corressponding names of the ids in the index, we can cretae the following index.

> CREATE INDEX id_idx on grades(id) include (name);

Now, the query `SELECT name FROM grades WHERE id = 7` is executed using an Index Only Scan.

**Key vs Non-Key Columns** - In the above example, `id` is a key and `name` is a non-key.
If we have a composite index on `a` and `b`, then the database will not use the index if only `b` is present in the query.

```sql
CREATE INDEX test_a_b_idx on test(a, b);

SELECT c FROM test WHERE a = 10; /* Index Only / Bitmap Index Scan */
SELECT c FROM test WHERE a = 10 AND b = 70; /* Index Only / Bitmap Index Scan */
SELECT c FROM test WHERE b = 70; /* Parellel Seq Scan */
SELECT c FROM test WHERE a = 10 OR b = 70; /* Parallel Seq Scan */
```

When we create an index, the database writes are blocked which is not a good practice in production. So, to create an index without blocking production database writes, use the following query.

```sql
CREATE INDEX CONCURRENTLY g on grades(g);
```

**Bloom Filter** - This filter is useful when the server receives requests where it has to check if a particular value is present in a database. We maintain a bitmap of let's say size 64. Whenever the server receives a request to check if a particular name/value is present in a table, we first mod the hash of the value against 64 (size of bitmap) and check if the ith bit in the bitmap is set where `i = hash(value)%64`. If the ith bit in the bitmap is unset, we do not need to query the database as the value won't be present there for sure. On the other hand, if the ith bit is set, then we query the database. This way, we save a lot of expensive queries on the database.

To maintain a Bloom filter, before writing a value to the database, we set the bitmap index of that value.

## Database Partitioning

Partitioning is breaking down a table into smaller tables, each containing a fraction of data contained in the main table.

- **Horizontal Partitioning** partitions on basis of rows.
- **Vertical Partitioning** partitions on the basis of columns.

Data can be partitioned on a certain range (dates, IDs, etc.), by list (discrete values eg countries) or by hashing.

```sql
/* create main table */
CREATE TABLE grades_part(id serial not null, g int not null) partition by range(g);

/* create partition tables */
CREATE TABLE g0035 (like grades_part including indexes);
CREATE TABLE g3560 (like grades_part including indexes);
CREATE TABLE g6080 (like grades_part including indexes);
CREATE TABLE g80100 (like grades_part including indexes);

/* attaching partitions to main table */
ALTER TABLE grades_part attach partition g0035 FOR VALUES FROM (0) TO (35);
ALTER TABLE grades_part attach partition g3560 FOR VALUES FROM (35) TO (60);
ALTER TABLE grades_part attach partition g6080 FOR VALUES FROM (60) TO (80);
ALTER TABLE grades_part attach partition g80100 FOR VALUES FROM (80) TO (100);
```

**Advantages**:

- Improves query performance
- Sequential scan vs scattered index scan
- Easy bulk loading
- Archive old data that are barely accessed into cheap storage

**Disadvantages**:

- Updates that move rows from a partition to another are slow and sometimes fail
- Inefficient queries could accidentally scan all partitions resulting in slower performance
- Schema changes can be challenging

## Database Sharding

Database Sharding is simply horizontal partitioning of data in a database onto separate database server instances to spread the load.

The server instance to read or write from is decided using _Consistent Hashing_.

**Difference between Horizontal Partitioning and Sharding:**

- HP splits a big table into multiple tables within the same database whereas sharding splits it into multiple tables across multiple database servers
- The table name changes in HP whereas in sharding, everything remains the same except the server

**Advantages:**

- Scalability
- Security (users can access certain shards)
- Optimal and smaller index size

**Disadvantages:**

- Complex client
- Transactions across shards cannot be done
- Rollbacks
- Schema changes are hard
- Joins

**When should you consider Sharding?**

- If reads from a table are getting slower due to high number of rows, consider Horizontal Paritioning
- However, if your database just cannot handle the increased load, coonsider Replication
- Finally, if the above methods are not enough, consider Sharding
