# [Fundamentals of Database Engineering](https://www.udemy.com/course/database-engines-crash-course)

## ACID

> A **transaction** is a collection of queries which perform one unit of work. The lifespan of a transaction include the following states - BEGIN, COMMIT, ROLLBACK.

### Atomicity

- All queries in a transaction must succeed.
- If one query fails, all prior successful queries in the transaction should rollback.
- If the database went down prior to a commit of a transaction, all successful queries in the transaction should rollback.

### Isolation
