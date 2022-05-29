# Designing Data Intensive Applications

- [Reliable, Scalable and Maintainable Applications](#reliable-scalable-and-maintainable-applications)
  - [Reliability](#reliability)
  - [Scalability](#scalability)
  - [Maintainability](#maintainability)
- [Data Models and Query Languages](#data-models-and-query-languages)
  - [Relational Model vs Document Model](#relational-model-vs-document-model)
  - [Query Languages for Data](#query-languages-for-data)
  - [Graph-Like Data Models](#graph-like-data-models)
- [Storage and Retrieval](#storage-and-retrieval)
  - [Data Structures That Power Your Database](#data-structures-that-power-your-database)
  - [Transaction Processing or Analytics?](#transaction-processing-or-analytics)
  - [Column-Oriented Storage](#column-oriented-storage)

## Reliable, Scalable and Maintainable Applications

A data-intensive application is typically built from standard building blocks that provide commonly needed functionality. For example, many applications need to:

- Store data so that they, or another application, can find it again later (databases)
- Remember the result of an expensive operation, to speed up reads (caches)
- Allow users to search data by keyword or filter it in various ways (search indexes)
- Send a message to another process, to be handled asynchronously (stream processing)
- Periodically crunch a large amount of accumulated data (batch processing)

### Reliability

The system should continue to work correctly (performing the correct function at the desired level of performance) even in the face of adversity (hardware or software faults, and even human error).

- The application performs the function that the user expected.
- It can tolerate the user making mistakes or using the software in unexpected ways.
- Its performance is good enough for the required use case, under the expetcted load and data volume.
- The system prevents any unauthorized access and abuse.

The things that can go wrong are called faults, and systems that anticipate faults and can cope with them are called fault-tolerant or resilient.
A fault is usually defined as one component of the system deviating from its spec, whereas a failure is when the system as a whole stops providing the required service to the user.

Types of Faults:

- **Hardware Faults** - Hard disks crash, RAM becomes faulty, the power grid has a blackout, someone unplugs the wrong network cable.
- **Software Errors** - The bugs that cause these kinds of software faults often lie dormant for a long time until they are triggered by an unusual set of circumstances. In those circumstances, it is revealed that the software is making some kind of assumption about its environment—and while that assumption is usually true, it eventually stops being true for some reason.
- **Human Errors** - Ways to prevent human errors:
  1. Design systems in a way that minimizes opportunities for error (using well-defined abstractions, APIs, etc.)
  2. Decouple the places where people make the most mistakes from the places where they can cause failures (like having staging environments)
  3. Test thoroughly
  4. Allow quick and easy recovery from human errors, to minimize the impact in the case of a failure.
  5. Set up detailed and clear monitoring, such as performance metrics and error rates.

### Scalability

As the system grows (in data volume, traffic volume, or complexity), there should be reasonable ways of dealing with that growth.

**Load** - Load can be described with a few numbers which we call load parameters. The best choice of parameters depends on the architecture of your system: it may be requests per second to a web server, the ratio of reads to writes in a database, the number of simultaneously active users in a chat room, the hit rate on a cache, or something else.

**Performance** - Performance of a system depends on the type of system. In online systems, what’s usually more important is the service’s response time—that is, the time between a client sending a request and receiving a response.

> Latency and response time are often used synonymously, but they are not the same. The response time is what the client sees: besides the actual time to process the request (the service time), it includes network delays and queueing delays. Latency is the duration that a request is waiting to be handled—during which it is latent, awaiting service.

Percentiles are a better metric than average response time to gauge the performance of a system. Percentiles are often used in _service level objectives (SLOs)_ and _service level agreements (SLAs)_, contracts that define the expected performance and availability of a service.

### Maintainability

Over time, many different people will work on the system (engineering and operations, both maintaining current behavior and adapting the system to new use cases), and they should all be able to work on it productively.

However, we can and should design software in such a way that it will hopefully minimize pain during maintenance, and thus avoid creating legacy software ourselves. To this end, we will pay particular attention to three design principles for software systems:

- Operability - Make it easy for operations teams to keep the system running smoothly.
- Simplicity - Make it easy for new engineers to understand the system, by removing as much complexity as possible from the system.
- Evolvability - Make it easy for engineers to make changes to the system in the future, adapting it for unanticipated use cases as requirements change. Also known as _extensibility_, _modifiability_, or _plasticity_.

## Data Models and Query Languages

A data model is an abstract model that organizes elements of data and standardizes how they relate to one another and to the properties of real-world entities.

### Relational Model vs Document Model

The roots of relational databases lie in _business data processing_. However, even for the more powerful and networked computers today, relational databases generalized very well.

Driving forces behind NoSQL adoption:

- need for greater scalability
- preference for free and open-source software
- specialized query operations
- restrictiveness of relational schemas

Most application development today is done in object-oriented programming languages, which leads to a common criticism of the SQL data model: if data is stored in relational tables, an awkward translation layer is required between the objects in the application code and the database model of tables, rows, and columns. The disconnect between the models is sometimes called an impedance mismatch. Some developers feel like the JSON model reduces this impedance mismatch.

When it comes to representing many-to-one and many-to-many relationships, relational and document databases are not fundamentally different. In both cases, the related item is referenced by a unique identifier, which is called a _foreign key_ in the relational model and a _document reference_ in the document model.

#### Which data model leads to simpler application code?

- if the data in the application is in the form of a tree of one-to-many relationships, where typically the entire tree is loaded at once, then it's a good idea to use a document model. The relational technique of splitting a document-like structure into multiple tables can lead to cumbersome schema and unnecessarily cumbersome code.
- document databases have poor support for joins. This might not be a problem in analytics applications in which many-to-many relationships are never needed to record the time at which events occur.
- if application uses many-to-many relationships, the document model can lead to more complex code and worse performance.
- for highly interconnected data, the relational model is more acceptable

#### Schema flexibility in document model

Most document databases do not enforce any schema on the data in documents. Document databases are sometimes called schemaless, but that’s misleading, as the code that reads the data usually assumes some kind of structure—i.e., there is an implicit schema, but it is not enforced by the database.

If the schema has to be changed, in document models, we could simply start writing new data in the new schema format. However, in relational schema, a migration might have to be performed.

### Query Languages for Data

SQL is a _declarative_ query language whereas most commonly used programming languages are imperative. An imperative language tells the computer to perform certain operations in a certain order. On the other hand, in a declarative query language, you just specify the pattern of the data you want—what conditions the results must meet, and how you want the data to be transformed (e.g., sorted, grouped, and aggregated)—but not how to achieve that goal.

Imperative code is very hard to parallelize across multiple cores and multiple machines, because it specifies instructions that must be performed in a particular order. Declarative languages have a better chance of getting faster in parallel execution because they specify only the pattern of the results, not the algorithm that is used to determine the results.

The advantages of declarative query languages are not limited to just databases. They can be seen in a web browser. Imagine having to set the background-color to a particular selector in CSS `li.selected > p`. It is fairly straighforward since it is a declarative approach. Imagine having to imperatively using JS to set the color. The code for the same would look awful.

### Graph-Like Data Models

If our application mostly has mostly one-to-many relationships or no relationships, it makes more sense to use a document model. However, if many-to-many relationships are common in our application, then the data becomes too complex for a relational model and we can start modelling the data as a graph.

#### Property Graphs

Each vertex consists of:

- a unique identifier
- a set of incoming edges
- a set of outgoing edges
- a collection of key-value pairs

Each edge consists of:

- a unique identifier
- vertex at which the edge starts (tail vertex)
- vertex at which the edge ends (head vertex)
- a label to describe the kind of relationship
- a collection of key-value pairs

Representing a property graph using a relational schema:

```sql
CREATE TABLE vertices (
    vertex_id   integer PRIMARY KEY,
    properties  json
);

CREATE TABLE edges (
    edge_id     integer PRIMARY KEY,
    tail_vertex integer REFERENCES vertices (vertex_id),
    head_vertex integer REFERENCES vertices (vertex_id),
    label       text,
    properties  json
);

CREATE INDEX edges_tails ON edges (tail_vertex);
CREATE INDEX edges_heads ON edges (head_vertex);
```

#### Graph Queries in SQL

The above example suggests that graph data can be represented in a relational database. We can also query this graph data in a relational structure, but with some difficulty.

In a relational database, you usually know in advance which joins you need in your query. In a graph query, you may need to traverse a variable number of edges before you find the vertex you’re looking for—that is, the number of joins is not fixed in advance.

#### Triple Stores

Triple Store model is mostly equivalent to the property graph model. In a triple-store, all information is stored in the form of very simple three-part statements: (subject, predicate, object).

- Subject -> vertex
- Object -> a value in primitive datatype OR another vertex

## Storage and Retrieval

### Data Structures That Power Your Database

Consider the world's simplest database implemented using just 2 Bash functions.

```bash
#!/bin/bash

db_set () {
    echo "$1,$2" >> database
}

db_get () {
    grep "^$1," database | sed -e "s/^$1,//" | tail -n 1
}
```

While `db_set()` has good performance, `db_get()` here would have horrible performance as it would lookup the entire list every time we fetch the value for a key. This is where indexing comes in.

An index is an additional structure that is derived from the primary data. Maintaining additional structures incurs overhead, especially on writes.

#### Hash Indexes

Hash maps can be used to index the above database if we maintain a map where the key is the key in the database and the value is the offset/location of that key in the database.

How do we avoid eventually running out of disk space? A good solution is to break the log into segments of a certain size by closing a segment file when it reaches a certain size, and making subsequent writes to a new segment file. We can then perform _compaction_ on these segments.

> Compaction means throwing away duplicate keys in the log, and keeping only the most recent update for each key.

#### SSTables and LSM-Trees

In the segment files, if we require that the sequence of key-value pairs is _sorted by key_, then this format is called _Sorted String Table_ or _SSTable_ for short. This way, merging the segments becomes easier using the mergesort approach. Also, we no longer need to maintain an index of _all_ the keys in memory: say you’re looking for the key `handiwork`, but you don’t know the exact offset of that key in the segment file. However, you do know the offsets for the keys `handbag` and `handsome`, and because of the sorting you know that `handiwork` must appear between those two. This means you can jump to the offset for `handbag` and scan from there until you find `handiwork`.

How to construct and maintain SSTables?
This can be done using various data structures like _red-black trees_ or _AVL trees_.

- When a write comes in, add it to an in-memory balanced tree data structure.
- When the memtable gets bigger than some threshold—typically a few megabytes—write it out to disk as an SSTable file.
- In order to serve a read request, first try to find the key in the memtable, then in the most recent on-disk segment, then in the next-older segment, etc.
- From time to time, run a merging and compaction process in the background to combine segment files and to discard overwritten or deleted values.

#### B-Trees

Like SSTables, B-trees keep key-value pairs sorted by key, which allows efficient key-value lookups and range queries. B-trees break the database down into fixed-size blocks or pages, traditionally 4 KB in size (sometimes bigger), and read or write one page at a time. This design corresponds more closely to the underlying hardware, as disks are also arranged in fixed-size blocks.

One page is designated as the root of the B-tree; whenever you want to look up a key in the index, you start here. The page contains several keys and references to child pages. Each child is responsible for a continuous range of keys, and the keys between the references indicate where the boundaries between those ranges lie.

![B-Tree Index Lookup](../images/b-trees.png)

The number of references to child pages in one page of the B-tree is called the _branching factor_. In the above image, the _branching factor_ is 6.

In order to make the database resilient to crashes, it is common for B-tree implementations to include an additional data structure on disk: a write-ahead log (WAL, also known as a redo log). This is an append-only file to which every B-tree modification must be written before it can be applied to the pages of the tree itself. When the database comes back up after a crash, this log is used to restore the B-tree back to a consistent state.

Careful concurrency control is required if multiple threads are going to access, typically done protecting the tree internal data structures with latches (lightweight locks).

#### Comparing B-Trees and LSM-Trees

**Advantages of LSM Trees:**

- LSM-trees are typically able to sustain higher write throughput than B-trees
- LSM-trees can be compressed better, and thus often produce smaller files on disk than B-trees

**Downsides of LSM Trees:**

- Compaction process can sometimes interfere with the performance of ongoing reads and writes
- An advantage of B-trees is that each key exists in exactly one place in the index, whereas a log-structured storage engine may have multiple copies of the same key in different segments.

#### Other Indexing Structures

There are two ways the value of a key can be stored in an index - it can be the actual row or it could be a reference to the row stored elsewhere. In the latter case, the place where the rows are stored is known as a _heap file_. The heap file approach is common because it avoids duplicating data when multiple secondary indexes are present: each index just references a location in the heap file, and the actual data is kept in one place.

When updating a value without changing the key, the heap file approach can be quite efficient: the record can be overwritten in place, provided that the new value is not larger than the old value.

In some situations, the extra hop from the index to the heap file is too much of a performance penalty for reads, so it can be desirable to store the indexed row directly within an index. This is known as a _clustered index_.

A secondary index can be easily constructed from a key-value index. The main difference is that in a secondary index, the indexed values are not necessarily unique. There are two ways of doing this: making each value in the index a list of matching row identifiers or by making a each entry unique by appending a row identifier to it.

### Transaction Processing or Analytics?

In earlier days, a write to database meant some kind of _commercial transaction_ taking place. Hence, the term _transaction_ stuck.

Now, a database is used for many different kinds of data, including data collected for analytics purposes. The separate database to store this kind of data is called a _data warehouse_.

OLTP - Transaction Processing Systems
OLAP - Analytics Systems

#### Data Warehousing

A _data warehouse_ is a database which the analysts use to run their analytics queries without affecting the OLTP operations since these operation typically require high availability.

Data is extracted from OLTP databases (using either a periodic data dump or a continuous stream of updates), transformed into an analysis-friendly schema, cleaned up, and then loaded into the data warehouse. This process of getting data into the warehouse is known as _Extract–Transform–Load (ETL)_.

Data warehouses are used in a faily formulaic style, known as _star schema_.

### Column-Oriented Storage

Column-oriented storage is simple: don't store all the values from one row together, but store all values from each column together instead. If each column is stored in a separate file, a query only needs to read and parse those columns that are used in a query, which can save a lot of work.

Column-oriented storage often lends itself very well to compression as the sequences of values for each column look quite repetitive, which is a good sign for compression. A technique that is particularly effective in data warehouses is bitmap encoding.

Bitmap indexes are well suited for all kinds of queries that are common in a data warehouse.

Column-oriented storage, compression, and sorting helps to make read queries faster and make sense in data warehouses, where most of the load consist on large read-only queries run by analysts. The downside is that writes are more difficult.
