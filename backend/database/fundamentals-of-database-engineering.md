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
