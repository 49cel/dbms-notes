# Database Management Systems

## Units 1 and 2 - Complete Notes

Reference tables used in all query examples:
- **EMP** (empno, ename, job, mgr, hiredate, sal, comm, deptno)
- **DEPT** (deptno, dname, loc)
- **SALGRADE** (grade, losal, hisal)

Each topic below is written to be usable as a direct exam answer. Where the professor gave a specific diagram or example, it has been reproduced faithfully.

---

# UNIT 1: INTRODUCTION TO DBMS

---

## Q. Explain data, information, and data stores.

**Data** is a collection of raw, unprocessed facts that on their own carry no particular meaning.

Examples of data:
- Student marks
- Customer names

**Information** is meaningful data, or data that has been processed to produce a useful result. When we process data, the meaningful result we obtain is called information.

Examples of information:
- Student marksheet
- Invoice of a customer

**Data store** is a place where we can store data or information. There are three kinds of data stores used to store data permanently in secondary storage devices:

1. Papers and books
2. Flat files
3. Database

---

## Q. What are flat files? Explain the drawbacks of flat file systems.

A **flat file** is a traditional mechanism used to store data or information permanently in secondary storage devices. In a flat file mechanism, **every application program maintains its own file, separate from other application programs** in the organization.

**Diagram: Flat file data management**

```
    +-----------+     +-----------+     +-----------+
    | Invoicing |     |    CRM    |     |    GIS    |     <- Applications
    |  App.Prog |     |  App.Prog |     |  App.Prog |
    +-----------+     +-----------+     +-----------+
         |                 |                  |
         v                 v                  v
    +-----------+     +-----------+     +-----------+
    |  File 1   |     |  File 2   |     |  File 3   |     <- Files
    | cust_name |     | cust_name |     | cust_name |
    | cust_no   |     | cust_no   |     | turnover  |
    | valcode   |     |           |     |           |
    +-----------+     +-----------+     +-----------+
              \___________|__________/
                          |
                    Redundant Data
```

Each application has its own file, and the same customer data ends up stored in three separate places. This produces the following drawbacks:

### 1. Data Retrieval
If we want to retrieve data from a flat file, we must first develop an application program in a high-level language (like C or COBOL) specifically to read that file. Flat files have no built-in query language. In contrast, if we want to retrieve data from a database, we simply use **SQL** (Structured Query Language).

### 2. Data Redundancy
In flat files, the same data is often kept in multiple files at different locations. This duplicate or redundant data creates a serious problem: whenever we modify the data in one location, the copies in other locations are not automatically affected. This is called **inconsistency**.

Flat files do not maintain consistency automatically. A database, however, automatically maintains consistent data through transactions. Every transaction internally has four properties, collectively called the **ACID** properties, which are what allow the database to automatically maintain consistent data. If we want to reduce duplicate data in a database, we use the process of **Normalization**.

### 3. Data Integrity
Integrity means maintaining proper, valid data. In a database we maintain proper data by using **constraints** such as primary key, foreign key, unique, not null, and check. In flat files, since there are no built-in constraints, we must write application programs in a high-level language ourselves to enforce valid data.

### 4. Data Security
Data stored in flat files cannot be secured because flat files do not provide any security mechanism. Anyone with access to the file has access to everything in it. Databases, on the other hand, provide **role-based security** so that different users have different levels of access.

### 5. Data Indexing
If we want to retrieve data very quickly from a database, we use an **indexing** mechanism. Flat files do not support indexing at all, which makes searching slow.

Because organizations kept suffering from these five drawbacks of flat files, they eventually adopted a new kind of software called **DBMS**, which stores data efficiently in secondary storage.

---

## Q. Define DBMS. Explain its advantages over file-processing systems.

**DBMS (Database Management System)** is a collection of programs (software) written to manage data. It is used to store, manage, and retrieve data efficiently, and to define, manipulate, and control access to that data.

Examples of DBMS software:
- Oracle
- Teradata
- SQL Server
- MySQL
- SQLite
- Informix
- Sybase

When we install DBMS software into our system, some space is automatically created on the hard disk. This space is called the **database**. Automatically, a user interface is also created through which we can directly interact with the database. Through application programs, we interact with the database indirectly.

**Diagram: DBMS approach for data management**

```
    +----------+   +----------+   +----------+
    | App.Prog |   | App.Prog |   | App.Prog |
    |     1    |   |     2    |   |     3    |
    +----------+   +----------+   +----------+
          \             |              /
           \            |             /
            +--------------------------+
            |   Database Management     |
            |         System            |
            +--------------------------+
                        |
            +--------------------------+
            |     Operating System      |
            |      (File System)        |
            +--------------------------+
                        |
        +--------------------------------+
        |            Database            |
        |    +------+  +------+  +----+  |
        |    |Table |  |Table |  |... |  |
        |    +------+  +------+  +----+  |
        +--------------------------------+
              ^
              |
        +---------+
        |  User   |
        | Interface|
        +---------+
```

### Advantages of DBMS over Flat File System

| Flat File Problem | DBMS Solution |
|---|---|
| No query language; retrieval needs custom programs | SQL provides an easy non-procedural retrieval language |
| Data redundancy and inconsistency | Normalization removes redundancy; ACID transactions maintain consistency automatically |
| No integrity rules built in | Constraints (PK, FK, unique, not null, check) enforced automatically |
| No security mechanism | Role-based security through GRANT and REVOKE |
| No indexing; slow retrieval | Indexes support fast retrieval |
| No concurrent multi-user access | Multi-user architecture with locking and concurrency control |

### ACID Properties

The four ACID properties that every database transaction has:

- **Atomicity** - a transaction either fully completes or does not happen at all. There is no partial execution.
- **Consistency** - a transaction moves the database from one valid state to another valid state.
- **Isolation** - if many transactions run at the same time, they do not interfere with each other.
- **Durability** - once a transaction is committed, its effects are permanent even if the system crashes immediately afterward.

### Components / Overall Structure of a DBMS

The main components that make up a DBMS include:
- Query processor
- DDL and DML compilers
- Storage manager
- Transaction manager
- Buffer manager
- File manager
- Data dictionary (system catalog)
- The database itself
- User interface (login screen with username and password)

### Multi-User DBMS Architecture

A DBMS is built to allow many users and many application programs to connect to the same database at the same time. The DBMS manages this simultaneous access through **concurrency control** and **locking mechanisms**, ensuring users do not corrupt each other's data. A plain file system has no such capability.

---

## Q. Define database. What are the types of structures a database has?

A **database** is an organized collection of interrelated data. In other words, it is a collection of structured data.

Every database has two types of structures:

### 1. Logical Structure
A structure which is **not visible in the operating system** is called a logical structure.

Logical structure contains: tables, views, sequences, synonyms, and so on.
- Logical structure is handled by the **database developer** or the **database administrator**.

### 2. Physical Structure
A structure which **is visible in the operating system** is called a physical structure.
- Physical structure is handled by the **DBA (Database Administrator) only**.

---

## Q. Explain the three-level ANSI/SPARC architecture of a DBMS.

**ANSI (American National Standards Institute)** established a three-level architecture for DBMS. This architecture is also called **ANSI/SPARC** architecture, where SPARC stands for Standards Planning And Requirements Committee.

**Main objective of DBMS architecture:** to separate the user's view of the database from the way the data is physically stored.

DBMS architecture consists of three levels:
1. External Level
2. Conceptual Level
3. Internal Level

**Diagram: Three-level architecture**

```
              +-------+  +-------+  +-------+
              | View1 |  | View2 |  | View3 |   <- External Level
              +-------+  +-------+  +-------+       [views, synonyms]
                    \        |         /
                     \       |        /              (Logical Data Independence)
                      \      |       /
                    +------------------+
                    | Conceptual Level |         <- [Tables]
                    +------------------+
                             |                    (Physical Data Independence)
                             |
                    +------------------+
                    |  Internal Level  |         <- [Index, Cluster]
                    +------------------+
                             |
                        +---------+
                        | Database|
                        +---------+
```

### 1. Conceptual Level
The conceptual level describes the **logical structure of the whole database** for all users combined. This level does **not** define how data is physically stored in the database.

At the conceptual level we define:
- What type of data can be stored (data type and datatype size)
- What data cannot be stored (through constraints such as primary key, foreign key)
- Relationships between tables

Example (SQL at conceptual level):
```sql
CREATE TABLE bank (
    accno   NUMBER(10) PRIMARY KEY,
    name    VARCHAR2(10),
    balance NUMBER(10)
);
```

### 2. External Level
The external level describes the **user's view** of the database.

External level provides a **security mechanism through views**, meaning that different users only access the portion of data that is relevant to them from the conceptual level.

Example:

```
    +-------------+           +---------------+
    |  Customer   |           | Purchase Mgr. |
    +-------------+           +---------------+
    | Item name   |           | Item name     |
    | Price       |           | Price         |    <- External Level
    +-------------+           | Quantity      |
           ^                  +---------------+
           |                          ^
           |                          |
           +-------+          +-------+
                   |          |
              +-----------------------+
              |      Item Table       |
              | (Itemno, Itemname,    |    <- Conceptual Level
              |  Price, Quantity)     |
              +-----------------------+
                          |
                    +------------+
                    |  Internal  |          <- Internal Level
                    |   Level    |
                    +------------+
                          |
                    +----------+
                    | Database |
                    +----------+
```

Generally, from a data security point of view, the Database Administrator (DBA) creates views from the conceptual level and then gives those views to the appropriate users. Only those users are allowed to access that portion of the data in the conceptual level.

### 3. Internal Level
Indexes and clusters are available to control and manage how the data is physically stored on the hard disk.

Whenever we add indexes to a table on the database, this structure change does **not** affect the conceptual level (this is physical data independence), but the **performance will be affected** (usually improved for reads).

---

## Q. What is data independence? Explain its two types.

**Data independence** means that upper levels of the DBMS architecture are unaffected by changes made at lower levels.

DBMS architecture has two types of data independence:

### 1. Logical Data Independence
Changes made to the **conceptual level do not require changes to the external level**. This is called logical data independence.

Example: adding a new entity in the conceptual level should not affect anything in the external level.

### 2. Physical Data Independence
Changes made in the **internal level do not require changes to the conceptual level**. This is called physical data independence.

Example: adding a new index in the internal level should not affect anything in the conceptual level.

---

## Q. Explain the different data models used in the history of database design.

How data is represented at the conceptual level is defined by means of a **data model**. In the history of database design, three data models have been used:

1. Hierarchical Data Model
2. Network Data Model
3. Relational Data Model

### 1. Hierarchical Data Model (1960s onwards)

In the Hierarchical Data Model, data is organized in a **tree-like structure**. In this data model, "record type" is the same concept as "table" in the relational data model. Data is represented in the format of records.

The Hierarchical Data Model is implemented based on **one-to-many relationships** between parent and child records. Based on this relationship, many child records have a single parent record.

That is why in this data model, child records are always **repeated**. So this data model always has **more duplicate data**.

In the 1960s, IBM introduced **IMS (Information Management System)**, a product based on the Hierarchical Data Model. If we want to operate a Hierarchical Data Model product, we must use a **procedural language**.

**Hierarchical vs Relational representation (example):**

```
Hierarchical Data Model:                Relational Data Model:

         Root                           EMP (master table)
      /    |    \                       +-------+-------+-------+
   1001   1002  1003                    | empno | ename | dname |
    S/w   Admin  HR                     +-------+-------+-------+
   / \    / \    / \                    | 1001  | Sud   | S/w   |
 2013 60 2014 90 2015 98                | 1002  | Sourav| Admin |
                                        | 1003  | Sona  | HR    |
                                        +-------+-------+-------+
                                        Primary key: empno

                                        LOAD (child table)
                                        +-------+------+--------+
                                        | empno | year | amount |
                                        +-------+------+--------+
                                        | 1001  | 2013 | 60     |
                                        | 1002  | 2014 | 90     |
                                        | 1003  | 2014 | 90     |
                                        | 1003  | 2015 | 98     |
                                        +-------+------+--------+
                                        Foreign key: empno
```

### 2. Network Data Model (1970s)

In the 1970s, **CODASYL (Conference on Data System Language)** committee introduced the Network Data Model.

In this data model, there is **less duplicate data** because parent-child relationships are implemented based on **many-to-many relationships**. In this data model, data is also represented in the format of records, and "record type" is the same as "table" in the relational data model.

In 1970, IBM introduced **IDMS (Information Data Management System)** based on the Network Data Model. To operate a Network Data Model product, we also use a **procedural language**.

### 3. Relational Data Model (1970)

In 1970, **E.F. Codd** introduced the Relational Data Model. This data model consists of a collection of relations, and these relations are represented in the form of a **table**. That is why in the Relational Data Model we can store data in a 2-D table.

The Relational Data Model mainly consists of three components:
- (a) Collection of objects (tables, views, indexes, synonyms, clusters, etc.)
- (b) Set of operators
- (c) Set of integrity rules

In 1970, E.F. Codd wrote a paper titled **"A Relational Data Model For Large Shared Data Banks"** using the **DSL/Alpha** language. This paper led to the development of Normal Forms: 1NF, 2NF, 3NF.

**Timeline of relational products:**
- IBM System/R (used SQUARE language)
- INGRES (1978)
- Oracle (1977) which became Oracle Corp. (1982)

CEO of Oracle Corporation: **Larry Ellison**.

---

## Q. Explain entities, attributes, relationships, constraints, and keys in data modeling.

- **Entity** is a real-world object or thing about which data is stored. Examples: Student, Employee, Course.

- **Attribute** is a property that describes an entity. Types of attributes:
  - Simple / Composite
  - Single-valued / Multi-valued
  - Stored / Derived
  - Key attribute

- **Relationship** is an association between two or more entities. Degree of a relationship can be unary, binary, or ternary. Cardinality can be one-to-one (1:1), one-to-many (1:N), or many-to-many (M:N).

- **Constraints** are rules that restrict what data is valid. Common constraints: NOT NULL, UNIQUE, CHECK, PRIMARY KEY, FOREIGN KEY.

### Keys

- **Candidate Key** - the minimal set of attributes that can uniquely identify a tuple in a relation.
- **Primary Key** - the candidate key chosen to uniquely identify tuples in a table. It cannot be NULL.
- **Alternate Key** - the candidate keys that were not chosen as the primary key.
- **Super Key** - any set of attributes (which may include extra ones) that uniquely identifies a tuple.
- **Foreign Key** - an attribute in one table that references the primary key of another table. Enforces referential integrity.
- **Composite Key** - a primary key made of two or more attributes together.

---

## Q. Explain the components/conventions of an E-R diagram.

**E-R (Entity-Relationship) diagram notation:**

| Symbol | Meaning |
|---|---|
| Rectangle | Entity |
| Double rectangle | Weak entity |
| Ellipse | Attribute |
| Double ellipse | Multi-valued attribute |
| Dashed ellipse | Derived attribute |
| Diamond | Relationship |
| Double diamond | Identifying relationship (for weak entities) |
| Line | Connects entities to attributes/relationships |
| Underline | Key attribute |
| Dashed underline | Partial key (of a weak entity) |

---

## Q. Explain how to convert an E-R diagram into tables.

The following rules are followed:

1. **Strong entity** - each strong entity becomes one table. Its attributes become columns, and its key attribute becomes the primary key.

2. **Weak entity** - each weak entity becomes one table. Include the primary key of its owner entity as a foreign key, combined with the weak entity's own partial key to form a composite primary key.

3. **1:1 relationship** - add the primary key of either entity as a foreign key in the other entity's table.

4. **1:N relationship** - add the primary key of the "1" side as a foreign key in the table representing the "N" side.

5. **M:N relationship** - create a new separate table containing the primary keys of both participating entities (forming a composite primary key), plus any attributes of the relationship itself.

6. **Multi-valued attribute** - create a separate table containing the attribute's value along with the primary key of the owning entity.

---

## Q. Explain the EER model and how to convert an EER diagram into tables.

The **EER (Enhanced/Extended Entity-Relationship)** model extends the basic ER model with the following concepts:

- **Generalization** - combining lower-level entities that share common features into a higher-level entity. This is a **bottom-up** approach.

- **Specialization** - dividing a higher-level entity into lower-level sub-entities based on distinguishing features. This is a **top-down** approach.

- **Aggregation** - treating an entire relationship as if it were a single higher-level entity.

- **Inheritance** - sub-entities automatically inherit the attributes of the super-entity.

- **Disjoint / Overlapping** and **Total / Partial** participation constraints are used to describe specialization.

**Converting EER to tables (one table per subclass approach):**

1. Create one table for the superclass with all common attributes plus the primary key.
2. For each subclass, create a separate table containing the subclass-specific attributes plus the primary key of the superclass (which also acts as a foreign key referencing the superclass table).

---

## Q. Explain the basic concepts of the relational model, attributes, and domains.

- **Relation** - a table.
- **Tuple** - a row in a table.
- **Attribute** - a column in a table.
- **Domain** - the set of allowable/legal values that an attribute can take. Example: the domain of "age" might be positive integers less than 150.
- **Degree** of a relation - number of attributes (columns) in the relation.
- **Cardinality** of a relation - number of tuples (rows) in the relation.

---

## Q. Explain Codd's 12 rules for a truly relational database.

E.F. Codd proposed 12 rules that a database system must follow to be considered truly relational:

1. **Information Rule** - all data must be represented as values in tables.
2. **Guaranteed Access Rule** - every data value must be accessible via a combination of table name, primary key value, and column name.
3. **Systematic treatment of NULL values** - NULLs must be handled consistently and treated as distinct from zero or blank.
4. **Active online catalog** - the database description must be stored as tables (data dictionary), queryable using the same query language as normal data.
5. **Comprehensive data sublanguage rule** - one well-defined language (like SQL) must be able to define data, manipulate data, define constraints, and control access.
6. **View updating rule** - all views that are theoretically updatable must actually be updatable by the system.
7. **High-level insert, update, delete** - the system must support set-at-a-time operations, not just record-at-a-time.
8. **Physical data independence** - applications should not be affected by changes to physical storage.
9. **Logical data independence** - applications should not be affected by logical schema changes.
10. **Integrity independence** - integrity constraints must be stored in the catalog, not in application code.
11. **Distribution independence** - users should not be aware of whether the database is distributed.
12. **Non-subversion rule** - if the system provides low-level record-at-a-time access, it must not bypass integrity rules enforced at the high level.

---

## Q. Explain relational integrity, entity integrity, and referential integrity.

### NULL

**Definition:** NULL is an undefined, unknown, or unavailable value. **It is not the same as zero.**

In all databases, whenever we perform an arithmetic operation with a NULL value, the result becomes NULL.

Example: `null + 50 = null`

### Entity Integrity
Every table must have a **primary key that is unique and NOT NULL**. This ensures every row in the table can be uniquely identified.

### Referential Integrity
A **foreign key** value must either:
- Match an existing primary key value in the referenced table, or
- Be NULL

This prevents "orphan" rows that reference a row that does not exist.

### Enterprise Constraints
These are additional constraints defined by an organization's own business rules, beyond entity integrity and referential integrity. Example: "salary must be greater than 10000 for managers."

### Views and Schema Diagram

- **View** - a database object that provides a level of security. It is essentially a virtual table created from a SELECT statement on a base table. A view does not store data (except for materialized views). Details are in Unit 2.

- **Schema Diagram** - a diagram showing the entire database schema: all tables, their columns, and the primary key / foreign key relationships between them.

---

# UNIT 2: RELATIONAL QUERY LANGUAGES

---

## Q. Explain relational algebra and its operators.

**Relational algebra** is a **procedural** query language. It takes instances of relations as input and produces new relations as output. It uses a set of operators to perform queries.

### Relational Algebra Operators

| Operator | Symbol | Meaning |
|---|---|---|
| Select | σ (sigma) | Selects rows satisfying a given condition: σ<sub>condition</sub>(R) |
| Project | π (pi) | Selects specific columns from a relation: π<sub>col1,col2</sub>(R) |
| Union | ∪ | Combines tuples from two union-compatible relations (removes duplicates) |
| Set Difference | − | Tuples in R1 but not in R2 |
| Intersection | ∩ | Tuples common to both R1 and R2 |
| Cartesian Product | × | Combines every tuple of R1 with every tuple of R2 |
| Rename | ρ (rho) | Renames a relation or attribute |
| Join | ⋈ | Combines related tuples from two relations based on a condition |
| Division | ÷ | Used for queries involving "for all" semantics |

---

## Q. Explain relational calculus.

**Relational calculus** is a **non-procedural** (declarative) query language. It describes *what* is to be retrieved, not *how* to retrieve it. It has two forms:

### Tuple Relational Calculus (TRC)
Uses tuple variables. Written as:

```
{ t | P(t) }
```

meaning: "the set of all tuples `t` such that the predicate `P(t)` is true."

### Domain Relational Calculus (DRC)
Uses domain variables (values from attribute domains) rather than whole tuples. Written as:

```
{ <x1, x2, ..., xn> | P(x1, x2, ..., xn) }
```

meaning: "the set of all domain-value tuples `<x1,...,xn>` such that the predicate `P` holds."

---

## Q. Explain SQL, its characteristics, and its history.

**SQL** stands for **Structured Query Language**. SQL is a **non-procedural language** which is used to operate all relational databases.

### History of SQL
- In 1970, E.F. Codd introduced **DSL/Alpha** language, which was used to operate relational databases.
- In IBM's System/R team, a simplified version of DSL/Alpha called **"Square"** was introduced.
- Again, IBM changed "Square" to **"SEQUEL" (Structured English Query Language)**.
- Later, IBM shortened "SEQUEL" to **"SQL"**.

### Standardization
- ANSI - 1986
- ISO - 1987
- ANSI/ISO SQL versions: SQL89, SQL92, SQL99, SQL2003

### Characteristics of SQL
- Non-procedural (say what you want, not how)
- Set-oriented (operates on sets of rows at once)
- English-like syntax
- Portable across all major RDBMSs
- Can be used interactively or embedded in programs

### Advantages of SQL
- Easy to learn and use
- Works with many database vendors
- Standard (ANSI/ISO)
- Retrieves large amounts of data quickly
- Requires much less code than procedural languages

---

## Q. Explain the sub-languages of SQL.

SQL is divided into five sub-languages:

### DDL (Data Definition Language)
Used to define the structure of tables and other database objects.
- CREATE
- ALTER
- DROP
- TRUNCATE
- RENAME (Oracle 9i)

### DML (Data Manipulation Language)
Used to manipulate data within a table.
- INSERT
- UPDATE
- DELETE
- MERGE (Oracle 9i)

### DQL / DRL (Data Query / Data Retrieval Language)
Used to fetch data from the database.
- SELECT

### TCL (Transaction Control Language)
Used to manage transactions.
- COMMIT (save)
- ROLLBACK (like undo)
- SAVEPOINT

### DCL (Data Control Language)
Used to control access to data.
- GRANT (give permission)
- REVOKE (cancel permission)

**Important note:** In all databases, by default, all **DDL commands are automatically committed (saved)**. That is why we cannot roll back a DDL operation.

---

## Q. Explain Oracle SQL data types.

Data types identify the type of data that can be stored within a table column. Oracle has the following main data types:

### 1. NUMBER(P, S)

- **P** is the precision - the total number of digits allowed.
- **S** is the scale - the number of digits after the decimal point.

Used to store fixed or floating-point numbers. Maximum precision is **38 digits**.

Syntax:
```sql
columnname NUMBER(P, S)
```

**Rule:** The maximum digits allowed *before* the decimal point is `P - S`. If we try to insert more digits before the decimal than allowed, Oracle throws an error. If we try to insert *more* digits *after* the decimal point than `S` allows, Oracle **automatically rounds** based on the scale, and does not throw an error.

Example:
```sql
CREATE TABLE test (sno NUMBER(7,2));
INSERT INTO test VALUES (12345.67);       -- OK
INSERT INTO test VALUES (123456.7);        -- ERROR: value larger than specified precision
INSERT INTO test VALUES (12345.6789);      -- Stored as 12345.68 (auto-rounded)
```

### 2. NUMBER(P)
Used to store fixed (whole) numbers only.
```sql
CREATE TABLE test1 (sno NUMBER(3));
INSERT INTO test1 VALUES (100);     -- 100
INSERT INTO test1 VALUES (99.4);    -- Stored as 99 (rounded)
```

### 3. CHAR(size)
Used to store **fixed-length** alphanumeric data in bytes. Maximum limit is **2000 bytes**. Default size is 1 byte.

When we store fewer bytes than the specified size, Oracle **automatically adds blank spaces** to fill the remaining bytes at the end of the string. This is called the **blank-padded mechanism**.

Example:
```
CHAR(10) storing 'Murali':
+---+---+---+---+---+---+---+---+---+---+
| M | u | r | a | l | i |   |   |   |   |
+---+---+---+---+---+---+---+---+---+---+
                        <---blank padded--->
```

To overcome the disk-space waste caused by blank padding, Oracle introduced VARCHAR2.

### 4. VARCHAR2(size)
Introduced in Oracle 7.0. Used to store **variable-length** alphanumeric data in bytes. Maximum size is **4000 bytes**.

When we store fewer bytes than the specified size, Oracle does **not** add blank spaces at the end of the string. That is why this data type does not waste disk space.

### 5. VARCHAR(size)
Prior to Oracle 7.0, Oracle had the VARCHAR data type. It is the same as VARCHAR2 but the maximum variable length is only 2000 bytes.

### 6. DATE
Used to store dates. In Oracle, the default date format is `DD-MON-YY`.

Syntax:
```sql
columnname DATE
```

Example:
```sql
INSERT INTO emp VALUES ('10-JAN-25');
```

---

## Q. Explain DDL commands with syntax and examples.

### 1. CREATE
Used to create database objects like tables, views, synonyms, indexes, etc.

**Creating a table:**
```sql
CREATE TABLE tablename (
    columnname1 datatype(size),
    columnname2 datatype(size),
    ...
);

-- Example
CREATE TABLE first (sno NUMBER(10), name VARCHAR2(10));
```

**To view the structure of a table:**
```sql
DESC tablename;

DESC first;
-- Output:
-- Name    Type
-- ------  -------------
-- SNO     NUMBER(10)
-- NAME    VARCHAR2(10)
```

### 2. ALTER
Used to change the structure of an existing table. ALTER has three forms:

```
              ALTER
        /       |       \
      ADD    MODIFY     DROP
```

**ADD** - used to add columns to an existing table.
```sql
ALTER TABLE tablename ADD (col1 datatype(size), col2 datatype(size), ...);

ALTER TABLE first ADD sal NUMBER(10);
```

**MODIFY** - used to change the datatype or datatype size of a column only.
```sql
ALTER TABLE tablename MODIFY (col1 datatype(size), ...);

ALTER TABLE first MODIFY sno DATE;
ALTER TABLE first MODIFY sno VARCHAR2(10);
```

**DROP** - used to drop columns from a table.
```sql
-- Method 1 (single column, without parentheses):
ALTER TABLE tablename DROP COLUMN columnname;
ALTER TABLE first DROP COLUMN sno;

-- Method 2 (single or multiple columns, with parentheses):
ALTER TABLE tablename DROP (col1, col2, ...);
ALTER TABLE first DROP (name);
```

**Rule:** In all databases, we cannot drop all columns in a table.
```sql
ALTER TABLE first DROP COLUMN sal;
-- ERROR: cannot drop all columns in a table
```

### 3. DROP
Used to remove database objects from the database.
```sql
DROP object objectname;
DROP TABLE tablename;
DROP VIEW viewname;
```

**Before Oracle 10g:** dropped tables were removed permanently.

**Oracle 10g, 11g, 12c (Enterprise Edition):** dropped tables are moved to the **Recycle Bin** first, and can be recovered.

```
Oracle Database                    Oracle Database
+-------------+                    +-------------+
|  Table      |                    |  Table      |
|  +------+   |                    |  +------+   |
|  |      |------> permanently     |  |      |----+
|  +------+   |    removed         |  +------+   |
+-------------+                    |             |
                                   |    Recycle  |
Before Oracle 10g                  |    Bin      |
                                   +-------------+
                                     Oracle 10g/11g/12c
```

**Get back from recycle bin:**
```sql
FLASHBACK TABLE tablename TO BEFORE DROP;
```

**To drop permanently (skip recycle bin, or clean it up):**
```sql
DROP TABLE tablename PURGE;      -- drops directly without going to recycle bin
PURGE TABLE tablename;            -- removes one table from recycle bin
PURGE RECYCLEBIN;                 -- empties whole recycle bin
```

### About the Recycle Bin
- Oracle 10g introduced the Recycle Bin, which stores dropped tables.
- The Recycle Bin is a predefined **read-only** table.
- Whenever we install Oracle server, so many predefined read-only tables are automatically created. These are also called the **Data Dictionary**.

```sql
DESC recyclebin;
SELECT original_name FROM recyclebin;
-- ORIGINAL_NAME
-- -------------
-- FIRST
```

### 4. TRUNCATE
Oracle 7.0 introduced TRUNCATE. Used to delete all rows permanently from the table.

Syntax:
```sql
TRUNCATE TABLE tablename;
```

Example:
```sql
CREATE TABLE first AS SELECT * FROM emp;
SELECT * FROM first;      -- shows all emp rows
TRUNCATE TABLE first;
SELECT * FROM first;      -- no rows selected
DESC first;               -- structure still exists
```

### 5. RENAME (Oracle 9i)
Used to rename a table.
```sql
RENAME oldtablename TO newtablename;
RENAME first TO second;
```

**Renaming a column (Oracle 9i):**
```sql
ALTER TABLE tablename RENAME COLUMN oldcolumnname TO newcolumnname;
ALTER TABLE emp RENAME COLUMN empno TO sno;
```

---

## Q. Difference between DELETE and TRUNCATE.

| DELETE | TRUNCATE |
|---|---|
| DML command | DDL command |
| When we use DELETE, deleted data is internally stored in a buffer, so we can get it back using ROLLBACK | When we use TRUNCATE, all rows are permanently deleted and cannot be recovered using ROLLBACK, because TRUNCATE is a DDL command and DDL commands are automatically committed |
| Can delete specific rows using WHERE clause | Deletes all rows only |
| Slower for large tables | Faster |

---

## Q. Explain DML commands with syntax and examples.

DML commands are used to manipulate data within a table. There are four DML commands:
1. INSERT
2. UPDATE
3. DELETE
4. MERGE

### 1. INSERT
Used to insert data into a table.

**Method 1: direct values**
```sql
INSERT INTO tablename VALUES (value1, value2, ...);

INSERT INTO first VALUES (1, 'murali');
INSERT INTO first VALUES (2, 'gokul');
```

**Method 2: using substitution operator (&)**

The `&` operator asks the user to enter a value at runtime.
```sql
INSERT INTO tablename VALUES (&columnname1, '&columnname2');

INSERT INTO first VALUES (&sno, '&name');
-- Enter value for sno: 3
-- Enter value for name: abc

-- Typing "/" re-runs the previous command:
/
-- Enter value for sno: 4
-- Enter value for name: sud
```

**Method 3: skipping columns**

Insert data into specific columns only. Other columns will be NULL.
```sql
INSERT INTO tablename (col1, col2, ...) VALUES (val1, val2, ...);

INSERT INTO first (name) VALUES ('zzz');
```

### 2. UPDATE
Used to change data in a table.

Syntax:
```sql
UPDATE tablename SET columnname = newvalue WHERE condition;
```

Example:
```sql
UPDATE emp SET sal = 1000 WHERE ename = 'SMITH';
-- 1 row updated
```

**Note:** In all databases, we can also use UPDATE to insert data into a particular empty (NULL) cell, or to clear a value:
```sql
UPDATE first SET address = 'Mumbai' WHERE name = 'sud';
UPDATE first SET address = NULL WHERE name = 'sud';   -- clears value
```

### 3. DELETE
Used to delete rows (all or specific) from a table.

Syntax:
```sql
DELETE FROM tablename;                       -- deletes ALL rows
DELETE FROM tablename WHERE condition;       -- deletes specific rows
```

Example (recovery after DELETE):
```sql
DELETE FROM first;
-- rows deleted

ROLLBACK;               -- recovers all deleted rows
SELECT * FROM first;    -- rows are back
```

---

## Q. Explain the SELECT statement (DQL) with syntax.

**SELECT** command is used to fetch data from the database.

### Basic syntax
```sql
SELECT * FROM tablename;
```

### Full syntax
```sql
SELECT col1, col2, ...
FROM tablename
WHERE condition
GROUP BY columnname
HAVING condition
ORDER BY columnname [ASC | DESC];
```

### Four basic forms of SELECT

1. Select all columns and all rows:
```sql
SELECT * FROM emp;
```

2. Select all columns and particular rows (WHERE condition returns true or false):
```sql
SELECT * FROM emp WHERE deptno = 10;
```

3. Select particular columns and all rows:
```sql
SELECT ename, sal FROM emp;
```

4. Select particular columns and particular rows:
```sql
SELECT ename, sal FROM emp WHERE deptno = 10;
```

### Creating a new table from an existing table (copying)

```sql
CREATE TABLE newtable AS SELECT * FROM existingtable;

CREATE TABLE diwali AS SELECT * FROM emp;
```

**Note:** When copying a table from another table, constraints (primary key, foreign key, etc.) are **never copied**.

### Creating a new table from an existing table without copying data (structure only)

Use a false condition:
```sql
CREATE TABLE newtable AS SELECT * FROM existingtable WHERE 1=2;

CREATE TABLE abc AS SELECT * FROM emp WHERE 1=2;
SELECT * FROM abc;     -- no rows selected
DESC abc;              -- structure only, no data
```

---

## Q. Explain the operators used in SELECT statements.

There are four types of operators used in SELECT:

1. Arithmetic operator: `+`, `-`, `*`, `/`
2. Relational operator: `=`, `<`, `>`, `>=`, `<=`, `<>` (or `!=`)
3. Logical operator: AND, OR, NOT
4. Special operator: IN, BETWEEN, IS NULL, LIKE (and their opposites)

Arithmetic operators are used in **number** and **date** datatype columns.

### Arithmetic operator example (column alias)

```sql
SELECT ename, sal, sal*12 annsal FROM emp;
--                        ^
--                        column alias name (annsal)
-- Output:
-- ENAME    SAL     ANNSAL
-- SMITH    2200    26400
-- ALLEN    300     3600
```

### Relational operator examples

```sql
-- Employees except JOB = CLERK
SELECT * FROM emp WHERE job <> 'CLERK';

-- Employees earning more than 2000
SELECT * FROM emp WHERE sal > 2000;
```

### Logical operator: AND

Whenever we use AND, the database server **filters** the data from the result set (rows must match ALL conditions).

```sql
-- CLERKs having salary more than 2000
SELECT * FROM emp WHERE job = 'CLERK' AND sal > 2000;
```

### Logical operator: OR

Whenever we use OR, the database server returns data based on **each individual condition** (rows matching any one condition).

```sql
SELECT * FROM emp WHERE job = 'CLERK' OR sal > 2000;
```

**Rule:** If we want to retrieve multiple values from a single column based on equality, we must use the logical operator OR (or the IN operator).

```sql
SELECT * FROM emp WHERE job = 'CLERK' OR job = 'SALESMAN';

-- Or with dept numbers:
SELECT * FROM emp
WHERE deptno = 20 OR deptno = 30 OR deptno = 50
   OR deptno = 70 OR deptno = 80;
```

---

## Q. Explain special operators: IN, BETWEEN, IS NULL, LIKE.

### 1. IN

Used to pick a value one by one from a list of values. Used in place of multiple OR operators. **Performance of IN is much higher than OR** when retrieving multiple values from a single column.

Syntax:
```sql
SELECT * FROM tablename WHERE columnname IN (list of values);
```

Examples:
```sql
SELECT * FROM emp WHERE deptno IN (20, 30, 50, 70, 80);
SELECT * FROM emp WHERE ename IN ('SMITH', 'ALLEN', 'MILLER');
```

**Note:** In all databases, the `NOT IN` operator does not work with NULL values.
```sql
SELECT * FROM emp WHERE deptno NOT IN (10, 20, null);
-- no rows selected
```

### 2. BETWEEN

Used to retrieve a range of values (inclusive on both ends).

Syntax:
```sql
SELECT * FROM tablename WHERE columnname BETWEEN lowvalue AND highvalue;
```

Examples:
```sql
-- Employees earning salary between 2000 and 5000
SELECT * FROM emp WHERE sal BETWEEN 2000 AND 5000;

-- Salary NOT between 2000 and 5000
SELECT * FROM emp WHERE sal NOT BETWEEN 2000 AND 5000;
```

### 3. IS NULL, IS NOT NULL

Used only in WHERE condition to test whether a column has a NULL value or not.

**Note:** In all databases, we are **not allowed to use relational operators** (=, <, >) with NULL values in a WHERE condition. We must use `IS NULL` and `IS NOT NULL`.

Syntax:
```sql
SELECT * FROM tablename WHERE columnname IS NULL;
SELECT * FROM tablename WHERE columnname IS NOT NULL;
```

Examples:
```sql
-- Employees who do NOT get commission
SELECT * FROM emp WHERE comm IS NULL;

-- Employees who DO get commission
SELECT * FROM emp WHERE comm IS NOT NULL;
```

### 4. LIKE

Used to search data based on a character pattern. LIKE operator performance is very high compared to predefined searching functions.

Two special operators (wildcards) are used with LIKE:

- `%` (percent) - matches a string or group of characters (any length, including zero)
- `_` (underscore) - matches exactly one character

Syntax:
```sql
SELECT * FROM tablename WHERE columnname LIKE 'character pattern';
```

Examples:

```sql
-- Employees whose ename starts with M
SELECT * FROM emp WHERE ename LIKE 'M%';
-- Output: MARTIN, MILLER

-- Employees whose ename has M in any position
SELECT * FROM emp WHERE ename LIKE '%M%';
-- Output: SMITH, MARTIN, ADAMS, JAMES, MILLER

-- Employees whose ename second letter is L
SELECT * FROM emp WHERE ename LIKE '_L%';
-- Output: ALLEN, BLAKE, CLARK

-- Employees whose ename 4th letter is M
SELECT * FROM emp WHERE ename LIKE '___M%';

-- Employees joining in December
SELECT * FROM emp WHERE hiredate LIKE '%DEC%';

-- Employees joining in year 81
SELECT * FROM emp WHERE hiredate LIKE '%81';
```

### ESCAPE clause with LIKE

Sometimes column data itself contains wildcard characters (`%` or `_`). Then LIKE gives wrong results because Oracle treats them as wildcards. To solve this, Oracle provides the **ESCAPE** function.

Example: suppose we have an employee named `S_MITH` in the emp table.
```sql
SELECT * FROM emp WHERE ename LIKE 'S_%';
-- Output: SCOTT, SMITH, S_MITH   <- WRONG! we only wanted S_MITH

-- Correct with ESCAPE:
SELECT * FROM emp WHERE ename LIKE 'S?_%' ESCAPE '?';
-- Output: S_MITH
```

The escape character (`?` here) tells Oracle: "the character right after me is a literal character, not a wildcard."

**Note:** Escape character length must be exactly 1 byte.

---

## Q. Explain the concatenation operator (||).

If we want to display column data along with a literal string, we use the concatenation operator `||`.

Syntax:
```sql
SELECT 'literal text' || columnname FROM tablename;
```

Example:
```sql
SELECT 'my employee name is: ' || ename FROM emp;
-- Output:
-- my employee name is: SMITH
-- my employee name is: ALLEN

SELECT ename || ' ' || sal FROM emp;
-- Output:
-- SMITH 2200
-- ALLEN 300
```

We use the concatenation operator to display our own spaces or literal strings between columns.

---

## Q. Explain NVL and NVL2 functions with examples.

### NVL()

**Definition:** NVL is a predefined function used to replace or substitute a user-defined value in place of NULL.

Syntax:
```sql
NVL(exp1, exp2)
```

Here `exp1` and `exp2` must belong to the same datatype. If `exp1` is NULL, it returns `exp2`; otherwise it returns `exp1`.

Examples:
```sql
SELECT NVL(null, 20) FROM dual;    -- Output: 20
SELECT NVL(10, 20) FROM dual;      -- Output: 10
```

**Use case:** any arithmetic operation performed with a NULL value produces NULL. NVL fixes this.

```sql
-- Without NVL:
SELECT ename, sal, comm, sal+comm FROM emp WHERE ename='SMITH';
-- ENAME  SAL    COMM   SAL+COMM
-- SMITH  2200   null   null       <-- wrong: we wanted 2200

-- With NVL:
SELECT ename, sal, comm, sal + NVL(comm, 0) FROM emp WHERE ename='SMITH';
-- ENAME  SAL    COMM   SAL+NVL(COMM,0)
-- SMITH  2200   null   2200        <-- correct
```

### NVL2() (Oracle 9i)

**Definition:** NVL2 is a predefined function used to replace or substitute a user-defined value in place of NULL. It accepts **three parameters**.

Syntax:
```sql
NVL2(exp1, exp2, exp3)
```

If `exp1` is NULL, it returns `exp3`; otherwise it returns `exp2`.

Examples:
```sql
SELECT NVL2(null, 10, 20) FROM dual;    -- Output: 20
SELECT NVL2(30, 10, 20) FROM dual;      -- Output: 10
```

**Use case:** update the employee's commission using the rule:
- If `comm` is null, update `comm` to 500.
- If `comm` is not null, update `comm` to `(comm + 500)`.

```sql
UPDATE emp SET comm = NVL2(comm, comm+500, 500);
```

### DUAL Table

**DUAL** is a predefined virtual table which contains one column and one row. It is used to test predefined and user-defined function results.

```sql
SELECT NVL(null, 10) FROM dual;
-- Output: 10

SELECT ABS(-50) FROM dual;
-- Output: 50
```

---

## Q. Explain the different types of functions in Oracle.

Functions are used to solve a particular task and always return a value.

Oracle has two types of functions:
1. **Predefined functions** (built into Oracle)
2. **User-defined functions** (written by the user)

### Predefined functions are categorized as:
(a) Number functions
(b) Character functions
(c) Date functions
(d) Group / Aggregate functions

---

## Q. Explain number functions with examples.

Number functions operate over number data.

| Function | Description | Example | Output |
|---|---|---|---|
| `ABS(n)` | Converts negative sign to positive | `ABS(-50)` | 50 |
| `ROUND(n, d)` | Rounds n to d decimal places | `ROUND(45.926, 2)` | 45.93 |
| `TRUNC(n, d)` | Truncates n to d decimal places (no rounding) | `TRUNC(45.926, 2)` | 45.92 |
| `MOD(m, n)` | Remainder when m is divided by n | `MOD(10, 3)` | 1 |
| `POWER(m, n)` | m raised to the power n | `POWER(2, 3)` | 8 |
| `SQRT(n)` | Square root | `SQRT(16)` | 4 |

Example:
```sql
SELECT ABS(-50) FROM dual;    -- 50
```

---

## Q. Explain character functions with examples.

| Function | Description | Example |
|---|---|---|
| `LENGTH(str)` | Returns number of characters | `LENGTH('SMITH')` returns 5 |
| `UPPER(str)` | Converts to uppercase | `UPPER('abc')` returns 'ABC' |
| `LOWER(str)` | Converts to lowercase | `LOWER('ABC')` returns 'abc' |
| `INITCAP(str)` | Capitalizes first letter of each word | `INITCAP('smith')` returns 'Smith' |
| `SUBSTR(str, pos, len)` | Extracts substring starting at pos, of length len | `SUBSTR('COMPUTER', 2, 2)` returns 'OM' |
| `INSTR(str, 'ch', pos, occ)` | Returns position of a character/substring | `INSTR('ABC*D', '*')` returns 4 |
| `LPAD(str, total_len, 'ch')` | Left-pads string with ch to total_len | `LPAD('ABCD', 10, '#')` returns '######ABCD' |
| `RPAD(str, total_len, 'ch')` | Right-pads string with ch to total_len | `RPAD('ABCD', 10, '#')` returns 'ABCD######' |
| `LTRIM(str, 'set')` | Removes characters from the left | `LTRIM('SSMISSTHSS', 'S')` returns 'MISSTHSS' |
| `RTRIM(str, 'set')` | Removes characters from the right | `RTRIM('SSMISSTHSS', 'S')` returns 'SSMISSTH' |
| `TRIM('ch' FROM 'str')` | Removes ch from both ends | `TRIM('S' FROM 'SSTHSSMISS')` returns 'THSSMI' |
| `TRANSLATE(s1, s2, s3)` | Replaces character-by-character | `TRANSLATE('india', 'in', 'xy')` returns 'xydxa' |
| `REPLACE(s1, s2, s3)` | Replaces string by string | `REPLACE('india', 'in', 'xy')` returns 'xydia' |
| `CONCAT(s1, s2)` | Joins two strings | `CONCAT('wel', 'come')` returns 'welcome' |

### Examples

**LENGTH and SUBSTR:**
```sql
-- Employees whose 2nd letter of ename is 'LA'
SELECT * FROM emp WHERE SUBSTR(ename, 2, 2) = 'LA';

-- Employees whose ename is 5 characters long
SELECT * FROM emp WHERE LENGTH(ename) = 5;
-- Output: SMITH, CLARK
```

**Note:** In all databases, we are **not allowed to use group functions in the WHERE clause**, but we are allowed to use number(), character(), and date() functions in the WHERE clause.

```sql
SELECT * FROM emp WHERE sal = MAX(sal);
-- ERROR: group function is not allowed here
```

**INSTR:** always returns the position of a delimiter, character, or string within a given string.

Full syntax:
```
INSTR(columnname or 'string', 'search_str', start_position, occurrence)
                                                  ^              ^
                                            +ve or -ve      number
```

Examples:
```sql
SELECT INSTR('ABC*D', '*') FROM dual;                     -- 4
SELECT INSTR('AB*CDEFGHCDIJKLCDMNP', 'CD', -6, 2) FROM dual;   -- 3
-- (searches from right, prints position from left)
```

**LPAD and RPAD:** fill remaining spaces with a specified character on the left or right side.

```sql
SELECT LPAD('ABCD', 10, '#') FROM dual;
-- Output: ######ABCD    (left-side padding)

SELECT RPAD('ABCD', 10, '#') FROM dual;
-- Output: ABCD######    (right-side padding)
```

If the string is already longer than the given total length, LPAD and RPAD return the original string truncated from the left onwards.

**LTRIM, RTRIM, TRIM:**

```sql
SELECT LTRIM('SSMISSTHSS', 'S') FROM dual;   -- MISSTHSS
SELECT RTRIM('SSMISSTHSS', 'S') FROM dual;   -- SSMISSTH
SELECT TRIM('S' FROM 'SSTHSSMISS') FROM dual;   -- THSSMI (removes from both ends)

-- Convert TRIM to LTRIM:
SELECT TRIM(LEADING 'S' FROM 'SSMISSTHSS') FROM dual;    -- MISSTHSS
-- Convert TRIM to RTRIM:
SELECT TRIM(TRAILING 'S' FROM 'SSMISSTHSS') FROM dual;   -- SSMISSTH
```

**TRANSLATE vs REPLACE:**

```sql
SELECT TRANSLATE('india', 'in', 'xy') FROM dual;
-- Output: xydxa   (character-by-character: i->x, n->y)

SELECT REPLACE('india', 'in', 'xy') FROM dual;
-- Output: xydia   (whole substring 'in' replaced with 'xy')

SELECT TRANSLATE('ABCDEF', 'FEDCBA', '123456') FROM dual;
-- Output: 654321

SELECT REPLACE('A B C', ' ', 'PQR') FROM dual;
-- Output: APQRBPQRC

SELECT REPLACE('SSMISSTHSS', 'S') FROM dual;
-- Output: MITH   (when 3rd argument omitted, 2nd arg is removed)
```

If we do not specify the third parameter in REPLACE, the second parameter's value is permanently removed from the given string.

**Query using REPLACE and LENGTH:** count occurrences of a character in a string.
```sql
-- Count occurrences of 'E' in 'SLEEP':
SELECT LENGTH('SLEEP') - LENGTH(REPLACE('SLEEP', 'E')) FROM dual;
-- Output: 2
```

**CONCAT:**
```sql
SELECT CONCAT('wel', 'come') FROM dual;
-- Output: welcome
```

---

## Q. Explain date functions with examples.

In Oracle, the default date format is `DD-MON-YY`. Oracle has the following date functions:

1. SYSDATE
2. ADD_MONTHS()
3. LAST_DAY()
4. NEXT_DAY()
5. MONTHS_BETWEEN()

### 1. SYSDATE
Returns the current system date in Oracle date format.
```sql
SELECT SYSDATE FROM dual;
-- Output: 17-NOV-15
```

### 2. ADD_MONTHS()
Adds or removes a number of months from the specified date, based on the second parameter.

Syntax:
```sql
ADD_MONTHS(date, number)
```
Number: +ve to add, -ve to subtract.

```sql
SELECT ADD_MONTHS(SYSDATE, 5) FROM dual;
-- Output: 17-APR-16 (assuming SYSDATE is 17-NOV-15)

SELECT ADD_MONTHS(SYSDATE, -1) FROM dual;
-- Output: 17-OCT-15
```

### 3. LAST_DAY()
Returns the last date of the specified month.
```sql
SELECT LAST_DAY(SYSDATE) FROM dual;
-- Output: 30-NOV-15
```

### 4. NEXT_DAY()
Returns the next occurrence of the day mentioned in the second parameter, from the specified date.

Syntax:
```sql
NEXT_DAY(date, 'day')
```

Example:
```sql
-- SYSDATE = 17-NOV-15 (Tuesday)
SELECT NEXT_DAY(SYSDATE, 'Monday') FROM dual;
-- Output: 23-NOV-15
```

### 5. MONTHS_BETWEEN()
Returns the number of months between two specified dates.

Syntax:
```sql
MONTHS_BETWEEN(date1, date2)
```

**Note:** date1 should be greater than date2, otherwise the function returns a negative sign.

```sql
SELECT ename, ROUND(MONTHS_BETWEEN(SYSDATE, hiredate)) FROM emp;
```

### Date Arithmetic

| Operation | Allowed? |
|---|---|
| Date + Number | Yes |
| Date - Number | Yes |
| Date1 + Date2 | NOT allowed |
| Date1 - Date2 | Yes (returns number of days) |

Examples:
```sql
SELECT SYSDATE + 1 FROM dual;
SELECT SYSDATE - 1 FROM dual;
SELECT SYSDATE - SYSDATE FROM dual;    -- Output: 0
```

**Q. WAQ to display first date of the current month using SYSDATE, ADD_MONTHS, LAST_DAY.**
```sql
SELECT LAST_DAY(ADD_MONTHS(SYSDATE, -1)) + 1 FROM dual;
-- Output: 01-NOV-15
```

---

## Q. Explain date conversion functions: TO_CHAR and TO_DATE.

Oracle has two date conversion functions:
1. TO_CHAR()
2. TO_DATE()

### 1. TO_CHAR()
Used to convert Oracle date type into character type (converts date type into date string).

Syntax:
```sql
TO_CHAR(date, 'format')
```

Examples:
```sql
SELECT TO_CHAR(SYSDATE, 'DD/MM/YY') FROM dual;
-- Output: 18/11/15
```

**Important:** TO_CHAR is a **case-sensitive** function. The character format is case-sensitive.

```sql
SELECT TO_CHAR(SYSDATE, 'DAY') FROM dual;
-- Output: WEDNESDAY

SELECT TO_CHAR(SYSDATE, 'day') FROM dual;
-- Output: wednesday
```

### Common Date Format Codes

**Character formats:**

| Code | Meaning |
|---|---|
| `D` | Day of the week (Sunday=1, Monday=2, ...) |
| `DD` | Day of the month |
| `DDD` | Day of the year |
| `MM` | Month (number) |
| `MON` | Month (abbreviated, e.g., DEC) |
| `MONTH` | Month (full name, e.g., DECEMBER) |
| `YY` | Year (2 digits) |
| `YYYY` | Year (4 digits) |
| `YEAR` | Year spelled out |
| `DY` | Abbreviated day name (e.g., WED) |
| `DAY` | Full day name |
| `HH` | Hour (12-hour clock) |
| `HH24` | Hour (24-hour clock) |
| `MI` | Minutes |
| `SS` | Seconds |
| `DDTH` | Ordinal day (e.g., 18TH) |
| `DDSPTH` | Spelled ordinal (e.g., EIGHTEENTH) |

Examples:
```sql
SELECT TO_CHAR(SYSDATE, 'D') FROM dual;         -- 4 (day of week)
SELECT TO_CHAR(SYSDATE, 'DDD') FROM dual;       -- 322 (day of year)
SELECT TO_CHAR(SYSDATE, 'DD') FROM dual;        -- 18 (day of month)
SELECT TO_CHAR(SYSDATE, 'DDTH') FROM dual;      -- 18TH
SELECT TO_CHAR(SYSDATE, 'DDSPTH') FROM dual;    -- EIGHTEENTH
SELECT TO_CHAR(SYSDATE, 'HH:MI:SS') FROM dual;  -- 02:51:33
SELECT TO_CHAR(SYSDATE, 'HH24:MI:SS') FROM dual; -- 14:51:33
```

### 2. TO_DATE()
Used to convert a date string into Oracle date type.

Syntax:
```sql
TO_DATE('date_string', 'format')
```

Examples:
```sql
SELECT TO_DATE('12/june/05') FROM dual;
-- Output: 12-JUN-05

SELECT TO_DATE('12/06/05') FROM dual;
-- ERROR: not a valid month
```

**Rule:** whenever using TO_DATE, the passed value must match the default date format, otherwise Oracle returns an error. To fix this, use a second parameter matching the string's format:

```sql
SELECT TO_DATE('12/06/05', 'DD/MM/YY') FROM dual;
-- Output: 12-JUN-05
```

### FM (Fill Mode)

TO_CHAR normally pads MONTH/DAY output with trailing spaces. FM suppresses leading zeros and trailing spaces.

```sql
SELECT TO_CHAR(TO_DATE('12-June-05'), 'DD/MONTH/YY') FROM dual;
-- Output: 12/JUNE     /15

SELECT TO_CHAR(TO_DATE('12-June-05'), 'DD/FMMONTH/YYYY') FROM dual;
-- Output: 12/JUNE/2015
```

### Query examples using TO_CHAR

```sql
-- Employees who are joining in December
SELECT * FROM emp WHERE TO_CHAR(hiredate, 'MM') = '12';
-- OR
SELECT * FROM emp WHERE TO_CHAR(hiredate, 'MON') = 'DEC';

-- Employees who are joining in DECEMBER (using MONTH format needs FM to avoid padding)
SELECT * FROM emp WHERE TO_CHAR(hiredate, 'FMMONTH') = 'DECEMBER';

-- Employees joining in year 81
SELECT * FROM emp WHERE TO_CHAR(hiredate, 'YY') = '81';

SELECT hiredate, TO_CHAR(hiredate, 'YYYY') FROM emp;
-- HIREDATE     TO_CHAR
-- 17-DEC-80    1980
-- 20-FEB-81    1981
```

**Rule:** Whenever we pass a date string into a predefined date function, Oracle **automatically** converts the date string into date type. In this case we do not need TO_DATE explicitly, but the passed value must be in Oracle date format.

```sql
-- Automatic conversion:
SELECT LAST_DAY('15-AUG-05') FROM dual;
-- Output: 31-AUG-05

-- Explicit conversion needed:
SELECT LAST_DAY('15-08-15') FROM dual;
-- ERROR

SELECT LAST_DAY(TO_DATE('15-08-15', 'DD-MM-YY')) FROM dual;
-- Output: 31-AUG-15
```

### ROUND and TRUNC with Dates

In Oracle date type contains both date and time. When we use ROUND or TRUNC on dates, the date part can be changed based on the time portion, and the time portion is automatically set to zero.

Whenever we use ROUND on a date:
- If the time portion is greater than or equal to 12 noon, Oracle server automatically **adds one day** to the given date.
- The time portion is automatically set to zeros.

Whenever we use TRUNC on a date:
- Oracle server **does not add one day**, even if time is >= 12 noon.
- The time portion is set to zero.

Example (at 2:30 PM, SYSDATE = 20-NOV-15 14:33:17):
```sql
SELECT TO_CHAR(SYSDATE, 'DD-MM-YY HH24:MI:SS') FROM dual;
-- Output: 20-11-15 14:33:17

SELECT TO_CHAR(ROUND(SYSDATE), 'DD-MON-YY HH24:MI:SS') FROM dual;
-- Output: 21-NOV-15 00:00:00

SELECT TO_CHAR(TRUNC(SYSDATE), 'DD-MON-YY HH24:MI:SS') FROM dual;
-- Output: 20-NOV-15 00:00:00
```

Also, ROUND and TRUNC can return the first date of the year or the first date of the month using format arguments:

```sql
ROUND(date, 'YEAR')      -- rounds to nearest Jan 1
ROUND(date, 'MONTH')     -- rounds to nearest 1st of the month
TRUNC(date, 'YEAR')      -- 1st Jan of the same year
TRUNC(date, 'MONTH')     -- 1st of the same month
```

Rule for ROUND on YEAR: Oracle checks if the given month is above 50% (Jul-Dec) or below 50% (Jan-Jun). If above 50%, it adds one year and rounds to Jan 1.

```sql
-- SYSDATE = 20-NOV-15
SELECT ROUND(SYSDATE, 'YEAR') FROM dual;    -- 01-JAN-16
SELECT ROUND(SYSDATE, 'MONTH') FROM dual;   -- 01-DEC-15
SELECT ROUND(SYSDATE, 'DAY') FROM dual;     -- 22-NOV-15 (nearest Sunday)
SELECT TRUNC(SYSDATE, 'YEAR') FROM dual;    -- 01-JAN-15
SELECT TRUNC(SYSDATE, 'MONTH') FROM dual;   -- 01-NOV-15
SELECT TRUNC(SYSDATE, 'DAY') FROM dual;     -- 15-NOV-15
```

### Comparing dates

Oracle internally compares dates based on both the date part and the time portion.

```sql
-- Employees joining today - this may return no rows because of time comparison:
SELECT * FROM emp WHERE hiredate = SYSDATE;

-- Correct: compare after truncating both:
SELECT * FROM emp WHERE TRUNC(hiredate) = TRUNC(SYSDATE);
```

---

## Q. Explain group (aggregate) functions with examples.

In all databases, group functions operate over multiple values in a column and return a single value.

Oracle has the following group functions:
1. MAX()
2. MIN()
3. AVG()
4. SUM()
5. COUNT(*)
6. COUNT(columnname)

### 1. MAX()
Returns the maximum value from a column.
```sql
SELECT MAX(sal) FROM emp;                -- 7000
SELECT MAX(hiredate) FROM emp;            -- 23-MAY-87
SELECT MAX(ename) FROM emp;               -- WARD
```

### 2. MIN()
Returns the minimum value from a column.
```sql
SELECT MIN(sal) FROM emp;                -- 500
SELECT MIN(hiredate) FROM emp;            -- 17-DEC-80
```

**Note:** In all databases, we are not allowed to use group functions in the WHERE clause.
```sql
SELECT * FROM emp WHERE sal = MIN(sal);
-- ERROR: group function is not allowed here
```

### 3. AVG()
Returns average from a number column.
```sql
SELECT AVG(sal) FROM emp;                 -- 2848.21429
SELECT AVG(comm) FROM emp;                -- 550
```

**Note:** By default, all group functions **ignore NULL values**, except `COUNT(*)`.
```sql
SELECT AVG(NVL(comm, 0)) FROM emp;        -- 157.14287 (nulls treated as 0)
```

### 4. SUM()
Returns total from a number column.
```sql
SELECT SUM(sal) FROM emp;                 -- 39875
```

### 5. COUNT(*)
Counts the total number of rows in the table (including nulls).
```sql
SELECT COUNT(*) FROM emp;                 -- 14
```

### 6. COUNT(columnname)
Counts the number of NON-NULL values in a column.
```sql
SELECT COUNT(comm) FROM emp;              -- 4 (only 4 employees have non-null commission)
SELECT COUNT(mgr) FROM emp;               -- 13

SELECT DISTINCT deptno FROM emp;          -- 10, 20, 30
SELECT COUNT(DISTINCT deptno) FROM emp;   -- 3
```

---

## Q. Explain GROUP BY clause with examples.

**GROUP BY** clause is used to arrange similar data items into sets of logical groups. Whenever we use GROUP BY, the database server selects similar data items from a table column and then reduces the number of data items in each group.

Syntax:
```sql
SELECT columnname, ... FROM tablename GROUP BY columnname;
```

Examples:

```sql
-- Number of employees in each department
SELECT deptno, COUNT(*) FROM emp GROUP BY deptno;
-- DEPTNO   COUNT(*)
-- 10       3
-- 20       5
-- 30       6

-- Number of employees in each job
SELECT job, COUNT(*) FROM emp GROUP BY job;
-- JOB       COUNT(*)
-- CLERK     4
-- SALESMAN  4
-- PRESIDENT 1
-- MANAGER   3
-- ANALYST   2

-- Min and max salary per department
SELECT deptno, MIN(sal), MAX(sal) FROM emp GROUP BY deptno;
-- DEPTNO   MIN(SAL)   MAX(SAL)
-- 10       4100       7000
-- 20       2200       4500
-- 30       200        2950
```

### Rule
Other than group function columns, all columns after SELECT must also be specified after GROUP BY. Otherwise Oracle returns the error **"not a GROUP BY expression"**.

```sql
SELECT deptno, SUM(sal), job FROM emp GROUP BY deptno;
-- ERROR: not a GROUP BY expression

-- Correct:
SELECT deptno, SUM(sal), job FROM emp GROUP BY deptno, job;
```

Also, we cannot mix group functions with normal (ungrouped) columns without GROUP BY:
```sql
SELECT deptno, SUM(sal) FROM emp;
-- ERROR: not a single-group group function

-- Correct:
SELECT deptno, SUM(sal) FROM emp GROUP BY deptno;
-- DEPTNO   SUM(SAL)
-- 10       8450
-- 20       15975
-- 30       15450
```

---

## Q. Explain HAVING clause with examples.

After GROUP BY clause, we are not allowed to use WHERE clause. In place of this, ANSI/ISO SQL provided another clause called **HAVING**, which is used to restrict groups after GROUP BY.

- Generally, if we want to restrict rows in a table, we use **WHERE** clause.
- If we want to restrict groups after GROUP BY, we use **HAVING** clause.
- We are **not allowed to use group functions in WHERE**. In HAVING clause, we **can** use group functions.

Syntax:
```sql
SELECT columnname FROM tablename
GROUP BY columnname
HAVING condition;
```

Examples:

```sql
-- Departments having more than 3 employees
SELECT deptno, COUNT(*) FROM emp GROUP BY deptno HAVING COUNT(*) > 3;
-- DEPTNO   COUNT(*)
-- 30       6
-- 20       5

-- Departments whose SUM(sal) is more than 9000
SELECT deptno, SUM(sal) FROM emp GROUP BY deptno HAVING SUM(sal) > 9000;
-- DEPTNO   SUM(SAL)
-- 20       15975
-- 10       15450

-- Years in which more than 1 employee was hired
SELECT TO_CHAR(hiredate, 'YYYY') "YEAR", COUNT(*) FROM emp
GROUP BY TO_CHAR(hiredate, 'YYYY') HAVING COUNT(*) > 1;
-- YEAR   COUNT(*)
-- 1987   2
-- 1981   10
```

---

## Q. Explain ORDER BY clause.

ORDER BY clause is used to arrange data in either ascending or descending order.

Two keywords are used with ORDER BY: `ASC` and `DESC`. By default, ORDER BY is ascending.

Syntax:
```sql
SELECT * FROM tablename ORDER BY columnname [ASC | DESC];
```

Examples:
```sql
SELECT sal FROM emp ORDER BY sal DESC;
SELECT * FROM emp ORDER BY ename ASC;

-- Multiple columns: sort by first column, then by second within each group
SELECT deptno, sal FROM emp ORDER BY deptno, sal DESC;
-- DEPTNO  SAL
-- 10      8350
-- 10      7000
-- 10      6000
-- 20      3300
-- 20      3000
-- 20      1800
```

### Complete SELECT statement clause order
```sql
SELECT col1, col2, ...
FROM tablename
WHERE condition
GROUP BY columnname
HAVING condition
ORDER BY columnname [ASC | DESC];
```

Example combining all clauses:
```sql
SELECT deptno, COUNT(*) FROM emp
WHERE sal > 1000
GROUP BY deptno
HAVING COUNT(*) > 3
ORDER BY deptno DESC;
```

---

## Q. Explain ROLLUP and CUBE.

Oracle 8i introduced ROLLUP and CUBE clauses. These clauses are used only along with GROUP BY.

These clauses are used to calculate **subtotals and grand totals automatically**.

- If we want to calculate subtotal based on a single column, we use **ROLLUP**.
- If we want to calculate subtotal based on multiple columns (all combinations), we use **CUBE**.

Syntax:
```sql
SELECT col1, col2, ... FROM tablename GROUP BY ROLLUP(col1, col2, ...);
SELECT col1, col2, ... FROM tablename GROUP BY CUBE(col1, col2, ...);
```

Examples:
```sql
SELECT deptno, job, SUM(sal) FROM emp GROUP BY ROLLUP(deptno, job);
SELECT deptno, job, SUM(sal), COUNT(*) FROM emp GROUP BY CUBE(deptno, job);
SELECT ename, SUM(sal) FROM emp GROUP BY ROLLUP(ename);
```

Example with join and rollup:
```sql
SELECT dname, SUM(sal) FROM emp e, dept d
WHERE e.deptno = d.deptno
GROUP BY ROLLUP(dname);
-- DNAME        SUM(SAL)
-- ACCOUNTING   10000
-- RESEARCH     22000
-- SALES        32000
--              64000     <- Grand total added automatically by ROLLUP
```

---

## Q. Explain sub-queries (nested queries).

A **sub-query** is a query written inside another query. The inner query executes first, and its result is used by the outer query.

### Single-row sub-query
Returns exactly one value. Used with `=`, `>`, `<`, etc.
```sql
SELECT * FROM emp WHERE sal > (SELECT sal FROM emp WHERE ename = 'JONES');
```

### Multi-row sub-query
Returns many values. Used with `IN`, `ANY`, `ALL`.
```sql
SELECT * FROM emp WHERE deptno IN (SELECT deptno FROM dept WHERE loc = 'CHICAGO');

SELECT * FROM emp WHERE sal > ANY (SELECT sal FROM emp WHERE deptno = 30);
-- greater than minimum of the list

SELECT * FROM emp WHERE sal > ALL (SELECT sal FROM emp WHERE deptno = 30);
-- greater than maximum of the list
```

### Correlated sub-query
Inner query references the outer query's table. Executes once per outer row.
```sql
SELECT ename, sal, deptno FROM emp e
WHERE sal > (SELECT AVG(sal) FROM emp WHERE deptno = e.deptno);
```

### EXISTS / NOT EXISTS
Checks whether the sub-query returns any rows at all.
```sql
SELECT * FROM dept d WHERE EXISTS (SELECT 1 FROM emp e WHERE e.deptno = d.deptno);
```

---

## Q. Explain joins and the different types.

**Joins** are used to retrieve data from multiple tables. In all databases, if we are joining `n` tables, we need `n - 1` joining conditions.

Oracle server supports the following types of joins:

### Oracle-style joins (8i joins)
1. Equi Join (or Inner Join)
2. Non-Equi Join
3. Self Join
4. Outer Join

### ANSI joins (9i joins)
1. Inner Join
2. Left Outer Join
3. Right Outer Join
4. Full Outer Join
5. Natural Join
6. Cross Join

**Cross Join:** in Oracle, we can also retrieve data from multiple tables without specifying a join condition. In this case, from clauses of the SELECT statement use cross join internally. Cross join is implemented based on Cartesian product. That is why this join returns more rows.

Example:
```sql
SELECT ename, sal, dname, loc FROM emp, dept;
-- If emp has 14 rows and dept has 4 rows, output = 14 * 4 = 56 rows
```

---

## Q. Explain Equi Join / Inner Join with examples.

Based on **equality condition**, we retrieve data from multiple tables. Here the joining conditional columns must belong to the **same datatype**.

Whenever tables have a common column, we are allowed to use Equi Join. These common columns must belong to the same datatype.

Syntax:
```sql
SELECT col1, col2, ...
FROM tablename1, tablename2, ...
WHERE tablename1.commoncol = tablename2.commoncol;
```

Example:
```sql
SELECT ename, sal, deptno, dname, loc
FROM emp, dept
WHERE emp.deptno = dept.deptno;
-- ERROR: column ambiguously defined
```

For avoiding ambiguity in Oracle, we must specify the table name along with the common column name using the dot (`.`) operator:
```sql
SELECT ename, sal, dept.deptno, dname, loc
FROM emp, dept
WHERE emp.deptno = dept.deptno;
```

**Note:** Always Equi Join returns **matching rows only**. If DEPT has a department 40 that no employee belongs to, it will not appear in the output.

**Using alias name for tables:**

In joins we can also create alias names for tables within the from clause. These alias names are also called reference names.

Syntax:
```sql
FROM tablename1 aliasname1, tablename2 aliasname2;
```

Example:
```sql
SELECT ename, sal, d.deptno, dname, loc
FROM emp e, dept d
WHERE e.deptno = d.deptno;
```

### Example queries with Equi Join

```sql
-- Q. Employees who are working in location "CHICAGO"
SELECT ename, loc FROM emp, dept
WHERE emp.deptno = dept.deptno
  AND loc = 'CHICAGO';
-- ENAME    LOC
-- WARD     CHICAGO
-- TURNER   CHICAGO
-- MILLER   CHICAGO
-- BLAKE    CHICAGO
-- MARTIN   CHICAGO
```

**Note:** If you want to filter data after joining condition, we use AND operator in 8i join. In 9i join, we can use either AND or WHERE clause.

```sql
-- Q. Display dname, SUM(sal) from emp, dept using equi join
SELECT dname, SUM(sal) FROM emp e, dept d
WHERE e.deptno = d.deptno
GROUP BY dname;
-- DNAME        SUM(SAL)
-- ACCOUNTING   15450
-- RESEARCH     20150
-- SALES        22100

-- Q. Display loc, no. of employees, min sal, max sal
SELECT loc, COUNT(*), MIN(sal), MAX(sal) FROM emp e, dept d
WHERE e.deptno = d.deptno
GROUP BY loc;
-- LOC       COUNT(*)  MIN(SAL)  MAX(SAL)
-- NEW YORK  3         4100      7000
-- CHICAGO   6         200       3000
-- DALLAS    5         2200      4500

-- Q. Same, but only where SUM(sal) > 10000
SELECT loc, COUNT(*), MIN(sal), MAX(sal) FROM emp e, dept d
WHERE e.deptno = d.deptno
GROUP BY loc
HAVING SUM(sal) > 10000;
```

---

## Q. Explain Non-Equi Join with example.

Based on other-than-equality conditions (`<`, `<=`, `>`, `>=`, `<>`, `BETWEEN`, ...), we retrieve data from multiple tables.

In Oracle, using Non-Equi Join we can also retrieve data from multiple tables when the tables do not have a common column, but one column value lies between two column values of another table.

Example (using SALGRADE):
```sql
SELECT * FROM emp;
SELECT * FROM salgrade;   -- grade, losal, hisal

SELECT ename, sal, losal, hisal FROM emp, salgrade
WHERE sal BETWEEN losal AND hisal;

-- OR:
SELECT ename, sal, losal, hisal FROM emp, salgrade
WHERE sal >= losal AND sal <= hisal;
```

---

## Q. Explain Self Join with example.

Joining a table itself is called **Self Join**.

- Here joining conditional columns must belong to the same datatype.
- Generally, if we want to compare two column values from different tables and retrieve matching rows, we use **Equi Join**. If we want to compare two different column values within the **same table**, we must use **Self Join**. But here these columns must belong to the same datatype.

Before we use Self Join, we must create table alias names in the FROM clause. These alias names must be different names and are also known as reference names. These alias names internally behave like exact tables at query execution time.

Syntax:
```sql
FROM tablename aliasname1, tablename aliasname2;
```

Example queries:

```sql
-- Q. Display employee name and manager name from emp table using self join
SELECT e1.ename "employee", e2.ename "manager"
FROM emp e1, emp e2
WHERE e1.mgr = e2.empno;

-- Q. Employees earning more salary than their manager
SELECT e1.ename "employee", e2.ename "manager", e1.sal, e2.sal "mgr_sal"
FROM emp e1, emp e2
WHERE e1.mgr = e2.empno
  AND e1.sal > e2.sal;
-- EMPLOYEE  MGR    SAL   MGR_SAL
-- FORD      JONES  3600  3475
-- SCOTT     JONES  4600  3475

-- Q. Employees who joined BEFORE their manager
SELECT e1.ename "employee", e1.hiredate, e2.ename "manager", e2.hiredate "mgr_hiredate"
FROM emp e1, emp e2
WHERE e1.mgr = e2.empno
  AND e1.hiredate < e2.hiredate;
```

---

## Q. Explain Outer Join with example.

This join is used to retrieve **matching rows plus all rows from one table** (or non-matching rows).

Generally, using Equi Join we retrieve only matching rows. If we want to retrieve non-matching rows also, we use the **JOIN operator `(+)`** within the joining condition of Equi Join. This is called **Oracle 8i Outer Join**.

**Note:** The `(+)` operator can be used only on **one side at a time** within the joining condition.

Example:
```sql
SELECT ename, sal, d.deptno, dname, loc
FROM emp e, dept d
WHERE e.deptno(+) = d.deptno;
--       ^                  ^
--       matching rows      all rows

-- Output includes:  40  OPERATIONS  BOSTON  (with NULL ename, NULL sal)
-- because dept 40 has no employees.
```

### Full Outer Join (before Oracle 9i)

In Oracle, if we want to retrieve matching and non-matching rows from all tables, we use **Full Outer Join**. But Full Outer Join is a 9i join. Prior to Oracle 9i, if we want to retrieve all data, we use the JOIN operator within the joining condition on one side first, and then on the other side, combined with UNION.

```sql
SELECT ename, sal, d.deptno, dname, loc
FROM emp e, dept d
WHERE e.deptno(+) = d.deptno
UNION
SELECT ename, sal, d.deptno, dname, loc
FROM emp e, dept d
WHERE e.deptno = d.deptno(+);
```

---

## Q. Explain 9i / ANSI joins with examples.

Oracle 9i introduced ANSI-style joins:
1. Inner Join
2. Left Outer Join
3. Right Outer Join
4. Full Outer Join
5. Natural Join
6. Cross Join

### 1. Inner Join
Returns matching rows only (same as Equi Join), but performance is very high compared to Oracle 8i Equi Join.

When tables have a common column, we are allowed to use Inner Join. Common columns must belong to the same datatype.

Syntax:
```sql
SELECT col1, col2 FROM tablename1
JOIN tablename2
ON tablename1.commoncol = tablename2.commoncol;
```

Example:
```sql
SELECT ename, sal, d.deptno, dname, loc
FROM emp e JOIN dept d
ON e.deptno = d.deptno;

-- Q. Employees working in CHICAGO using inner join
SELECT ename, loc FROM emp JOIN dept
ON emp.deptno = dept.deptno
WHERE loc = 'CHICAGO';
-- OR
SELECT ename, loc FROM emp JOIN dept
ON emp.deptno = dept.deptno
AND loc = 'CHICAGO';
```

### USING clause

In 9i joins we can also use the **USING clause** in place of ON clause. Generally USING clause performance is very high compared to ON clause, and USING clause returns common columns only one time.

Syntax:
```sql
SELECT * FROM tablename1
JOIN tablename2
USING (commoncolumnname1, commoncolumnname2, ...);
```

Example:
```sql
SELECT ename, sal, deptno, dname, loc
FROM emp JOIN dept
USING (deptno);
```

**Note:** Whenever we use USING clause, we are **not allowed to use alias names** in joining conditional columns.
```sql
SELECT e.ename, sal, d.deptno, dname, loc
FROM emp e JOIN dept d
USING (deptno);
-- ERROR

-- Correct:
SELECT ename, sal, deptno, dname, loc
FROM emp JOIN dept
USING (deptno);
```

### 2. Left Outer Join

This join always returns all rows from left side table, and matching rows from the right side table. It also returns NULL values in place of non-matching rows in the other table.

```sql
SELECT * FROM z1 LEFT OUTER JOIN z2
ON z1.a = z2.a AND z1.b = z2.b;
```

### 3. Right Outer Join

Returns all rows from right side table, and matching rows from left side table. Returns NULL values in place of non-matching rows in the other table.

```sql
SELECT * FROM z1 RIGHT OUTER JOIN z2
ON z1.b = z2.b;
```

### 4. Full Outer Join

Returns all rows from all tables because it is a combination of Left and Right Outer Join. Returns NULL values in place of non-matching rows.

```sql
SELECT * FROM z1 FULL OUTER JOIN z2
ON z1.a = z2.a AND z1.b = z2.b;
```

### 5. Natural Join

This join also returns matching rows only. Natural Join performance is very high compared to Inner Join. In this join, we are **not required to use joining conditions explicitly**. But in this case, the source tables must have a common column name. Based on this common column, Oracle server internally, automatically establishes a joining condition.

Syntax:
```sql
SELECT * FROM tablename1 NATURAL JOIN tablename2;
```

**Note:** Whenever we use Natural Join, Oracle server always returns common columns only one time (because internally it uses the USING clause). Also, whenever we use Natural Join, we are **not allowed to use alias names** for joining conditional columns.

Example:
```sql
SELECT ename, sal, deptno, dname, loc
FROM emp NATURAL JOIN dept;
```

### 6. Cross Join

Returns Cartesian product of two tables.
```sql
SELECT ename, sal, dname, loc FROM emp CROSS JOIN dept;
-- If emp has 14 rows and dept has 4 rows, output = 56 rows
```

### Joining more than two tables

**8i Join syntax:**
```sql
SELECT col1, col2, ... FROM table1, table2, table3
WHERE table1.commoncol = table2.commoncol
  AND table2.commoncol = table3.commoncol;
```

**9i / ANSI Join syntax:**
```sql
SELECT col1, col2, ... FROM table1
JOIN table2 ON table1.commoncol = table2.commoncol
JOIN table3 ON table2.commoncol = table3.commoncol;
```

---

## Q. Explain views. What are simple views and complex views?

**View** is a database object which is used to provide authority-level security.

- Generally, from a data security point of view, the Database Administrator creates views from a table, and then those views are given to the number of users.
- Views do not store data. That is why a view is also called a **virtual table** or **window of a table**.
- Views are created from a base table. Based on the base table, views are categorized into two types:
  (a) **Simple View**
  (b) **Complex View** (or Join View)

### (a) Simple View
A view which is created from **only one base table**.

Syntax:
```sql
CREATE OR REPLACE VIEW viewname
AS
SELECT statement;
```

Example:
```sql
CREATE OR REPLACE VIEW v1
AS
SELECT * FROM emp WHERE deptno = 10;

SELECT * FROM v1;
```

### DML operations on Simple View

In Oracle we can also perform DML operations through a simple view to the base table, based on the following restrictions:

1. If a simple view has a group function, GROUP BY clause, ROWNUM, DISTINCT, set operators, or joins, then **we cannot perform DML operations** through the simple view to the base table.

2. We must include base table NOT NULL columns into the view. Then only we are allowed to perform insertion operation through the simple view to the base table.

Examples:

**Example 1: works fine**
```sql
CREATE OR REPLACE VIEW v1
AS
SELECT * FROM emp WHERE deptno = 10;

INSERT INTO v1 (empno, ename, deptno) VALUES (1, 'murali', 30);
-- 1 row created
```

**Example 2: fails**
```sql
CREATE OR REPLACE VIEW v2
AS
SELECT ename, sal, deptno FROM emp WHERE deptno = 10;

INSERT INTO v2 (ename, sal, deptno) VALUES ('abc', 2000, 30);
-- ERROR: cannot insert NULL into empno (empno is NOT NULL in base table but missing from view)
```

### View Definition Storage

In all databases, whenever we create a view, its definition (the SELECT statement) is **automatically permanently stored** in the database.

In Oracle, to view a view's stored definition, we use the `USER_VIEWS` data dictionary:
```sql
DESC USER_VIEWS;
SELECT text FROM USER_VIEWS WHERE view_name = 'V1';
-- Text
-- ---------
-- SELECT "SNO", "NAME" FROM BASE
```

**Note:** Views are also used for **simplifying queries**. When a view contains functions or expressions, we must create alias names for those functions/expressions, otherwise Oracle returns an error.

```sql
CREATE OR REPLACE VIEW v1
AS
SELECT deptno, MAX(sal) FROM emp GROUP BY deptno;
-- ERROR: must name this expression with a column alias

-- Solution:
CREATE OR REPLACE VIEW v1
AS
SELECT deptno, MAX(sal) a FROM emp GROUP BY deptno;
```

Same rule applies when a view uses ROWNUM:
```sql
CREATE OR REPLACE VIEW v1
AS
SELECT ROWNUM, ename FROM emp;
-- ERROR

-- Correct:
CREATE OR REPLACE VIEW v1
AS
SELECT ROWNUM sno, ename FROM emp;
```

### (b) Complex View

A view which is created from **multiple base tables**.

Example:
```sql
CREATE OR REPLACE VIEW v5
AS
SELECT ename, sal, dname, loc
FROM emp, dept
WHERE emp.deptno = dept.deptno;

SELECT * FROM v5;
```

**Generally, we cannot perform DML operations through a complex view to the base table.**

```sql
UPDATE v5 SET ename = 'abc' WHERE ename = 'SMITH';
-- 1 row updated

UPDATE v5 SET dname = 'xyz' WHERE dname = 'SALES';
-- ERROR: cannot modify a column which maps to a non key-preserved table
```

In Oracle, when we try to perform DML operation through a complex view to the base table, some columns are affected and some are not. To see which columns are updatable:
```sql
DESC USER_UPDATABLE_COLUMNS;

SELECT column_name, updatable FROM USER_UPDATABLE_COLUMNS
WHERE table_name = 'V5';
-- COLUMN_NAME  UPDATABLE
-- ENAME        YES
-- SAL          YES
-- DNAME        NO
-- LOC          NO
```

Generally, we cannot perform DML operations through a complex view to the base table. To overcome this problem, Oracle 8.0 introduced **INSTEAD OF triggers** in PL/SQL (details in the triggers section).

---

## Q. Explain triggers in detail.

A **trigger** is a stored PL/SQL block, similar to a stored procedure, and it will be **automatically invoked** whenever a DML operation is performed on a table.

All DB systems have two types of triggers:
(a) **Statement-level triggers**
(b) **Row-level triggers**

- In statement-level triggers, the trigger body is executed **only once per DML statement**.
- In row-level triggers, the trigger body is executed **for each row of a DML statement**. That is why in row-level triggers we use `FOR EACH ROW` clause.

### Trigger syntax

```sql
CREATE OR REPLACE TRIGGER triggername
BEFORE | AFTER  INSERT | DELETE | UPDATE  ON tablename
[FOR EACH ROW]    -- only for row-level triggers
BEGIN
    -- trigger body
END;
/
```

### Statement-level trigger example

```sql
CREATE TABLE test (col1 DATE);

CREATE OR REPLACE TRIGGER th1
AFTER UPDATE ON emp
BEGIN
    INSERT INTO test VALUES (SYSDATE);
END;
/

-- Testing:
UPDATE emp SET sal = sal + 100 WHERE deptno = 10;
-- 3 rows updated

SELECT * FROM test;
-- COL1
-- 21-DEC-15    <- only ONE entry, even though 3 rows were updated

DELETE FROM test;
DROP TRIGGER th1;
```

### Row-level trigger example

```sql
CREATE OR REPLACE TRIGGER th2
AFTER UPDATE ON emp
FOR EACH ROW
BEGIN
    INSERT INTO test VALUES (SYSDATE);
END;
/

-- Testing:
UPDATE emp SET sal = sal + 100 WHERE deptno = 10;
-- 3 rows updated

SELECT * FROM test;
-- COL1
-- 21-DEC-15
-- 21-DEC-15
-- 21-DEC-15    <- 3 entries because 3 rows were updated
```

### :OLD and :NEW qualifiers

In row-level triggers, the trigger body is executed for each row of a DML statement. That is why we use `FOR EACH ROW` clause in trigger specification. Also, DML transactional values are internally stored using two **rollback segment qualifiers**:

1. `:OLD` - contains column values before the change
2. `:NEW` - contains column values after the change

These are used either in trigger specification or in the trigger body. When we use these qualifiers in trigger body, we must use a **colon (:)** in front of the qualifier name.

Syntax:
- `:old.columnname`
- `:new.columnname`

Availability:

| Event | :OLD | :NEW |
|---|---|---|
| INSERT | Not available | Available |
| UPDATE | Available | Available |
| DELETE | Available | Not available |

### Example: row-level trigger backing up deleted rows

**Q.** Write a PL/SQL row-level trigger on emp table such that whenever a user deletes data from emp table, those deleted records are automatically stored in another table.

```sql
-- Step 1: Create backup table with same structure but no data
CREATE TABLE backup AS SELECT * FROM emp WHERE 1 = 2;

SELECT * FROM backup;   -- no rows selected
DESC backup;

-- Step 2: Create row-level trigger
CREATE OR REPLACE TRIGGER tk1
AFTER DELETE ON emp
FOR EACH ROW
BEGIN
    INSERT INTO backup VALUES
        (:old.empno, :old.ename, :old.job, :old.mgr,
         :old.hiredate, :old.sal, :old.comm, :old.deptno);
END;
/

-- Testing:
DELETE FROM emp WHERE sal > 2000;
-- 10 rows deleted

SELECT * FROM backup;
-- shows the 10 deleted rows
```

### INSTEAD OF trigger

Generally, we cannot perform DML operations through a complex view to the base table. To overcome this problem, Oracle 8.0 introduced **INSTEAD OF triggers** in PL/SQL. By default, INSTEAD OF triggers are **row-level triggers**, and they are always created on **views**, not on tables.

Whenever we create an INSTEAD OF trigger on a complex view, then only we are allowed to perform DML operations through the complex view to the base table.

Syntax:
```sql
CREATE OR REPLACE TRIGGER triggername
INSTEAD OF  INSERT | UPDATE | DELETE  ON viewname
FOR EACH ROW
BEGIN
    -- trigger body
END;
/
```

Example:
```sql
CREATE OR REPLACE TRIGGER tv1
INSTEAD OF UPDATE ON v5
FOR EACH ROW
BEGIN
    UPDATE dept SET dname = :new.dname WHERE dname = :old.dname;
    UPDATE dept SET loc = :new.loc WHERE loc = :old.loc;
END;
/

-- Testing:
UPDATE v5 SET dname = 'xyz' WHERE dname = 'SALES';
-- 6 rows updated (works now!)
```

---

## Q. Explain materialized views. Difference between views and materialized views.

Oracle 8i introduced **Materialized View** (M.View).

- M.View is used in Data Warehousing applications.
- M.Views are handled by Database Administration.
- Generally, views do not store data, whereas **materialized views store data**.
- Materialized views are used to **improve performance** of joined or aggregate queries.
- Materialized view stores the result of the query. That is, M.View stores replication of remote database into local node.
- M.View also stores data like a table, but when we refresh the M.View, it synchronizes the data based on the base table.

### Difference between View and Materialized View

| Views | Materialized Views |
|---|---|
| View does not store data | Materialized view stores data |
| Security purpose | Improve performance purpose |
| When we drop the base table, view cannot be accessed | When we drop the base table, materialized view can be accessed |
| Through the view we can perform DML operation | We cannot perform DML operation |

### Syntax

```sql
CREATE MATERIALIZED VIEW viewname
AS
SELECT statement;
```

In Oracle, before creating a materialized view, the Database Administrator must give `CREATE ANY MATERIALIZED VIEW` privilege to the user.

```sql
GRANT CREATE ANY MATERIALIZED VIEW TO username;
```

Example:
```sql
CONN system/manager;
CREATE MATERIALIZED VIEW mz1 AS SELECT * FROM emp;
-- ERROR: insufficient privileges

CONN sys AS sysdba;
Enter password: sys
GRANT CREATE ANY MATERIALIZED VIEW TO system;

CONN system/manager;
CREATE MATERIALIZED VIEW mz1 AS SELECT * FROM emp;
-- materialized view created
```

**Note:** In Oracle, sometimes we get an "insufficient privileges" error for views too. To fix:
```sql
GRANT CREATE ANY VIEW TO username;
```

**Rule:** One of the materialized base tables must have a **primary key**, otherwise Oracle returns an error. Note: in Oracle 11g and 12c, we can create a materialized view even when the base table does not have a primary key.

### Types of Materialized Views

Oracle has two types of materialized views:
1. Complete Refresh Materialized View
2. Fast Refresh Materialized View

### 1. Complete Refresh Materialized View

In Oracle, by default, materialized views are complete refresh. In these materialized views, internally rowids are recreated when we are refreshing the materialized view, even if we are not modifying data in the base table. That is why these materialized views do not give better performance when we refresh many times.

Syntax:
```sql
CREATE MATERIALIZED VIEW viewname
REFRESH COMPLETE
AS
SELECT statement;
```

Example:
```sql
SELECT ROWID, sno, name FROM mj1;
EXEC DBMS_MVIEW.REFRESH('mj1');
SELECT ROWID, sno, name FROM mj1;
-- Here ROWIDs are changed after refresh.
```

### 2. Fast Refresh Materialized View

Fast Refresh materialized views are also called **incremental refresh materialized views**. This M.View performance is very high compared to Complete Refresh M.View, because in this M.View, rowids are not changed when we refresh, no matter how many times we refresh.

Syntax:
```sql
CREATE MATERIALIZED VIEW viewname
REFRESH FAST
AS
SELECT statement;
```

Before we create a Fast Refresh M.View, it needs a mechanism to capture any changes made to its base table. This requirement is also called **M.View log**. That is why before creating a fast refresh materialized view, we must create a materialized view log on the base table using:

```sql
CREATE MATERIALIZED VIEW LOG ON basetablename;
```

Example:
```sql
CREATE MATERIALIZED VIEW LOG ON base;

CREATE MATERIALIZED VIEW mj2
REFRESH FAST
AS
SELECT * FROM base;

SELECT ROWID, sno, name FROM mj2;

UPDATE base SET name = 'pqr' WHERE sno = 2;
EXEC DBMS_MVIEW.REFRESH('mj2');

SELECT ROWID, sno, name FROM mj2;
-- Here ROWIDs are NEVER changed.
```

### Refresh methods: ON DEMAND vs ON COMMIT

Generally, we are refreshing M.View in two ways:
1. Manually
2. Automatically

**1. Manually (on demand):**
In manual method, we are refreshing M.View by using `DBMS_MVIEW` package. This method is also called **ON DEMAND** method. In Oracle, by default, refresh method is ON DEMAND.

**2. Automatically (on commit):**
In Oracle, we can also refresh M.View without using DBMS_MVIEW package. This method is called **ON COMMIT** method.

Syntax:
```sql
CREATE MATERIALIZED VIEW viewname
REFRESH [COMPLETE | FAST]  [ON DEMAND | ON COMMIT]
AS
SELECT statement;
```

Example:
```sql
CREATE MATERIALIZED VIEW mj3
REFRESH FAST ON COMMIT
AS
SELECT * FROM base;

UPDATE base SET name = 'zzz' WHERE sno = 3;
SELECT * FROM base;

SELECT * FROM mj3;   -- not yet updated

COMMIT;

SELECT * FROM mj3;   -- now automatically updated
```

**Summary:** Generally, M.Views are used to improve performance of joined or aggregate queries. Views are used to provide security by restricting table columns. In all databases, if you want to restrict table columns from one user to another user, we create a view with the required columns, and then those views are given to the number of users by using GRANT command.

---

## Q. Explain stored procedures and functions in PL/SQL.

A **stored procedure** is a named PL/SQL block, stored in the database, that performs a task and can be invoked repeatedly.

Syntax:
```sql
CREATE OR REPLACE PROCEDURE proc_name (p1 IN datatype, p2 OUT datatype)
IS
BEGIN
    -- statements
END;
/

-- Calling it:
EXEC proc_name(value1, value2);
```

A **stored function** is similar but must return a value using `RETURN`.

Syntax:
```sql
CREATE OR REPLACE FUNCTION func_name (p1 IN datatype) RETURN datatype
IS
BEGIN
    RETURN some_value;
END;
/
```

---

## Q. Explain QBE and QUEL.

### QBE (Query By Example)
QBE is a visual/graphical query language where the user fills sample values and conditions into a table-like grid (a "skeleton table") instead of writing text-based SQL statements. It is used in tools like MS Access.

### QUEL (QUEry Language)
QUEL is a query language developed for the **INGRES** database system, as an alternative to SQL. It is based on tuple relational calculus. QUEL syntax is similar to SQL but predates the SQL standard. It uses keywords like `RANGE OF` and `RETRIEVE` instead of SELECT and FROM.

Example (QUEL style):
```
RANGE OF e IS emp
RETRIEVE (e.ename, e.sal)
WHERE e.deptno = 10
```

---

## Q. Explain DCL and TCL commands.

### DCL: GRANT and REVOKE

Used to control access to data.

**Creating a user:**
```sql
CONN sys AS sysdba;
Enter password: sys

CREATE USER username IDENTIFIED BY password;
GRANT CONNECT, RESOURCE TO username;    -- or GRANT DBA TO username;

CONN username/password;
```

**Granting object privileges:**
```sql
GRANT SELECT, INSERT ON tablename TO username;
GRANT CREATE ANY VIEW TO username;
GRANT CREATE ANY MATERIALIZED VIEW TO username;
```

**Revoking:**
```sql
REVOKE SELECT, INSERT ON tablename FROM username;
```

### TCL: COMMIT, ROLLBACK, SAVEPOINT

Used to manage transactions.

```sql
COMMIT;                -- permanently saves all changes since last commit
ROLLBACK;               -- undoes all changes since last commit
SAVEPOINT sp1;          -- marks a point to roll back to
ROLLBACK TO sp1;        -- rolls back only up to the savepoint
```

---

## Q. Explain programmatic SQL: Embedded SQL, Dynamic SQL, and ODBC.

### Embedded SQL
SQL statements written directly inside a host programming language's source code (like C, COBOL, or Java) and pre-compiled before the host language compiler runs. Embedded SQL is **static** - the SQL statement structure is fixed at compile time.

Example (in C):
```c
EXEC SQL SELECT ename INTO :name FROM emp WHERE empno = :id;
```

### Dynamic SQL
SQL statements that are **constructed and executed at runtime** as strings, allowing flexible queries whose exact structure is not known until the program runs. Oracle supports this via PL/SQL's `EXECUTE IMMEDIATE`.

Example:
```sql
DECLARE
    v_sql VARCHAR2(1000);
BEGIN
    v_sql := 'SELECT COUNT(*) FROM emp WHERE deptno = ' || :dept;
    EXECUTE IMMEDIATE v_sql INTO v_count;
END;
```

### ODBC (Open DataBase Connectivity)
A standard API (Application Programming Interface) that allows application programs to access data in different DBMSs using a common set of function calls, independent of the specific database or programming language. ODBC uses drivers to translate application calls into vendor-specific database calls.

---

# Query Practice Bank

Try to write each of these on your own first, then verify.

```sql
-- 1. Display ename, sal, annual salary
SELECT ename, sal, sal*12 annsal FROM emp;

-- 2. Employees except JOB = CLERK
SELECT * FROM emp WHERE job <> 'CLERK';

-- 3. Employees earning more than 2000
SELECT * FROM emp WHERE sal > 2000;

-- 4. CLERKs earning more than 2000
SELECT * FROM emp WHERE job = 'CLERK' AND sal > 2000;

-- 5. Employees in dept 20, 30, 50, 70, 80
SELECT * FROM emp WHERE deptno IN (20, 30, 50, 70, 80);

-- 6. Employees with no commission
SELECT * FROM emp WHERE comm IS NULL;

-- 7. Employees whose name starts with M
SELECT * FROM emp WHERE ename LIKE 'M%';

-- 8. Employees whose name has M in any position
SELECT * FROM emp WHERE ename LIKE '%M%';

-- 9. Employees whose second letter is L
SELECT * FROM emp WHERE ename LIKE '_L%';

-- 10. Employees whose name is exactly 5 chars
SELECT * FROM emp WHERE LENGTH(ename) = 5;

-- 11. Employees joining in December
SELECT * FROM emp WHERE TO_CHAR(hiredate, 'MON') = 'DEC';

-- 12. Employees joining in year 1981
SELECT * FROM emp WHERE TO_CHAR(hiredate, 'YYYY') = '1981';

-- 13. Number of employees in each department
SELECT deptno, COUNT(*) FROM emp GROUP BY deptno;

-- 14. Number of employees per job
SELECT job, COUNT(*) FROM emp GROUP BY job;

-- 15. Departments having more than 3 employees
SELECT deptno, COUNT(*) FROM emp GROUP BY deptno HAVING COUNT(*) > 3;

-- 16. Departments where total salary > 9000
SELECT deptno, SUM(sal) FROM emp GROUP BY deptno HAVING SUM(sal) > 9000;

-- 17. Year-wise count of employees hired, only years with more than 1 hire
SELECT TO_CHAR(hiredate, 'YYYY') "YEAR", COUNT(*)
FROM emp
GROUP BY TO_CHAR(hiredate, 'YYYY')
HAVING COUNT(*) > 1;

-- 18. Employees working in Chicago (equi join)
SELECT ename, loc FROM emp, dept
WHERE emp.deptno = dept.deptno AND loc = 'CHICAGO';

-- 19. Dept name and total salary
SELECT dname, SUM(sal) FROM emp e, dept d
WHERE e.deptno = d.deptno GROUP BY dname;

-- 20. Location-wise employee count, min sal, max sal
SELECT loc, COUNT(*), MIN(sal), MAX(sal)
FROM emp e, dept d
WHERE e.deptno = d.deptno GROUP BY loc;

-- 21. Employee name and manager name (self join)
SELECT e1.ename "employee", e2.ename "manager"
FROM emp e1, emp e2
WHERE e1.mgr = e2.empno;

-- 22. Employees earning more than their manager
SELECT e1.ename, e2.ename "manager"
FROM emp e1, emp e2
WHERE e1.mgr = e2.empno AND e1.sal > e2.sal;

-- 23. All departments including those with no employees (outer join)
SELECT ename, sal, d.deptno, dname, loc
FROM emp e, dept d
WHERE e.deptno(+) = d.deptno;

-- 24. Same as above using ANSI syntax
SELECT ename, sal, d.deptno, dname, loc
FROM emp e RIGHT OUTER JOIN dept d ON e.deptno = d.deptno;

-- 25. Update comm using NVL2
UPDATE emp SET comm = NVL2(comm, comm + 500, 500);

-- 26. Employees with salary between 2000 and 5000
SELECT * FROM emp WHERE sal BETWEEN 2000 AND 5000;

-- 27. Rollup: dept and job wise total salary with subtotals/grand total
SELECT deptno, job, SUM(sal) FROM emp GROUP BY ROLLUP(deptno, job);

-- 28. Count E's in 'SLEEP'
SELECT LENGTH('SLEEP') - LENGTH(REPLACE('SLEEP', 'E')) FROM dual;

-- 29. First date of current month
SELECT LAST_DAY(ADD_MONTHS(SYSDATE, -1)) + 1 FROM dual;

-- 30. Create a view of dept 10 employees, insert through it
CREATE OR REPLACE VIEW v1 AS SELECT * FROM emp WHERE deptno = 10;
INSERT INTO v1 (empno, ename, deptno) VALUES (1, 'murali', 30);

-- 31. Materialized view with fast refresh on commit
CREATE MATERIALIZED VIEW LOG ON base;
CREATE MATERIALIZED VIEW mj3 REFRESH FAST ON COMMIT
AS SELECT * FROM base;

-- 32. Row-level trigger to backup deleted rows
CREATE TABLE backup AS SELECT * FROM emp WHERE 1 = 2;

CREATE OR REPLACE TRIGGER tk1
AFTER DELETE ON emp
FOR EACH ROW
BEGIN
    INSERT INTO backup VALUES
        (:old.empno, :old.ename, :old.job, :old.mgr,
         :old.hiredate, :old.sal, :old.comm, :old.deptno);
END;
/

-- 33. INSTEAD OF trigger for updating complex view
CREATE OR REPLACE TRIGGER tv1
INSTEAD OF UPDATE ON v5
FOR EACH ROW
BEGIN
    UPDATE dept SET dname = :new.dname WHERE dname = :old.dname;
    UPDATE dept SET loc = :new.loc WHERE loc = :old.loc;
END;
/
```
