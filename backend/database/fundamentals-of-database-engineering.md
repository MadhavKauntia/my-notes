# [Fundamentals of Database Engineering](https://www.udemy.com/course/database-engines-crash-course)

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
