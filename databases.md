# Databases

## Types

- **Hierarchical databases**: Tree-styled database where a single object has
  one or more objects beneath it. No child can have more than one parent. It
  offers data access using standard tree traversal algorithms. Example:
  Windows registry store.
- **Relational databases**: Data is stored in structured tables and CRUD
  operations are performed in SQL (Structured Query Language). Relationships
  define the links between entities (tables).
  - MySQL
  - Oracle SQL
  - MS SQL Server
  - PostgreSQL
- **Non-relational databases**: Also referred to as NoSQL databases. Data is
  unstructured and can have significant redundancy for optimal performance.
  - Document databases: JSON, BSON, XML storage formats. Example: MongoDB.
  - Key-value stores
  - Graph databases: A data object is represented as a node and relations are
    represented by edges between nodes. Useful for use in social networks.
    Example: Neo4j.
- **Vector databases**: Data is stored as high dimensional vectors which are
  derived by applying some mathematical transformations on the raw data. To
  find similarities among vectors, metrics such as euclidean distance, hamming
  distance etc. are used in the defined vector space. These are specifically
  used in AI/ML domain such as Natural language processing, computer vision
  and large language models. Example: Pinecone.

## Relational databases

- Relational is optimal choice for strictly structured data.
- Relational DB minimizes redundancy in data and due to fixed structure and
  order it is scalable by database sharding.
- Scalable using database sharding.
- ACID compliant
  - Atomicity: Each transaction is either run completely or not run at all.
  - Consistency: Follow rules that validate and prevent corruption at every
    step.
  - Isolation: Transactions are mutually exclusive.
  - Durability: Completed transactions are persitent even on system failure.
- Data is stored in the form of B-trees. It is a self balancing tree in which
  actual values are stored in the leaf nodes and internal nodes contain info
  about how to reach them.

## Non-relational databases

- Unstructured or semi-structured data.
- Easier to scale horizontally.
- Trade availability over consistency for high performance systems. Although
  they still provide eventual consistency.

## Normalization

- Eliminating redundancy and improving data integrity in the database.
- Levels:
  - 1NF: Data must be seperated into columns.
  - 2NF: All non-key attribute must be functionally dependent on the primary
    key. No partial dependence is allowed, i.e., `X -> Y` is not allowed if
    Y contains both non-key and key attributes.
  - 3NF: No transitive dependency. This implies that for every trivial
    functional dependency `X -> Y`:
    1. X is a super key, and/or,
    2. Y is a prime attribute (part of candidate key).
  - BCNF: For every functional dependency `X -> Y`, X must be a super key.
- Denormalization: Introducing redundancy deliberately to facilitate improved
  read performance for specific scalablity use cases.

## CAP theorem

- Consistency, Availability, Partition tolerance.
- Specifies trade-offs for large scale distributed databases.
- At most two properties can be achieved at a time.

## Database sharding

- Partition the database based on some _shard key_.
- It is a way to scale databases horizontally. Improves read performance
  particularly for relational databases.
- Shard key must be chosen such that it best satisfies two conditions:
  1. High cardinality
  2. Low frequency
- Usually the primary key of the table or its combination with some other
  attribute is chosen as the shard key.

## Data concurrency

- Concurrent data processing is achieved with the help of mutex locks.
- Types:
  - Shared lock: Only allows reading, shared among multiple readers.
  - Exclusive lock: Acquired by writers. It can be held by only one writer at
    a time.

## PostgreSQL

It is the most widely used SQL database for production level applications.
Some of the reasons for this are:

- **Multi-version concurrency control**: While querying a database each
  transaction sees a snapshot of the data (a database version) as it was some
  time ago (not too long), regardless of the current state of the underlying
  data. This protects the transaction from viewing inconsistent data which may
  be caused by any other concurrent transaction accessing the same rows and
  columns. This provides transaction isolation during different database sessions.

- **Inheritance** in schemas inspired from the object oriented paradigm. This
  allows for better integration with Object relation models (ORMs).

- Ability to create complex custom attributes by combining native or other custom
  attributes. This is analogous to composition in OOP. It also has native JSON
  datatype for storing which can be used for semi-structured data.

- Stored precompiled SQL procedures.
