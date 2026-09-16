# Database Management Systems

### A Book of Notes for Units 1 and 2

---

## Table of Contents

**Part I: Foundations of DBMS (Unit 1)**

1. Data, Information, and the Need for Databases
2. Flat Files and Their Drawbacks
3. Understanding DBMS
4. Database Structures (Logical and Physical)
5. The Three-Level Database Architecture
6. Data Independence
7. History of Data Models
8. Data Modeling: Entities, Attributes, Relationships, Keys
9. ER and EER Diagrams
10. The Relational Model
11. Codd's Rules
12. Relational Integrity

**Part II: Relational Query Languages (Unit 2)**

13. Relational Algebra
14. Relational Calculus
15. Introduction to SQL
16. Sub-languages of SQL
17. SQL Data Types
18. DDL Commands
19. DML Commands
20. The SELECT Statement
21. Operators in SQL
22. NULL Handling: NVL and NVL2
23. Number and Character Functions
24. Date Functions
25. Date Conversion Functions
26. Group (Aggregate) Functions
27. GROUP BY, HAVING, ORDER BY
28. ROLLUP and CUBE
29. Sub-queries
30. Joins (Oracle Style and ANSI Style)
31. Views
32. Materialized Views
33. Triggers
34. Stored Procedures and Functions
35. QBE and QUEL
36. DCL and TCL
37. Programmatic SQL

Reference tables used in every example:
- **EMP** (empno, ename, job, mgr, hiredate, sal, comm, deptno)
- **DEPT** (deptno, dname, loc)
- **SALGRADE** (grade, losal, hisal)

---

# PART I: FOUNDATIONS OF DBMS

---

## Chapter 1: Data, Information, and the Need for Databases

### Understanding the difference

**Data** is a collection of raw facts. On its own, data does not carry meaning. Think of it as ingredients before you cook them.

Examples:
- The number 45
- The name "Rahul"
- The date 12-JUN-2025

**Information** is what you get when data is processed into something meaningful. It is the finished dish, not the ingredients.

Examples:
- A student's marksheet (built by processing raw marks)
- A customer's invoice (built by processing raw transaction data)
- A cricket scorecard (built by processing raw ball-by-ball data)

### What is a data store?

A **data store** is any place where we permanently keep data or information in a computer system. Historically we have used three kinds of data stores:

1. Papers and books (offline, hard to search)
2. Flat files (basic computer files)
3. Databases (organized, queryable, secure)

### Possible Exam Questions

- Define data and information with examples.
- What is a data store? Name the three kinds.
- Differentiate between data and information.

---

## Chapter 2: Flat Files and Their Drawbacks

### What is a flat file?

A **flat file** is the traditional way of permanently storing data on a computer. In a flat-file system, every application program keeps its own separate file. There is no sharing of files between applications.

For example, in one company you might have:
- The Invoicing application keeps its own file with customer name, customer number, and vehicle code
- The CRM application keeps its own file with customer name, customer number
- The GIS application keeps its own file with customer name, turnover

Notice that the same customer information ends up stored in three separate places.

**Diagram: Flat file data management**

```
    +-----------+     +-----------+     +-----------+
    | Invoicing |     |    CRM    |     |    GIS    |     Applications
    |  App.Prog |     |  App.Prog |     |  App.Prog |
    +-----------+     +-----------+     +-----------+
         |                 |                  |
         v                 v                  v
    +-----------+     +-----------+     +-----------+
    |  File 1   |     |  File 2   |     |  File 3   |     Files
    | cust_name |     | cust_name |     | cust_name |
    | cust_no   |     | cust_no   |     | turnover  |
    | valcode   |     |           |     |           |
    +-----------+     +-----------+     +-----------+
              \___________|__________/
                          |
                    Redundant Data
```

### The five drawbacks of flat files

**1. Data Retrieval.**
To read data out of a flat file, you must write an entire program in a high-level language like C or COBOL. There is no built-in query language. Compare that to a database, where you simply write `SELECT * FROM emp;` and instantly get results.

**2. Data Redundancy.**
The same customer name might be sitting in three different files. If the customer changes their name and you update file 1 but forget file 2 and file 3, the data in different files no longer agrees with each other. This mismatch is called **inconsistency**. Flat files have no way to keep data consistent automatically. Databases handle this using transactions with ACID properties, and reduce duplicate storage using a process called **Normalization**.

**3. Data Integrity.**
Integrity means keeping data valid and correct. A database enforces validity using **constraints** (primary key, foreign key, unique, not null, check). In a flat file there are no built-in rules, so every application that touches the file must enforce validity itself in code. This is error prone.

**4. Data Security.**
Flat files have no security mechanism. Whoever has access to the file can read and change everything in it. Databases have **role-based security**, where each user has their own permissions.

**5. Data Indexing.**
Flat files cannot be indexed, so finding a row means scanning the whole file. Databases support indexing so searches are fast.

Because of these five drawbacks, organizations moved on to a new type of software called DBMS.

### Possible Exam Questions

- What is a flat file? Explain how data is managed in a flat-file system with a diagram.
- Explain the drawbacks of a flat-file system.
- What is data redundancy? What problem does it cause?
- Why did organizations move from flat files to DBMS?

---

## Chapter 3: Understanding DBMS

### Definition

A **DBMS** (Database Management System) is a collection of programs (software) written to manage data. It is used to store, manage, and retrieve data efficiently. It also defines the structure of data, lets you manipulate the data, and controls who can access what.

### Examples of DBMS software

- Oracle
- Teradata
- SQL Server
- MySQL
- SQLite
- Informix
- Sybase

### How it works

When you install DBMS software, some space is automatically created on the hard disk. This space is called the **database**. A user interface is also created so you can log in and directly interact with the database. You can also interact with the database indirectly through application programs.

**Diagram: DBMS approach**

```
    +----------+   +----------+   +----------+
    | App.Prog |   | App.Prog |   | App.Prog |
    |     1    |   |     2    |   |     3    |
    +----------+   +----------+   +----------+
          \             |              /
           \            |             /
            +--------------------------+
            |   Database Management    |
            |         System           |
            +--------------------------+
                        |
            +--------------------------+
            |     Operating System     |
            |      (File System)       |
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
        |Interface|
        +---------+
```

### How DBMS solves flat-file problems

| Flat File Problem | DBMS Solution |
|---|---|
| No query language | SQL, a simple non-procedural query language |
| Data redundancy and inconsistency | Normalization reduces duplication, transactions maintain consistency |
| No integrity rules | Constraints (PK, FK, NOT NULL, UNIQUE, CHECK) enforced automatically |
| No security | Role-based security through GRANT and REVOKE |
| No indexing | Indexes support fast retrieval |

### ACID properties

Every transaction in a database automatically has four properties known as **ACID**. This is what keeps data consistent without programmer effort.

- **Atomicity**: a transaction either finishes fully or does not happen at all. There is no half-finished state. Example: transferring money from account A to account B. Either both the debit and the credit happen, or neither does.
- **Consistency**: a transaction moves the database from one valid state to another valid state. It never leaves the database in an invalid state.
- **Isolation**: many transactions can run at the same time without interfering with each other. It looks to each user as if they are alone on the system.
- **Durability**: once a transaction is committed, its effects are permanent, even if the computer crashes right after.

### Components of a DBMS

The main parts inside a DBMS are:
- Query processor
- DDL and DML compilers
- Storage manager
- Transaction manager
- Buffer manager
- File manager
- Data dictionary (the DBMS's own catalog of tables and columns)
- The database itself
- The user interface (login screen with username and password)

### Multi-user DBMS architecture

A DBMS is designed for many users and many application programs to connect at the same time and work on the same database. It handles this using **concurrency control** and **locking**, which ensure two users cannot corrupt each other's data. A plain file system has no such mechanism.

### Possible Exam Questions

- Define DBMS. Give five examples of DBMS software.
- Explain the advantages of DBMS over a file-processing system.
- What are ACID properties? Explain each with an example.
- List and explain the main components of a DBMS.
- Draw and explain the DBMS approach for data management.
- Explain multi-user DBMS architecture.

---

## Chapter 4: Database Structures (Logical and Physical)

### What is a database?

A **database** is an organized collection of interrelated data. Another way to say it: a database is a collection of structured data.

Every database has two kinds of structures:

### 1. Logical Structure

A structure which is **not visible in the operating system** is called a logical structure.

Logical structure contains: tables, views, sequences, synonyms, and so on.

Logical structure is handled by the **database developer** or by the **database administrator**.

### 2. Physical Structure

A structure which **is visible in the operating system** is called a physical structure. This is the actual files on disk.

Physical structure is handled by the **DBA (Database Administrator) only**.

### Possible Exam Questions

- Define database.
- Explain the two types of structures a database has.
- Who handles logical and physical structures?

---

## Chapter 5: The Three-Level Database Architecture

### Who defined it

**ANSI** (American National Standards Institute) established a three-level architecture for DBMS. It is also called the **ANSI/SPARC** architecture, where SPARC stands for Standards Planning And Requirements Committee.

### Main objective

To **separate the user's view of the database from the way the data is physically stored**. In other words, users should see the database the way they want, while the DBA controls how the data is really stored.

### The three levels

**Diagram**

```
              +-------+  +-------+  +-------+
              | View1 |  | View2 |  | View3 |     External Level
              +-------+  +-------+  +-------+     [views, synonyms]
                    \        |         /
                     \       |        /             Logical Data Independence
                      \      |       /
                    +------------------+
                    | Conceptual Level |           [Tables]
                    +------------------+
                             |                     Physical Data Independence
                             |
                    +------------------+
                    |  Internal Level  |           [Index, Cluster]
                    +------------------+
                             |
                        +---------+
                        | Database|
                        +---------+
```

**1. Conceptual Level (Middle)**

Describes the logical structure of the whole database for all users combined. This level does not define how data is physically stored.

At this level we define:
- What type of data can be stored (data type and size)
- What data cannot be stored (through constraints such as primary key and foreign key)
- The relationships between tables

Example SQL at the conceptual level:
```sql
CREATE TABLE bank (
    accno   NUMBER(10) PRIMARY KEY,
    name    VARCHAR2(10),
    balance NUMBER(10)
);
```

**2. External Level (Top)**

Describes the user's view of the database. Different users see only what is relevant to them. This provides a security mechanism through **views**.

Example: think of an e-commerce database.
- A Customer's view might show: item name, price
- A Purchase Manager's view might show: item name, price, quantity

Both views come from the same underlying table, but each user sees only what they need.

**Diagram**

```
    +-------------+           +---------------+
    |  Customer   |           | Purchase Mgr. |
    +-------------+           +---------------+
    | Item name   |           | Item name     |
    | Price       |           | Price         |     External Level
    +-------------+           | Quantity      |
           ^                  +---------------+
           |                          ^
           |                          |
           +-------+          +-------+
                   |          |
              +-----------------------+
              |      Item Table       |
              | (Itemno, Itemname,    |     Conceptual Level
              |  Price, Quantity)     |
              +-----------------------+
                          |
                    +------------+
                    |  Internal  |          Internal Level
                    |   Level    |
                    +------------+
                          |
                    +----------+
                    | Database |
                    +----------+
```

The DBA creates these views from the conceptual level and gives each view to the right kind of user.

**3. Internal Level (Bottom)**

Describes how data is physically stored on disk. Indexes and clusters live here. Only the **DBA** works at this level.

When we add an index to a table on the database, this structural change does not affect the conceptual level (that is physical data independence), but it does affect performance (usually improving read speed).

### Possible Exam Questions

- Explain the three-level ANSI/SPARC architecture of a DBMS with a diagram.
- What is the main objective of DBMS architecture?
- Explain the conceptual, external, and internal levels.
- Give an example of how the external level provides security.

---

## Chapter 6: Data Independence

### Meaning

**Data independence** means that upper levels of the DBMS architecture are not affected by changes made at lower levels. This is a key benefit of the three-level architecture.

There are two types.

### 1. Logical Data Independence

Changes made at the **conceptual level do not require changes at the external level**.

Example: if we add a new table (a new entity) at the conceptual level, existing views at the external level should still work fine and should not need to be rewritten.

### 2. Physical Data Independence

Changes made at the **internal level do not require changes at the conceptual level**.

Example: if we add a new index at the internal level, the conceptual level (tables and constraints) does not need to change. Only performance is affected.

### Possible Exam Questions

- What is data independence?
- Explain logical data independence with an example.
- Explain physical data independence with an example.
- Differentiate between logical and physical data independence.

---

## Chapter 7: History of Data Models

A **data model** defines how data is represented at the conceptual level. Three data models have been used in the history of database design.

### 1. Hierarchical Data Model (1960s onwards)

Data is organized in a **tree-like structure**. In this model, "record type" is the same concept as "table" in the relational model.

- Implemented on a **one-to-many relationship** between parent and child records.
- Because each child has only one parent, child records get **repeated**, causing more duplicate data.
- In the 1960s, **IBM introduced IMS** (Information Management System) as a product based on this model.
- Requires a **procedural language** to operate.

### 2. Network Data Model (1970s)

- In the 1970s, the **CODASYL** (Conference on Data System Language) committee introduced this model.
- Uses a **many-to-many relationship** between parent and child, so there is less duplicate data than in the hierarchical model.
- Data is again represented in the format of records, and record type = table in the relational world.
- In 1970, **IBM introduced IDMS** (Information Data Management System) based on this model.
- Also requires a procedural language.

### 3. Relational Data Model (1970)

- Introduced by **E.F. Codd** in 1970.
- Consists of a collection of relations, represented as tables.
- Data is stored in a 2-D table (rows and columns).
- Three main components:
  1. Collection of objects (tables, views, indexes, synonyms, clusters)
  2. Set of operators
  3. Set of integrity rules
- In 1970, E.F. Codd wrote a paper called **"A Relational Data Model for Large Shared Data Banks"** using the **DSL/Alpha** language.
- This paper led to the development of Normal Forms: 1NF, 2NF, 3NF.

### Comparison: Hierarchical vs Relational

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

### Timeline of relational products

- IBM's System/R (used a language called SQUARE)
- INGRES (1978)
- Oracle (1977), which became Oracle Corporation (1982)
- CEO of Oracle Corporation: **Larry Ellison**

### Possible Exam Questions

- What is a data model?
- Explain the Hierarchical Data Model with an example.
- Explain the Network Data Model. Who introduced it?
- Explain the Relational Data Model. Who introduced it and in which year?
- Differentiate between Hierarchical, Network, and Relational data models.
- What are the three components of the Relational Data Model?

---

## Chapter 8: Data Modeling — Entities, Attributes, Relationships, Keys

### Entities

An **entity** is a real-world object or thing about which data is stored. Examples: Student, Employee, Course, Book.

### Attributes

An **attribute** is a property that describes an entity.

Types of attributes:
- **Simple** (indivisible, e.g. age) vs **Composite** (made of parts, e.g. name = first + last)
- **Single-valued** (one value, e.g. date of birth) vs **Multi-valued** (many values, e.g. phone numbers)
- **Stored** (kept as-is) vs **Derived** (calculated, e.g. age from DOB)
- **Key attribute** (uniquely identifies the entity)

### Relationships

A **relationship** is an association between two or more entities. Example: "works in" links Employee to Department.

- **Degree** of a relationship: unary (one entity), binary (two entities), ternary (three entities)
- **Cardinality**: one-to-one (1:1), one-to-many (1:N), many-to-many (M:N)

### Constraints

**Constraints** are rules that restrict what data is allowed:
- NOT NULL (field cannot be empty)
- UNIQUE (no duplicates)
- CHECK (custom condition)
- PRIMARY KEY (unique + not null)
- FOREIGN KEY (references another table's primary key)

### Keys

- **Candidate Key**: the smallest set of attributes that can uniquely identify a row.
- **Primary Key**: the candidate key chosen to identify rows. Cannot be NULL.
- **Alternate Key**: candidate keys that were not chosen as primary key.
- **Super Key**: any set of attributes (possibly with extra ones) that uniquely identifies a row.
- **Foreign Key**: an attribute in one table that refers to the primary key of another table. Enforces referential integrity.
- **Composite Key**: a primary key made of two or more attributes together.

**Example.** In a Student table:
- Candidate keys: {roll_no}, {email}
- Primary key: roll_no
- Alternate key: email
- Super key: {roll_no, name} (has extra attribute but still unique)

### Possible Exam Questions

- Define entity, attribute, and relationship with examples.
- What are the types of attributes? Give examples.
- What is cardinality of a relationship? Explain 1:1, 1:N, M:N.
- Explain candidate key, primary key, alternate key, super key, foreign key, and composite key with examples.
- Differentiate between primary key and candidate key.
- What are constraints? List and explain any five.

---

## Chapter 9: ER and EER Diagrams

### ER (Entity-Relationship) diagram notation

| Symbol | Meaning |
|---|---|
| Rectangle | Entity |
| Double rectangle | Weak entity |
| Ellipse | Attribute |
| Double ellipse | Multi-valued attribute |
| Dashed ellipse | Derived attribute |
| Diamond | Relationship |
| Double diamond | Identifying relationship (weak entity's link) |
| Line | Connects entities to attributes or relationships |
| Underline | Key attribute |
| Dashed underline | Partial key (of a weak entity) |

### Converting an ER diagram into tables

Follow these rules:

1. **Strong entity**: becomes one table. Its key attribute becomes the primary key.
2. **Weak entity**: becomes one table. Include the primary key of its owner (the strong entity) as a foreign key, combined with the weak entity's partial key to form a composite primary key.
3. **1:1 relationship**: add either entity's primary key as a foreign key in the other's table.
4. **1:N relationship**: add the primary key of the "1" side as a foreign key in the table on the "N" side.
5. **M:N relationship**: create a new table with the primary keys of both entities forming a composite primary key, plus any attributes of the relationship itself.
6. **Multi-valued attribute**: create a separate table with the attribute value and the primary key of the owning entity.

### EER (Enhanced ER) model

The EER model extends the basic ER model with these concepts:

- **Generalization**: combining lower-level entities that share common features into a higher-level entity. Bottom-up.
- **Specialization**: dividing a higher-level entity into lower-level sub-entities. Top-down.
- **Aggregation**: treating a whole relationship as if it were a higher-level entity.
- **Inheritance**: sub-entities automatically inherit attributes of the super-entity.
- **Disjoint vs Overlapping** and **Total vs Partial** participation constraints for specialization.

### Converting EER into tables (one-table-per-subclass approach)

1. Create one table for the superclass with all the common attributes plus the primary key.
2. For each subclass, create a separate table with only that subclass's own attributes plus the superclass's primary key (which also acts as a foreign key back to the superclass).

**Example.** Entity: Person. Subclasses: Student and Employee.
- Table Person(pid, name, dob) with pid as primary key
- Table Student(pid, roll_no, cgpa) with pid as PK and FK to Person
- Table Employee(pid, empid, salary) with pid as PK and FK to Person

### Possible Exam Questions

- Explain the components of an ER diagram with symbols.
- Explain the rules to convert an ER diagram into tables.
- What is a weak entity? How is it converted to a table?
- Explain generalization, specialization, and aggregation in EER.
- Convert a given EER diagram into tables.
- Differentiate between ER and EER models.

---

## Chapter 10: The Relational Model

### Basic terms

- **Relation**: a table.
- **Tuple**: a row.
- **Attribute**: a column.
- **Domain**: the set of allowable values for an attribute. Example: domain of "age" might be positive integers under 150.
- **Degree**: number of attributes (columns) in a relation.
- **Cardinality**: number of tuples (rows) in a relation.

### Example

Consider EMP(empno, ename, sal) with 3 rows.
- Relation: EMP
- Tuples: the 3 rows
- Attributes: empno, ename, sal
- Degree: 3 (three columns)
- Cardinality: 3 (three rows)

### Possible Exam Questions

- Define relation, tuple, attribute, domain, degree, and cardinality with examples.
- What is the difference between degree and cardinality?
- Explain the basic concepts of the relational model.

---

## Chapter 11: Codd's Rules

E.F. Codd proposed 12 rules that a system must follow to be truly relational.

1. **Information Rule**: all data must be represented as values in tables.
2. **Guaranteed Access Rule**: every value must be reachable using table name + primary key + column name.
3. **Systematic treatment of NULL values**: NULLs must be handled consistently and treated differently from zero or blank.
4. **Active online catalog**: the database's description must be stored as tables (data dictionary), queryable in the same way as normal data.
5. **Comprehensive data sublanguage rule**: one language (like SQL) must be able to define, manipulate, and control the data.
6. **View updating rule**: any view that is theoretically updatable must be updatable by the system.
7. **High-level insert, update, delete**: the system must support set-at-a-time operations, not just row-at-a-time.
8. **Physical data independence**: applications should not be affected by changes to how data is stored physically.
9. **Logical data independence**: applications should not be affected by changes to the logical schema (as far as possible).
10. **Integrity independence**: integrity rules must be stored in the catalog, not in application code.
11. **Distribution independence**: users should not be aware of whether the database is distributed.
12. **Non-subversion rule**: low-level record-at-a-time access must not be able to bypass integrity rules enforced at the high level.

### Possible Exam Questions

- Who proposed Codd's rules and why?
- List and briefly explain Codd's 12 rules.
- Explain the information rule and the guaranteed access rule.
- What is the non-subversion rule?

---

## Chapter 12: Relational Integrity

### NULL

**NULL** is an undefined, unknown, or unavailable value. It is **not** the same as zero or a blank space.

Rule: any arithmetic operation involving NULL produces NULL.

Example: `null + 50 = null`

### Entity Integrity

Every table must have a **primary key that is unique and NOT NULL**. This guarantees every row can be uniquely identified.

### Referential Integrity

A **foreign key** value must either:
- Match an existing primary key value in the referenced table, or
- Be NULL

This prevents "orphan" rows that point to something that does not exist.

**Example.** In EMP.deptno, every non-null value must appear in DEPT.deptno. You cannot have an employee with deptno = 99 if department 99 does not exist.

### Enterprise Constraints

These are business rules specific to an organization, beyond entity and referential integrity.

Example: "manager salary must be greater than 10000."

### Views and schema diagrams

- A **view** is a database object providing a level of security. It behaves like a virtual table built from a SELECT statement. Views do not store data (except materialized views). Full details in Chapter 31.
- A **schema diagram** shows all tables, their columns, and the primary key / foreign key links between them.

### Possible Exam Questions

- What is NULL? How does it differ from zero?
- Explain entity integrity and referential integrity with examples.
- What are enterprise constraints?
- What is the result of `null + 50`? Why?
- What is a schema diagram?

---

# PART II: RELATIONAL QUERY LANGUAGES

---

## Chapter 13: Relational Algebra

Relational algebra is a **procedural** query language. You describe a sequence of operations, and each operation on a relation produces a new relation.

### Operators

| Operator | Symbol | Meaning |
|---|---|---|
| Select | σ (sigma) | picks rows satisfying a condition |
| Project | π (pi) | picks specific columns |
| Union | ∪ | combines rows from two compatible relations, removes duplicates |
| Set Difference | − | rows in R1 but not in R2 |
| Intersection | ∩ | rows common to both R1 and R2 |
| Cartesian Product | × | pairs every row of R1 with every row of R2 |
| Rename | ρ (rho) | renames a relation or attribute |
| Join | ⋈ | combines related rows from two relations based on a condition |
| Division | ÷ | used for "for all" queries |

### Simple examples

- σ<sub>sal>2000</sub>(EMP): select all rows from EMP where sal > 2000
- π<sub>ename, sal</sub>(EMP): project only ename and sal columns from EMP
- EMP ⋈<sub>emp.deptno = dept.deptno</sub> DEPT: join EMP and DEPT on matching deptno

### Possible Exam Questions

- What is relational algebra? Is it procedural or non-procedural?
- Explain the select and project operators with examples.
- List all relational algebra operators and their symbols.
- Differentiate between Cartesian product and join.

---

## Chapter 14: Relational Calculus

Relational calculus is a **non-procedural (declarative)** query language. You describe what you want, not how to get it.

### Tuple Relational Calculus (TRC)

Uses tuple variables. Written as:

```
{ t | P(t) }
```

Meaning: the set of all tuples `t` such that predicate `P(t)` is true.

**Example.** All employees earning more than 2000:
```
{ t | t IN EMP AND t.sal > 2000 }
```

### Domain Relational Calculus (DRC)

Uses domain variables (values from attribute domains) rather than whole tuples. Written as:

```
{ <x1, x2, ..., xn> | P(x1, x2, ..., xn) }
```

Meaning: the set of all value tuples that satisfy predicate `P`.

**Example.** Names and salaries of all employees earning more than 2000:
```
{ <name, sal> | exists empno (<empno, name, sal> IN EMP AND sal > 2000) }
```

### Possible Exam Questions

- What is relational calculus? How does it differ from relational algebra?
- Explain Tuple Relational Calculus with an example.
- Explain Domain Relational Calculus with an example.
- Differentiate between TRC and DRC.

---

## Chapter 15: Introduction to SQL

### Definition

**SQL** stands for **Structured Query Language**. SQL is a **non-procedural language** used to operate all relational databases.

### History

- 1970: E.F. Codd introduced **DSL/Alpha**.
- IBM's System/R team built a simplified version called **"Square"**.
- IBM renamed it to **"SEQUEL"** (Structured English Query Language).
- IBM later shortened "SEQUEL" to **"SQL"**.

### Standardization

- **ANSI**: 1986
- **ISO**: 1987
- Versions: SQL89, SQL92, SQL99, SQL2003

### Characteristics

- Non-procedural (say what, not how)
- Set-oriented (operates on many rows at once)
- English-like syntax
- Portable across vendors
- Can be used interactively or embedded in a program

### Advantages

- Easy to learn
- Standard across vendors
- Retrieves large amounts of data quickly
- Requires much less code than procedural languages

### Possible Exam Questions

- Define SQL. Is it procedural or non-procedural?
- Trace the history of SQL from DSL/Alpha to SQL.
- List the characteristics and advantages of SQL.
- When was SQL standardized by ANSI and ISO?

---

## Chapter 16: Sub-languages of SQL

SQL has five sub-languages, each with a specific purpose.

### DDL (Data Definition Language)

Used to define the structure of database objects.
- CREATE
- ALTER
- DROP
- TRUNCATE
- RENAME (Oracle 9i)

### DML (Data Manipulation Language)

Used to manipulate data inside a table.
- INSERT
- UPDATE
- DELETE
- MERGE (Oracle 9i)

### DQL / DRL (Data Query / Retrieval Language)

Used to fetch data.
- SELECT

### TCL (Transaction Control Language)

Used to manage transactions.
- COMMIT (save)
- ROLLBACK (like undo)
- SAVEPOINT

### DCL (Data Control Language)

Used to control access.
- GRANT (give permission)
- REVOKE (cancel permission)

**Important rule:** In all databases, by default, **DDL commands are automatically committed (saved)**. That is why you cannot roll back a DDL operation.

### Possible Exam Questions

- List the five sub-languages of SQL with their commands.
- What is the difference between DDL and DML?
- Which sub-language does SELECT belong to?
- Why can DDL commands not be rolled back?

---

## Chapter 17: SQL Data Types

Data types identify what kind of value can be stored in a column. Oracle's main data types are described below.

### 1. NUMBER(P, S)

Stores fixed or floating-point numbers.
- **P** = precision, the total number of digits.
- **S** = scale, the number of digits after the decimal point.
- Maximum digits before the decimal point = `P - S`.
- Maximum precision = 38 digits.

If you insert more digits after the decimal than `S` allows, Oracle **rounds automatically**. If you insert more digits before the decimal than allowed, Oracle throws an error.

**Example.**
```sql
CREATE TABLE test (sno NUMBER(7,2));
INSERT INTO test VALUES (12345.67);      -- OK
INSERT INTO test VALUES (123456.7);       -- ERROR: value too large
INSERT INTO test VALUES (12345.6789);     -- Stored as 12345.68 (rounded)
```

### 2. NUMBER(P)

Stores whole numbers only.

**Example.**
```sql
CREATE TABLE test1 (sno NUMBER(3));
INSERT INTO test1 VALUES (100);     -- 100
INSERT INTO test1 VALUES (99.4);    -- Stored as 99 (rounded)
```

### 3. CHAR(size)

Fixed-length alphanumeric data, up to 2000 bytes. Default size is 1 byte.

If the value is shorter than the declared size, Oracle pads the rest with blank spaces. This is called the **blank-padded mechanism**.

**Example.**
```
CHAR(10) storing 'Murali':
+---+---+---+---+---+---+---+---+---+---+
| M | u | r | a | l | i |   |   |   |   |
+---+---+---+---+---+---+---+---+---+---+
                        <---blank padded--->
```

### 4. VARCHAR2(size)

Introduced in Oracle 7.0. Variable-length alphanumeric data, up to 4000 bytes.

Unlike CHAR, VARCHAR2 does **not** pad with blank spaces, so it does not waste disk space.

### 5. VARCHAR(size)

An older variable-length text type. Same idea as VARCHAR2, but maximum size is only 2000 bytes. Existed before Oracle 7.0.

### 6. DATE

Stores date and time. Default format is `DD-MON-YY`.

**Example.**
```sql
INSERT INTO emp VALUES ('10-JAN-25');
```

### Possible Exam Questions

- Explain the NUMBER(P, S) data type with an example.
- What happens if you insert more decimal digits than the scale allows in NUMBER(P, S)?
- Differentiate between CHAR and VARCHAR2.
- What is the blank-padded mechanism?
- What is the maximum precision of NUMBER in Oracle?
- Differentiate between VARCHAR and VARCHAR2.
- What is the default date format in Oracle?

---

## Chapter 18: DDL Commands

### 1. CREATE

Used to create objects like tables, views, and indexes.

**Creating a table:**
```sql
CREATE TABLE tablename (
    columnname1 datatype(size),
    columnname2 datatype(size),
    ...
);

CREATE TABLE first (sno NUMBER(10), name VARCHAR2(10));
```

**To view the structure of a table:**
```sql
DESC first;
-- Name    Type
-- ------  -------------
-- SNO     NUMBER(10)
-- NAME    VARCHAR2(10)
```

### 2. ALTER

Used to change the structure of an existing table. Three forms: ADD, MODIFY, DROP.

**ADD** adds columns to an existing table.
```sql
ALTER TABLE tablename ADD (col1 datatype(size), col2 datatype(size));
ALTER TABLE first ADD sal NUMBER(10);
```

**MODIFY** changes a column's datatype or size only.
```sql
ALTER TABLE tablename MODIFY (col1 datatype(size));
ALTER TABLE first MODIFY sno DATE;
```

**DROP** removes columns.
```sql
-- Method 1: single column, no parentheses
ALTER TABLE tablename DROP COLUMN columnname;
ALTER TABLE first DROP COLUMN sno;

-- Method 2: one or many columns, with parentheses
ALTER TABLE tablename DROP (col1, col2);
ALTER TABLE first DROP (name);
```

Rule: you cannot drop **all** columns of a table.
```sql
ALTER TABLE first DROP COLUMN sal;
-- ERROR: cannot drop all columns in a table
```

### 3. DROP

Removes a database object entirely.

```sql
DROP TABLE tablename;
DROP VIEW viewname;
```

- **Before Oracle 10g**: dropped tables were removed permanently.
- **Oracle 10g, 11g, 12c**: dropped tables first go to the **Recycle Bin**, from where they can be recovered.

```
Oracle Database                    Oracle Database
+-------------+                    +-------------+
|  Table      |                    |  Table      |
|  +------+   |                    |  +------+   |
|  |      |---> permanently        |  |      |---+
|  +------+   |    removed         |  +------+   |
+-------------+                    |             |
                                   |   Recycle   |
Before Oracle 10g                  |   Bin       |
                                   +-------------+
                                    Oracle 10g / 11g / 12c
```

**Recovering from recycle bin:**
```sql
FLASHBACK TABLE tablename TO BEFORE DROP;
```

**Dropping permanently (skipping or cleaning the recycle bin):**
```sql
DROP TABLE tablename PURGE;      -- skip recycle bin
PURGE TABLE tablename;            -- remove one table from recycle bin
PURGE RECYCLEBIN;                 -- empty the whole recycle bin
```

**About the Recycle Bin:**
- Introduced in Oracle 10g.
- A predefined read-only table that stores dropped tables.
- When Oracle server is installed, many predefined read-only tables are created automatically. These are collectively called the **Data Dictionary**.

```sql
DESC recyclebin;
SELECT original_name FROM recyclebin;
```

### 4. TRUNCATE

Introduced in Oracle 7.0. Deletes **all rows permanently** from a table but keeps the structure.

```sql
TRUNCATE TABLE tablename;
```

**Example.**
```sql
CREATE TABLE first AS SELECT * FROM emp;
SELECT * FROM first;          -- shows rows
TRUNCATE TABLE first;
SELECT * FROM first;          -- no rows selected
DESC first;                   -- structure still exists
```

### 5. RENAME (Oracle 9i)

Renames a table.
```sql
RENAME oldtablename TO newtablename;
RENAME first TO second;
```

**Renaming a column (Oracle 9i):**
```sql
ALTER TABLE tablename RENAME COLUMN oldcolumnname TO newcolumnname;
ALTER TABLE emp RENAME COLUMN empno TO sno;
```

### Difference between DELETE and TRUNCATE

| DELETE | TRUNCATE |
|---|---|
| DML command | DDL command |
| Deleted data is held in a buffer, so ROLLBACK can bring it back | Rows are permanently gone. ROLLBACK cannot recover them, because DDL is auto-committed |
| Can delete specific rows using WHERE | Deletes all rows only |
| Slower on large tables | Faster |

### Possible Exam Questions

- What are DDL commands? List all of them.
- Explain CREATE, ALTER, DROP, TRUNCATE, and RENAME with syntax and examples.
- What is the difference between DELETE and TRUNCATE?
- What is the Recycle Bin in Oracle? How do you recover a dropped table?
- How do you drop a table permanently, skipping the recycle bin?
- Can you drop all columns of a table? Why or why not?
- Explain the three forms of the ALTER command.

---

## Chapter 19: DML Commands

DML commands manipulate data within a table. Four commands: INSERT, UPDATE, DELETE, MERGE.

### 1. INSERT

**Method 1: direct values.**
```sql
INSERT INTO tablename VALUES (value1, value2, ...);
INSERT INTO first VALUES (1, 'murali');
```

**Method 2: substitution operator (&).**
The `&` operator asks the user for a value at runtime.
```sql
INSERT INTO first VALUES (&sno, '&name');
-- Enter value for sno: 3
-- Enter value for name: abc

-- Typing "/" re-runs the previous command:
/
```

**Method 3: skipping columns.**
Insert into specific columns only, leaving the rest NULL.
```sql
INSERT INTO tablename (col1, col2) VALUES (val1, val2);
INSERT INTO first (name) VALUES ('zzz');   -- sno will be NULL
```

### 2. UPDATE

Changes existing data.
```sql
UPDATE tablename SET columnname = newvalue WHERE condition;
UPDATE emp SET sal = 1000 WHERE ename = 'SMITH';

-- Clearing a value:
UPDATE first SET address = NULL WHERE name = 'sud';
```

UPDATE can also be used to fill in a currently empty (NULL) cell.

### 3. DELETE

```sql
DELETE FROM tablename;                       -- deletes all rows
DELETE FROM tablename WHERE condition;        -- deletes specific rows

-- Recovering deleted rows before commit:
ROLLBACK;
```

### Possible Exam Questions

- What are DML commands? List them.
- Explain the three methods of INSERT with examples.
- Explain UPDATE with syntax and an example.
- What happens if you use DELETE without a WHERE clause?
- Can deleted rows be recovered? Explain how.

---

## Chapter 20: The SELECT Statement (DQL)

SELECT is used to fetch data from the database.

### Basic syntax

```sql
SELECT * FROM tablename;
```

### Full syntax with all clauses (in required order)

```sql
SELECT col1, col2, ...
FROM tablename
WHERE condition
GROUP BY columnname
HAVING condition
ORDER BY columnname [ASC | DESC];
```

### Four basic forms of SELECT

**1. All columns and all rows.**
```sql
SELECT * FROM emp;
```

**2. All columns and specific rows.**
```sql
SELECT * FROM emp WHERE deptno = 10;
```

**3. Specific columns and all rows.**
```sql
SELECT ename, sal FROM emp;
```

**4. Specific columns and specific rows.**
```sql
SELECT ename, sal FROM emp WHERE deptno = 10;
```

### Creating a new table from an existing table

**Copying structure and data:**
```sql
CREATE TABLE newtable AS SELECT * FROM existingtable;
CREATE TABLE diwali AS SELECT * FROM emp;
```

Note: constraints (PK, FK) are **never copied**.

**Copying only structure (no data):**
Use a condition that is always false.
```sql
CREATE TABLE newtable AS SELECT * FROM existingtable WHERE 1=2;
CREATE TABLE abc AS SELECT * FROM emp WHERE 1=2;
-- abc has emp's structure but no rows
```

### Possible Exam Questions

- Write the full syntax of the SELECT statement.
- List and explain the four basic forms of SELECT.
- How do you create a new table from an existing one? Are constraints copied?
- How do you copy only the structure of a table without the data?

---

## Chapter 21: Operators in SQL

There are four types of operators used in SELECT statements.

1. Arithmetic operator: `+`, `-`, `*`, `/`
2. Relational operator: `=`, `<`, `>`, `>=`, `<=`, `<>` (or `!=`)
3. Logical operator: AND, OR, NOT
4. Special operator: IN, BETWEEN, IS NULL, LIKE (and their opposites)

Arithmetic operators are used on number and date data types.

### Arithmetic operator example (with column alias)

```sql
SELECT ename, sal, sal*12 annsal FROM emp;
--                       ^
--                       annsal is a column alias
-- ENAME    SAL     ANNSAL
-- SMITH    2200    26400
-- ALLEN    300     3600
```

### Relational operator examples

```sql
SELECT * FROM emp WHERE job <> 'CLERK';    -- not clerks
SELECT * FROM emp WHERE sal > 2000;         -- earning more than 2000
```

### Logical operator: AND

When you use AND, the database filters data matching **all** conditions. It narrows down the results.

```sql
SELECT * FROM emp WHERE job = 'CLERK' AND sal > 2000;
```

### Logical operator: OR

When you use OR, the database returns data matching **any one** of the conditions. It widens the results.

```sql
SELECT * FROM emp WHERE job = 'CLERK' OR sal > 2000;
```

Rule: to retrieve multiple values from a single column using equality, use OR (or the IN operator).

```sql
SELECT * FROM emp WHERE job = 'CLERK' OR job = 'SALESMAN';
SELECT * FROM emp
WHERE deptno = 20 OR deptno = 30 OR deptno = 50
   OR deptno = 70 OR deptno = 80;
```

### Special Operator 1: IN

Used to pick a value one by one from a list. Replaces multiple ORs. Performance is much higher than OR.

```sql
SELECT * FROM emp WHERE deptno IN (20, 30, 50, 70, 80);
SELECT * FROM emp WHERE ename IN ('SMITH', 'ALLEN', 'MILLER');
```

Note: `NOT IN` does not work if the list contains NULL. It returns no rows.

### Special Operator 2: BETWEEN

Retrieves a range of values (inclusive on both ends).

```sql
SELECT * FROM emp WHERE sal BETWEEN 2000 AND 5000;
SELECT * FROM emp WHERE sal NOT BETWEEN 2000 AND 5000;
```

### Special Operator 3: IS NULL, IS NOT NULL

Used only in WHERE to test whether a column has a NULL value.

Note: relational operators (=, <, >) do not work with NULL in a WHERE clause. You must use IS NULL / IS NOT NULL.

```sql
SELECT * FROM emp WHERE comm IS NULL;       -- no commission
SELECT * FROM emp WHERE comm IS NOT NULL;    -- has commission
```

### Special Operator 4: LIKE

Used to search based on a character pattern. Two wildcards:

- `%` matches any string of any length (including empty).
- `_` (underscore) matches exactly one character.

```sql
SELECT * FROM emp WHERE ename LIKE 'M%';       -- starts with M
SELECT * FROM emp WHERE ename LIKE '%M%';       -- contains M anywhere
SELECT * FROM emp WHERE ename LIKE '_L%';       -- second letter is L
SELECT * FROM emp WHERE ename LIKE '___M%';     -- fourth letter is M
SELECT * FROM emp WHERE hiredate LIKE '%DEC%';  -- hired in December
SELECT * FROM emp WHERE hiredate LIKE '%81';    -- hired in year 81
```

### ESCAPE clause with LIKE

Sometimes data itself contains a `%` or `_`. If you want to search for that literal character, use `ESCAPE`.

Example: an employee named `S_MITH`.
```sql
SELECT * FROM emp WHERE ename LIKE 'S_%';
-- returns SCOTT, SMITH, S_MITH   (WRONG, we only wanted S_MITH)

-- Correct with ESCAPE:
SELECT * FROM emp WHERE ename LIKE 'S?_%' ESCAPE '?';
-- returns S_MITH only
```

The escape character tells Oracle: "the character right after me is a literal, not a wildcard."

Note: escape character length must be exactly 1 byte.

### Concatenation operator (||)

Joins column data with literal text or with other columns.

```sql
SELECT 'my employee name is: ' || ename FROM emp;
-- my employee name is: SMITH
-- my employee name is: ALLEN

SELECT ename || ' ' || sal FROM emp;
-- SMITH 2200
```

### Possible Exam Questions

- What are the four types of operators used in SELECT?
- Explain arithmetic and relational operators with examples.
- Differentiate between AND and OR.
- Explain IN, BETWEEN, IS NULL, and LIKE with examples.
- What is the ESCAPE clause? Give an example.
- Explain the concatenation operator with an example.
- Why must you use IS NULL instead of `= NULL`?
- What is the difference between IN and OR? Which performs better?

---

## Chapter 22: NULL Handling with NVL and NVL2

### NVL()

**Definition:** NVL is a predefined function used to replace a user-defined value in place of NULL.

Syntax:
```sql
NVL(exp1, exp2)
```

Both must be the same data type. If `exp1` is NULL, returns `exp2`. Otherwise returns `exp1`.

```sql
SELECT NVL(null, 20) FROM dual;    -- 20
SELECT NVL(10, 20) FROM dual;      -- 10
```

**Use case.** Any arithmetic operation with NULL gives NULL. NVL fixes that.

```sql
-- Without NVL:
SELECT ename, sal, comm, sal+comm FROM emp WHERE ename='SMITH';
-- SMITH  2200  null  null    <- wrong, we wanted 2200

-- With NVL:
SELECT ename, sal, comm, sal + NVL(comm, 0) FROM emp WHERE ename='SMITH';
-- SMITH  2200  null  2200    <- correct
```

### NVL2() (Oracle 9i)

**Definition:** NVL2 replaces a value based on whether the first argument is null. Takes three parameters.

Syntax:
```sql
NVL2(exp1, exp2, exp3)
```

If `exp1` is NULL, returns `exp3`. Otherwise returns `exp2`.

```sql
SELECT NVL2(null, 10, 20) FROM dual;    -- 20
SELECT NVL2(30, 10, 20) FROM dual;      -- 10
```

**Use case.** Update commission using the rule: if comm is null, set to 500; if comm is not null, add 500.
```sql
UPDATE emp SET comm = NVL2(comm, comm+500, 500);
```

### DUAL table

**DUAL** is a predefined virtual table with one column and one row. It is used to test predefined and user-defined functions.

```sql
SELECT NVL(null, 10) FROM dual;    -- 10
SELECT ABS(-50) FROM dual;          -- 50
```

### Possible Exam Questions

- What is NVL? Explain with syntax and example.
- What is NVL2? How is it different from NVL?
- What is the DUAL table? Why is it used?
- Why do we need NVL when performing arithmetic with commission?

---

## Chapter 23: Number and Character Functions

Functions are used to solve a particular task and always return a value. Oracle has two kinds:
1. Predefined functions
2. User-defined functions

Predefined functions are categorized into:
- Number functions
- Character functions
- Date functions
- Group (aggregate) functions

### Number functions

| Function | Description | Example | Output |
|---|---|---|---|
| `ABS(n)` | negative to positive | `ABS(-50)` | 50 |
| `ROUND(n, d)` | rounds to d decimal places | `ROUND(45.926, 2)` | 45.93 |
| `TRUNC(n, d)` | truncates to d decimal places, no rounding | `TRUNC(45.926, 2)` | 45.92 |
| `MOD(m, n)` | remainder of m/n | `MOD(10, 3)` | 1 |
| `POWER(m, n)` | m raised to n | `POWER(2, 3)` | 8 |
| `SQRT(n)` | square root | `SQRT(16)` | 4 |

Example:
```sql
SELECT ABS(-50) FROM dual;    -- 50
```

### Character functions

| Function | Description | Example |
|---|---|---|
| `LENGTH(s)` | number of characters | `LENGTH('SMITH')` = 5 |
| `UPPER(s)` | to uppercase | `UPPER('abc')` = 'ABC' |
| `LOWER(s)` | to lowercase | `LOWER('ABC')` = 'abc' |
| `INITCAP(s)` | capitalizes first letter of each word | `INITCAP('smith')` = 'Smith' |
| `SUBSTR(s, pos, len)` | extracts substring | `SUBSTR('COMPUTER', 2, 2)` = 'OM' |
| `INSTR(s, 'c')` | position of a character | `INSTR('ABC*D', '*')` = 4 |
| `LPAD(s, len, 'c')` | left-pad | `LPAD('ABCD', 10, '#')` = '######ABCD' |
| `RPAD(s, len, 'c')` | right-pad | `RPAD('ABCD', 10, '#')` = 'ABCD######' |
| `LTRIM(s, 'set')` | strip from left | `LTRIM('SSMISS', 'S')` = 'MISS' |
| `RTRIM(s, 'set')` | strip from right | `RTRIM('SSMISS', 'S')` = 'SSMI' |
| `TRIM('c' FROM 's')` | strip from both ends | `TRIM('S' FROM 'STHS')` = 'TH' |
| `TRANSLATE(s1, s2, s3)` | replace character by character | `TRANSLATE('india', 'in', 'xy')` = 'xydxa' |
| `REPLACE(s1, s2, s3)` | replace string by string | `REPLACE('india', 'in', 'xy')` = 'xydia' |
| `CONCAT(s1, s2)` | join two strings | `CONCAT('wel', 'come')` = 'welcome' |

### Examples

**LENGTH and SUBSTR:**
```sql
-- Employees whose 2nd letter of ename is 'LA'
SELECT * FROM emp WHERE SUBSTR(ename, 2, 2) = 'LA';

-- Employees whose ename is 5 characters long
SELECT * FROM emp WHERE LENGTH(ename) = 5;
-- SMITH, CLARK
```

Note: you cannot use group functions in WHERE, but you can use number(), character(), and date() functions in WHERE.

**INSTR full syntax:**
```
INSTR(columnname or 'string', 'search_str', start_position, occurrence)
                                                  ^              ^
                                            +ve or -ve      number
```

```sql
SELECT INSTR('ABC*D', '*') FROM dual;    -- 4
SELECT INSTR('AB*CDEFGHCDIJKLCDMNP', 'CD', -6, 2) FROM dual;    -- 3
-- (searches from right, prints position from left)
```

**LPAD and RPAD:**
```sql
SELECT LPAD('ABCD', 10, '#') FROM dual;    -- '######ABCD'
SELECT RPAD('ABCD', 10, '#') FROM dual;    -- 'ABCD######'
```

If the string is already longer than the total length, LPAD and RPAD return the original string truncated from the left.

**LTRIM, RTRIM, TRIM:**
```sql
SELECT LTRIM('SSMISSTHSS', 'S') FROM dual;    -- 'MISSTHSS'
SELECT RTRIM('SSMISSTHSS', 'S') FROM dual;    -- 'SSMISSTH'
SELECT TRIM('S' FROM 'SSTHSSMISS') FROM dual;    -- 'THSSMI'
SELECT TRIM(LEADING 'S' FROM 'SSMISSTHSS') FROM dual;   -- 'MISSTHSS' (like LTRIM)
SELECT TRIM(TRAILING 'S' FROM 'SSMISSTHSS') FROM dual;  -- 'SSMISSTH' (like RTRIM)
```

**TRANSLATE vs REPLACE:**
```sql
SELECT TRANSLATE('india', 'in', 'xy') FROM dual;    -- 'xydxa'  (i to x, n to y)
SELECT REPLACE('india', 'in', 'xy') FROM dual;      -- 'xydia'  (whole 'in' becomes 'xy')

SELECT REPLACE('SSMISSTHSS', 'S') FROM dual;         -- 'MITH'  (2nd arg removed when 3rd omitted)
```

**Counting occurrences of a character:**
```sql
SELECT LENGTH('SLEEP') - LENGTH(REPLACE('SLEEP', 'E')) FROM dual;    -- 2
```

**CONCAT:**
```sql
SELECT CONCAT('wel', 'come') FROM dual;    -- 'welcome'
```

### Possible Exam Questions

- What are the two types of functions in Oracle?
- Explain any five number functions with examples.
- Differentiate between ROUND and TRUNC.
- Explain LENGTH, SUBSTR, INSTR with examples.
- Explain LPAD and RPAD with examples.
- Explain LTRIM, RTRIM, TRIM.
- Differentiate between TRANSLATE and REPLACE with examples.
- Write a query to count the number of times a character appears in a string.
- What happens when REPLACE is used without the third argument?

---

## Chapter 24: Date Functions

Oracle's default date format is `DD-MON-YY`. Oracle has these date functions:

1. SYSDATE
2. ADD_MONTHS()
3. LAST_DAY()
4. NEXT_DAY()
5. MONTHS_BETWEEN()

### 1. SYSDATE

Returns the current system date.
```sql
SELECT SYSDATE FROM dual;    -- 17-NOV-15
```

### 2. ADD_MONTHS(date, n)

Adds n months to the given date. If n is negative, subtracts months.
```sql
SELECT ADD_MONTHS(SYSDATE, 5) FROM dual;    -- 17-APR-16
SELECT ADD_MONTHS(SYSDATE, -1) FROM dual;   -- 17-OCT-15
```

### 3. LAST_DAY(date)

Returns the last date of the month.
```sql
SELECT LAST_DAY(SYSDATE) FROM dual;    -- 30-NOV-15
```

### 4. NEXT_DAY(date, 'day')

Returns the next occurrence of the specified weekday after the given date.
```sql
-- SYSDATE = 17-NOV-15 (Tuesday)
SELECT NEXT_DAY(SYSDATE, 'Monday') FROM dual;    -- 23-NOV-15
```

### 5. MONTHS_BETWEEN(date1, date2)

Returns the number of months between two dates. If date1 < date2, the result is negative.
```sql
SELECT ename, ROUND(MONTHS_BETWEEN(SYSDATE, hiredate)) FROM emp;
```

### Date arithmetic

| Operation | Allowed? |
|---|---|
| Date + Number | Yes |
| Date - Number | Yes |
| Date1 + Date2 | Not allowed |
| Date1 - Date2 | Yes (gives number of days) |

```sql
SELECT SYSDATE + 1 FROM dual;
SELECT SYSDATE - 1 FROM dual;
SELECT SYSDATE - SYSDATE FROM dual;    -- 0
```

**Example.** First date of the current month:
```sql
SELECT LAST_DAY(ADD_MONTHS(SYSDATE, -1)) + 1 FROM dual;
-- 01-NOV-15
```

### Possible Exam Questions

- What is the default date format in Oracle?
- Explain SYSDATE, ADD_MONTHS, LAST_DAY, NEXT_DAY, MONTHS_BETWEEN with examples.
- Explain date arithmetic. Which operations are allowed?
- Write a query to display the first date of the current month using SYSDATE, ADD_MONTHS, and LAST_DAY.

---

## Chapter 25: Date Conversion Functions (TO_CHAR and TO_DATE)

### TO_CHAR()

Converts an Oracle date type into a character string.

Syntax:
```sql
TO_CHAR(date, 'format')
```

Example:
```sql
SELECT TO_CHAR(SYSDATE, 'DD/MM/YY') FROM dual;    -- 18/11/15
```

Important: TO_CHAR format codes are **case sensitive**.
```sql
SELECT TO_CHAR(SYSDATE, 'DAY') FROM dual;    -- 'WEDNESDAY'
SELECT TO_CHAR(SYSDATE, 'day') FROM dual;    -- 'wednesday'
```

### Common date format codes

| Code | Meaning |
|---|---|
| `D` | day of the week (1 = Sunday) |
| `DD` | day of the month |
| `DDD` | day of the year |
| `MM` | month number |
| `MON` | month, abbreviated (e.g. DEC) |
| `MONTH` | month, full name (e.g. DECEMBER) |
| `YY` | year, 2 digits |
| `YYYY` | year, 4 digits |
| `YEAR` | year spelled out |
| `DY` | day, abbreviated (WED) |
| `DAY` | day, full name |
| `HH` | 12-hour clock |
| `HH24` | 24-hour clock |
| `MI` | minutes |
| `SS` | seconds |
| `DDTH` | 18TH |
| `DDSPTH` | EIGHTEENTH |

Examples:
```sql
SELECT TO_CHAR(SYSDATE, 'D') FROM dual;         -- 4
SELECT TO_CHAR(SYSDATE, 'DDD') FROM dual;       -- 322
SELECT TO_CHAR(SYSDATE, 'DDTH') FROM dual;      -- 18TH
SELECT TO_CHAR(SYSDATE, 'DDSPTH') FROM dual;    -- EIGHTEENTH
SELECT TO_CHAR(SYSDATE, 'HH:MI:SS') FROM dual;  -- 02:51:33
SELECT TO_CHAR(SYSDATE, 'HH24:MI:SS') FROM dual;-- 14:51:33
```

### TO_DATE()

Converts a character string into an Oracle date type.

Syntax:
```sql
TO_DATE('date_string', 'format')
```

Example:
```sql
SELECT TO_DATE('12/june/05') FROM dual;    -- 12-JUN-05
SELECT TO_DATE('12/06/05') FROM dual;       -- ERROR: not a valid month
```

Rule: whenever using TO_DATE, the passed value must match the default date format, otherwise Oracle gives an error. To fix, pass a second argument matching the actual format:
```sql
SELECT TO_DATE('12/06/05', 'DD/MM/YY') FROM dual;    -- 12-JUN-05
```

### FM (Fill Mode)

TO_CHAR normally pads MONTH and DAY output with trailing spaces. FM suppresses leading zeros and trailing spaces.

```sql
SELECT TO_CHAR(TO_DATE('12-June-05'), 'DD/MONTH/YY') FROM dual;
-- '12/JUNE     /15'

SELECT TO_CHAR(TO_DATE('12-June-05'), 'DD/FMMONTH/YYYY') FROM dual;
-- '12/JUNE/2015'
```

### Query examples

```sql
-- Employees joining in December
SELECT * FROM emp WHERE TO_CHAR(hiredate, 'MM') = '12';
SELECT * FROM emp WHERE TO_CHAR(hiredate, 'MON') = 'DEC';

-- Using MONTH format needs FM to avoid padding
SELECT * FROM emp WHERE TO_CHAR(hiredate, 'FMMONTH') = 'DECEMBER';

-- Employees joining in year 81
SELECT * FROM emp WHERE TO_CHAR(hiredate, 'YY') = '81';
```

Rule: whenever we pass a date string into a predefined date function, Oracle **automatically** converts the string into date type if it matches the default format. Otherwise, use TO_DATE explicitly.

```sql
-- Automatic:
SELECT LAST_DAY('15-AUG-05') FROM dual;    -- 31-AUG-05

-- Needs explicit conversion:
SELECT LAST_DAY('15-08-15') FROM dual;    -- ERROR
SELECT LAST_DAY(TO_DATE('15-08-15', 'DD-MM-YY')) FROM dual;    -- 31-AUG-15
```

### ROUND and TRUNC on dates

Oracle date type contains both date and time. When using ROUND or TRUNC on dates, the time portion is set to zero and the date portion may change.

**ROUND(date):**
- If time is >= 12 noon, adds one day.
- Time is set to 00:00:00.

**TRUNC(date):**
- Never adds a day.
- Time is set to 00:00:00.

Example (at 2:30 PM, SYSDATE = 20-NOV-15 14:33:17):
```sql
SELECT TO_CHAR(SYSDATE, 'DD-MM-YY HH24:MI:SS') FROM dual;
-- 20-11-15 14:33:17

SELECT TO_CHAR(ROUND(SYSDATE), 'DD-MON-YY HH24:MI:SS') FROM dual;
-- 21-NOV-15 00:00:00

SELECT TO_CHAR(TRUNC(SYSDATE), 'DD-MON-YY HH24:MI:SS') FROM dual;
-- 20-NOV-15 00:00:00
```

Also, ROUND and TRUNC can return the first date of the year or the first date of the month:
```sql
ROUND(date, 'YEAR')      -- rounds to nearest Jan 1
ROUND(date, 'MONTH')     -- rounds to nearest 1st of the month
TRUNC(date, 'YEAR')      -- 1st Jan of same year
TRUNC(date, 'MONTH')     -- 1st of same month
```

Rule for ROUND on YEAR: Oracle checks if the month is above 50% (Jul-Dec) or below (Jan-Jun). If above, adds one year and rounds to Jan 1.

```sql
-- SYSDATE = 20-NOV-15
SELECT ROUND(SYSDATE, 'YEAR') FROM dual;    -- 01-JAN-16
SELECT ROUND(SYSDATE, 'MONTH') FROM dual;   -- 01-DEC-15
SELECT TRUNC(SYSDATE, 'YEAR') FROM dual;    -- 01-JAN-15
SELECT TRUNC(SYSDATE, 'MONTH') FROM dual;   -- 01-NOV-15
```

### Comparing dates

Oracle compares dates using both date and time.
```sql
-- Might return no rows because of time:
SELECT * FROM emp WHERE hiredate = SYSDATE;

-- Correct: strip time from both:
SELECT * FROM emp WHERE TRUNC(hiredate) = TRUNC(SYSDATE);
```

### Possible Exam Questions

- Explain TO_CHAR and TO_DATE with syntax and examples.
- Why is TO_CHAR case sensitive?
- List the common date format codes in Oracle.
- What is FM (Fill Mode) and why is it used?
- Differentiate between ROUND and TRUNC when applied to dates.
- Write a query to display employees hired in December.
- Write a query to display employees hired in year 1981 using TO_CHAR.
- When is TO_DATE needed and when is Oracle's automatic conversion enough?

---

## Chapter 26: Group (Aggregate) Functions

Group functions work over multiple rows and return a single value.

Oracle has these group functions:

| Function | Description |
|---|---|
| `MAX(col)` | maximum value |
| `MIN(col)` | minimum value |
| `AVG(col)` | average value (ignores NULLs) |
| `SUM(col)` | total |
| `COUNT(*)` | total number of rows (including NULLs) |
| `COUNT(col)` | count of non-null values in that column |

### Examples

```sql
SELECT MAX(sal) FROM emp;                    -- 7000
SELECT MIN(sal) FROM emp;                     -- 500
SELECT AVG(sal) FROM emp;                     -- 2848.21
SELECT SUM(sal) FROM emp;                     -- 39875
SELECT COUNT(*) FROM emp;                     -- 14
SELECT COUNT(comm) FROM emp;                  -- 4 (only 4 non-null commissions)
SELECT COUNT(DISTINCT deptno) FROM emp;       -- 3
```

### Important rule

Group functions are **not allowed in the WHERE clause**. They can only be used in SELECT or HAVING.

```sql
SELECT * FROM emp WHERE sal = MAX(sal);
-- ERROR: group function is not allowed here
```

### NULL and group functions

By default, all group functions **ignore NULL values**, except `COUNT(*)`.

```sql
SELECT AVG(comm) FROM emp;                -- 550 (nulls ignored)
SELECT AVG(NVL(comm, 0)) FROM emp;         -- 157.14 (nulls treated as 0)
```

### Possible Exam Questions

- List and explain all group functions in Oracle.
- Differentiate between COUNT(*) and COUNT(columnname).
- Why can we not use group functions in the WHERE clause?
- How do group functions handle NULL values?
- Write a query to find the maximum salary in each department.

---

## Chapter 27: GROUP BY, HAVING, and ORDER BY

### GROUP BY

Used to arrange similar data into logical groups. Whenever we use GROUP BY, the database selects similar data items from a column and reduces the number of items in each group.

Syntax:
```sql
SELECT columnname, ... FROM tablename GROUP BY columnname;
```

Examples:
```sql
-- Number of employees in each department
SELECT deptno, COUNT(*) FROM emp GROUP BY deptno;
-- DEPTNO  COUNT(*)
-- 10      3
-- 20      5
-- 30      6

-- Min and max salary per department
SELECT deptno, MIN(sal), MAX(sal) FROM emp GROUP BY deptno;
```

**Rule:** every column in SELECT (other than group function columns) must also appear in GROUP BY. Otherwise Oracle gives the error "not a GROUP BY expression."

```sql
SELECT deptno, SUM(sal), job FROM emp GROUP BY deptno;
-- ERROR

-- Correct:
SELECT deptno, SUM(sal), job FROM emp GROUP BY deptno, job;
```

You cannot mix group functions with normal columns without GROUP BY:
```sql
SELECT deptno, SUM(sal) FROM emp;
-- ERROR: not a single-group group function

-- Correct:
SELECT deptno, SUM(sal) FROM emp GROUP BY deptno;
```

### HAVING

After GROUP BY, you cannot use WHERE. Instead, use HAVING to filter groups.

- WHERE filters rows.
- HAVING filters groups.
- Group functions are **allowed in HAVING**, unlike WHERE.

Syntax:
```sql
SELECT columnname FROM tablename
GROUP BY columnname
HAVING condition;
```

Examples:
```sql
-- Departments with more than 3 employees
SELECT deptno, COUNT(*) FROM emp GROUP BY deptno HAVING COUNT(*) > 3;

-- Departments where total salary > 9000
SELECT deptno, SUM(sal) FROM emp GROUP BY deptno HAVING SUM(sal) > 9000;

-- Years where more than 1 employee was hired
SELECT TO_CHAR(hiredate, 'YYYY') "YEAR", COUNT(*) FROM emp
GROUP BY TO_CHAR(hiredate, 'YYYY') HAVING COUNT(*) > 1;
```

### ORDER BY

Sorts the final result in ascending (ASC, default) or descending (DESC) order.

Syntax:
```sql
SELECT * FROM tablename ORDER BY columnname [ASC | DESC];
```

Examples:
```sql
SELECT sal FROM emp ORDER BY sal DESC;
SELECT * FROM emp ORDER BY ename ASC;

-- Multiple columns
SELECT deptno, sal FROM emp ORDER BY deptno, sal DESC;
-- sorts by deptno first, then by sal within each deptno
```

### Complete SELECT clause order

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

### Possible Exam Questions

- Explain GROUP BY with an example.
- What is the difference between WHERE and HAVING?
- Why can we not use group functions in WHERE but can use them in HAVING?
- Write the complete syntax of SELECT in the correct clause order.
- Write a query to find departments with more than 3 employees.
- Write a query to find years where more than 1 employee was hired.
- Explain ORDER BY with ASC and DESC.

---

## Chapter 28: ROLLUP and CUBE

Oracle 8i introduced ROLLUP and CUBE. They are used only with GROUP BY.

They automatically calculate **subtotals and grand totals**.

- **ROLLUP**: subtotal based on a hierarchy of columns.
- **CUBE**: subtotals for **all combinations** of the given columns.

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
--              64000     <- grand total added by ROLLUP
```

### Possible Exam Questions

- What is the purpose of ROLLUP and CUBE?
- Differentiate between ROLLUP and CUBE.
- When were ROLLUP and CUBE introduced?
- Write a query using ROLLUP to display dept-wise total salary with grand total.

---

## Chapter 29: Sub-queries (Nested Queries)

A **sub-query** is a query inside another query. The inner query executes first, and its result is used by the outer query.

### Single-row sub-query

Returns one value. Used with =, >, <, etc.
```sql
SELECT * FROM emp WHERE sal > (SELECT sal FROM emp WHERE ename = 'JONES');
```

### Multi-row sub-query

Returns many values. Used with IN, ANY, ALL.
```sql
SELECT * FROM emp WHERE deptno IN (SELECT deptno FROM dept WHERE loc = 'CHICAGO');
SELECT * FROM emp WHERE sal > ANY (SELECT sal FROM emp WHERE deptno = 30);   -- > min of list
SELECT * FROM emp WHERE sal > ALL (SELECT sal FROM emp WHERE deptno = 30);   -- > max of list
```

### Correlated sub-query

Inner query references the outer query's table. Runs once for each row of the outer query.
```sql
SELECT ename, sal, deptno FROM emp e
WHERE sal > (SELECT AVG(sal) FROM emp WHERE deptno = e.deptno);
```

### EXISTS / NOT EXISTS

Checks whether the sub-query returns any rows.
```sql
SELECT * FROM dept d WHERE EXISTS (SELECT 1 FROM emp e WHERE e.deptno = d.deptno);
```

### Possible Exam Questions

- What is a sub-query? How does the inner query execute?
- Differentiate between single-row and multi-row sub-queries.
- What is a correlated sub-query? Give an example.
- Explain the ANY and ALL operators with examples.
- Write a query to find employees whose salary is greater than the average salary of their department.

---

## Chapter 30: Joins

Joins are used to retrieve data from multiple tables. If we are joining n tables, we need n - 1 joining conditions.

Oracle supports:

**8i style joins:**
1. Equi Join (Inner Join)
2. Non-Equi Join
3. Self Join
4. Outer Join

**9i / ANSI style joins:**
1. Inner Join
2. Left Outer Join
3. Right Outer Join
4. Full Outer Join
5. Natural Join
6. Cross Join

### Cross Join

If you list tables without a join condition, Oracle performs a cross join internally (Cartesian product). Returns rows = (rows in table1) × (rows in table2).
```sql
SELECT ename, sal, dname, loc FROM emp, dept;
-- if emp has 14 rows and dept has 4 rows: output = 56 rows
```

### 1. Equi Join (Inner Join, 8i style)

Uses an **equality condition** on a common column. Both tables must have a common column of the same data type.

Syntax:
```sql
SELECT col1, col2, ...
FROM tablename1, tablename2
WHERE tablename1.commoncol = tablename2.commoncol;
```

If both tables have a column with the same name, use `tablename.columnname` to avoid ambiguity:
```sql
SELECT ename, sal, dept.deptno, dname, loc
FROM emp, dept
WHERE emp.deptno = dept.deptno;
```

**Using alias names:**
```sql
SELECT ename, sal, d.deptno, dname, loc
FROM emp e, dept d
WHERE e.deptno = d.deptno;
```

**Important:** Equi Join returns matching rows only. Departments with no employees will not appear.

**Example queries:**
```sql
-- Employees working in CHICAGO
SELECT ename, loc FROM emp, dept
WHERE emp.deptno = dept.deptno
  AND loc = 'CHICAGO';

-- Dept name and total salary
SELECT dname, SUM(sal) FROM emp e, dept d
WHERE e.deptno = d.deptno
GROUP BY dname;

-- Location-wise count, min, max salary
SELECT loc, COUNT(*), MIN(sal), MAX(sal) FROM emp e, dept d
WHERE e.deptno = d.deptno
GROUP BY loc;
```

### 2. Non-Equi Join

Uses conditions other than equality (<, >, <=, >=, BETWEEN, ...). Useful when tables do not have a common column but one column value falls in another column's range.

Example using SALGRADE:
```sql
SELECT ename, sal, losal, hisal FROM emp, salgrade
WHERE sal BETWEEN losal AND hisal;
```

### 3. Self Join

Joining a table to itself. Compares two rows within the same table. Must create two aliases for the same table.

```sql
-- Employee name next to manager name
SELECT e1.ename "employee", e2.ename "manager"
FROM emp e1, emp e2
WHERE e1.mgr = e2.empno;

-- Employees earning more than their manager
SELECT e1.ename "employee", e2.ename "manager", e1.sal, e2.sal "mgr_sal"
FROM emp e1, emp e2
WHERE e1.mgr = e2.empno AND e1.sal > e2.sal;

-- Employees who joined before their manager
SELECT e1.ename "employee", e1.hiredate, e2.ename "manager"
FROM emp e1, emp e2
WHERE e1.mgr = e2.empno AND e1.hiredate < e2.hiredate;
```

### 4. Outer Join (8i style)

Returns matching rows plus non-matching rows from one table. Uses the `(+)` operator inside the joining condition. `(+)` can be used on only **one side** of the condition.

```sql
SELECT ename, sal, d.deptno, dname, loc
FROM emp e, dept d
WHERE e.deptno(+) = d.deptno;
--       ^                ^
--       matching only    all rows

-- Output includes: 40  OPERATIONS  BOSTON  with NULL ename and sal
-- because dept 40 has no employees.
```

**Full outer join (before 9i):**
```sql
SELECT ename, sal, d.deptno, dname, loc
FROM emp e, dept d
WHERE e.deptno(+) = d.deptno
UNION
SELECT ename, sal, d.deptno, dname, loc
FROM emp e, dept d
WHERE e.deptno = d.deptno(+);
```

### 9i (ANSI) Joins

### Inner Join

Returns matching rows only, like Equi Join, but with better performance.
```sql
SELECT ename, sal, d.deptno, dname, loc
FROM emp e JOIN dept d
ON e.deptno = d.deptno;
```

### USING clause

Alternative to ON, when both tables have an identically named common column. Returns the common column only once. **Cannot** use table alias on the joining column.
```sql
SELECT ename, sal, deptno, dname, loc
FROM emp JOIN dept
USING (deptno);
```

### Left Outer Join

All rows from the left table, plus matching rows from the right. NULLs for non-matching rows on the right.
```sql
SELECT * FROM z1 LEFT OUTER JOIN z2 ON z1.a = z2.a;
```

### Right Outer Join

All rows from the right table, plus matching rows from the left.
```sql
SELECT * FROM z1 RIGHT OUTER JOIN z2 ON z1.a = z2.a;
```

### Full Outer Join

All rows from both tables. NULLs where there is no match on either side.
```sql
SELECT * FROM z1 FULL OUTER JOIN z2 ON z1.a = z2.a;
```

### Natural Join

Automatically joins two tables using all identically-named columns. No join condition needed. Internally uses USING clause, so common columns appear once. Cannot use aliases for joining columns.
```sql
SELECT ename, sal, deptno, dname, loc FROM emp NATURAL JOIN dept;
```

### Cross Join (explicit)

Cartesian product.
```sql
SELECT ename, sal, dname, loc FROM emp CROSS JOIN dept;
```

### Joining more than two tables

**8i syntax:**
```sql
SELECT col1, col2, ... FROM table1, table2, table3
WHERE table1.commoncol = table2.commoncol
  AND table2.commoncol = table3.commoncol;
```

**9i syntax:**
```sql
SELECT col1, col2, ... FROM table1
JOIN table2 ON table1.commoncol = table2.commoncol
JOIN table3 ON table2.commoncol = table3.commoncol;
```

### Possible Exam Questions

- What is a join? How many joining conditions are needed to join n tables?
- List the different types of joins in Oracle.
- Explain Equi Join with syntax and example.
- Differentiate between Equi Join and Non-Equi Join.
- Explain Self Join with an example.
- Explain Outer Join with the (+) operator.
- Write a query to display employees earning more than their manager (self join).
- Write a query to display all departments including those with no employees.
- What is Cross Join? What is Cartesian product?
- Differentiate between Inner Join and Natural Join.
- What is the USING clause? How does it differ from ON?
- Explain Left Outer Join, Right Outer Join, and Full Outer Join.

---

## Chapter 31: Views

### Definition

A **view** is a database object that provides authority-level security. It behaves like a virtual table.

- Views do not store data (except materialized views).
- View is also called a **virtual table** or **window of a table**.
- Views are created from a base table.

Two types:
1. **Simple View** (from one base table)
2. **Complex View / Join View** (from multiple base tables)

### Simple View

Created from only one base table.

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

DML through a simple view to the base table is allowed with two restrictions:

1. If the view has a group function, GROUP BY, ROWNUM, DISTINCT, set operators, or joins, DML is **not allowed**.
2. The view must include the base table's NOT NULL columns for INSERT to work.

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
-- ERROR: cannot insert NULL into empno
-- (empno is NOT NULL in base table but missing from view)
```

### View definition storage

When a view is created, its definition (the SELECT) is permanently stored in the database.

To see it, use the USER_VIEWS data dictionary:
```sql
DESC USER_VIEWS;
SELECT text FROM USER_VIEWS WHERE view_name = 'V1';
```

Note: if a view uses functions or expressions, you must give them aliases:
```sql
CREATE OR REPLACE VIEW v1
AS
SELECT deptno, MAX(sal) FROM emp GROUP BY deptno;
-- ERROR: must name this expression with a column alias

-- Correct:
CREATE OR REPLACE VIEW v1
AS
SELECT deptno, MAX(sal) a FROM emp GROUP BY deptno;
```

### Complex View (Join View)

Created from multiple base tables.
```sql
CREATE OR REPLACE VIEW v5
AS
SELECT ename, sal, dname, loc
FROM emp, dept
WHERE emp.deptno = dept.deptno;
```

Generally, DML through a complex view is **not allowed**:
```sql
UPDATE v5 SET dname = 'xyz' WHERE dname = 'SALES';
-- ERROR: cannot modify a column which maps to a non key-preserved table
```

To check which columns are updatable:
```sql
SELECT column_name, updatable FROM USER_UPDATABLE_COLUMNS
WHERE table_name = 'V5';
-- COLUMN_NAME  UPDATABLE
-- ENAME        YES
-- SAL          YES
-- DNAME        NO
-- LOC          NO
```

To fix this, Oracle 8.0 introduced **INSTEAD OF triggers** (Chapter 33).

### Possible Exam Questions

- What is a view? What are the two types?
- Differentiate between simple view and complex view.
- What are the restrictions for performing DML through a simple view?
- Can DML be performed through a complex view? How can it be enabled?
- Write the syntax to create a view.
- Where are view definitions stored in Oracle?
- Why do views need aliases for functions and expressions?

---

## Chapter 32: Materialized Views

Oracle 8i introduced **Materialized View** (M.View).

- Used in Data Warehousing applications.
- Handled by the DBA.
- Unlike normal views, M.Views **store data**.
- Used to **improve performance** of joined or aggregate queries.
- M.View stores the result of the query. It is a replication of remote database into local node.
- When we refresh the M.View, it syncs data based on the base table.

### Difference between View and Materialized View

| Views | Materialized Views |
|---|---|
| Does not store data | Stores data |
| Security purpose | Performance purpose |
| If base table is dropped, view cannot be accessed | Even if base table is dropped, M.View can be accessed |
| DML operations possible (with restrictions) | DML operations not possible |

### Syntax

```sql
CREATE MATERIALIZED VIEW viewname
AS
SELECT statement;
```

Before creating an M.View, the DBA must grant the privilege:
```sql
GRANT CREATE ANY MATERIALIZED VIEW TO username;
```

**Rule:** One of the base tables must have a primary key. In Oracle 11g and 12c, this restriction is relaxed.

### Types of Materialized Views

### 1. Complete Refresh M.View (default)

Rowids are recreated when refreshing, even if base table data is unchanged. Not efficient for frequent refreshes.

Syntax:
```sql
CREATE MATERIALIZED VIEW viewname
REFRESH COMPLETE
AS
SELECT statement;
```

### 2. Fast Refresh M.View (incremental)

Only applies changes since last refresh. Rowids do not change. Better performance.

Requires a **materialized view log** on the base table first:
```sql
CREATE MATERIALIZED VIEW LOG ON basetablename;

CREATE MATERIALIZED VIEW viewname
REFRESH FAST
AS
SELECT statement;
```

Example:
```sql
CREATE MATERIALIZED VIEW LOG ON base;

CREATE MATERIALIZED VIEW mj2
REFRESH FAST
AS
SELECT * FROM base;

UPDATE base SET name = 'pqr' WHERE sno = 2;
EXEC DBMS_MVIEW.REFRESH('mj2');
```

### Refresh methods

**1. Manually (on demand).** Default. Use DBMS_MVIEW package.
```sql
EXEC DBMS_MVIEW.REFRESH('viewname');
```

**2. Automatically (on commit).** Refresh happens automatically when the base table transaction is committed.

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
SELECT * FROM mj3;    -- not yet updated
COMMIT;
SELECT * FROM mj3;    -- automatically updated
```

### Possible Exam Questions

- What is a materialized view? Why is it used?
- Differentiate between views and materialized views.
- Explain complete refresh and fast refresh materialized views.
- What is a materialized view log? Why is it needed?
- Differentiate between ON DEMAND and ON COMMIT refresh methods.
- Write the syntax to create a fast refresh materialized view.
- What privilege is needed to create a materialized view?

---

## Chapter 33: Triggers

A **trigger** is a stored PL/SQL block, similar to a stored procedure. It is **automatically invoked** when a DML operation is performed on a table.

Two types:
1. **Statement-level triggers**: body executes once per DML statement.
2. **Row-level triggers**: body executes for each row affected. Uses `FOR EACH ROW` clause.

### Trigger syntax

```sql
CREATE OR REPLACE TRIGGER triggername
BEFORE | AFTER  INSERT | DELETE | UPDATE  ON tablename
[FOR EACH ROW]
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
-- 21-DEC-15    <- only ONE entry, even though 3 rows updated
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
-- 21-DEC-15    <- 3 entries because 3 rows updated
```

### :OLD and :NEW qualifiers

In row-level triggers, the body executes per row. DML values are internally stored in two rollback segment qualifiers:

- `:OLD` = column values before the change
- `:NEW` = column values after the change

Use a colon `:` in front of the qualifier name in trigger body.

Syntax:
```
:old.columnname
:new.columnname
```

Availability:

| Event | :OLD | :NEW |
|---|---|---|
| INSERT | Not available | Available |
| UPDATE | Available | Available |
| DELETE | Available | Not available |

### Full example: row-level trigger to back up deleted rows

**Question.** Write a PL/SQL row-level trigger on emp table so that whenever a user deletes data, deleted records are stored in another table.

```sql
-- Step 1: create backup table (structure only)
CREATE TABLE backup AS SELECT * FROM emp WHERE 1 = 2;

-- Step 2: create row-level trigger
CREATE OR REPLACE TRIGGER tk1
AFTER DELETE ON emp
FOR EACH ROW
BEGIN
    INSERT INTO backup VALUES
        (:old.empno, :old.ename, :old.job, :old.mgr,
         :old.hiredate, :old.sal, :old.comm, :old.deptno);
END;
/

-- Test:
DELETE FROM emp WHERE sal > 2000;
-- 10 rows deleted

SELECT * FROM backup;
-- shows the 10 deleted rows
```

### INSTEAD OF trigger

To allow DML on a complex view, Oracle 8.0 introduced INSTEAD OF triggers. By default, INSTEAD OF triggers are row-level and created on views, not tables.

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

-- Now this works:
UPDATE v5 SET dname = 'xyz' WHERE dname = 'SALES';
-- 6 rows updated
```

### Possible Exam Questions

- What is a trigger? When is it invoked?
- Differentiate between statement-level and row-level triggers.
- Write the syntax to create a trigger.
- What are :OLD and :NEW qualifiers? When are they available?
- Write a row-level trigger to back up deleted rows from emp table.
- What is an INSTEAD OF trigger? Why is it used?
- On what object is an INSTEAD OF trigger created?

---

## Chapter 34: Stored Procedures and Functions

A **stored procedure** is a named PL/SQL block, stored in the database, that performs a task and can be called repeatedly.

Syntax:
```sql
CREATE OR REPLACE PROCEDURE proc_name (p1 IN datatype, p2 OUT datatype)
IS
BEGIN
    -- statements
END;
/

-- Calling:
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

### Possible Exam Questions

- What is a stored procedure? Write the syntax.
- What is a stored function? How does it differ from a procedure?
- How do you call a stored procedure?

---

## Chapter 35: QBE and QUEL

### QBE (Query By Example)

A visual/graphical query language. The user fills sample values and conditions into a grid that looks like the table itself. Used in tools like Microsoft Access.

Instead of writing text SQL, the user just enters what they want to match in a template.

### QUEL (QUEry Language)

Developed for the **INGRES** database system as an alternative to SQL. Based on tuple relational calculus. Predates the SQL standard. Uses keywords like `RANGE OF` and `RETRIEVE` instead of SELECT and FROM.

Example (QUEL style):
```
RANGE OF e IS emp
RETRIEVE (e.ename, e.sal)
WHERE e.deptno = 10
```

### Possible Exam Questions

- What is QBE? Where is it used?
- What is QUEL? Which database system used it?
- Differentiate between SQL, QBE, and QUEL.

---

## Chapter 36: DCL and TCL

### DCL: GRANT and REVOKE

Used to control access.

**Creating a user (as SYS/DBA):**
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

### Possible Exam Questions

- What are DCL commands? Give examples.
- Write the syntax to create a new user and grant privileges.
- What are TCL commands? Explain each.
- What is the difference between COMMIT and ROLLBACK?
- What is a SAVEPOINT? How is it used?

---

## Chapter 37: Programmatic SQL

### Embedded SQL

SQL statements written directly inside a host programming language's source code (like C, COBOL, Java). Pre-compiled before the host compiler runs. Static: the SQL structure is fixed at compile time.

Example (in C):
```c
EXEC SQL SELECT ename INTO :name FROM emp WHERE empno = :id;
```

### Dynamic SQL

SQL statements constructed and executed at runtime as strings. Allows flexible queries whose exact structure is not known until run time. In PL/SQL, use `EXECUTE IMMEDIATE`.

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

A standard API that allows application programs to access data in different DBMSs using a common set of function calls, independent of the specific database or programming language. ODBC uses drivers to translate application calls into vendor-specific calls.

### Possible Exam Questions

- What is embedded SQL? Give an example.
- What is dynamic SQL? How is it different from embedded SQL?
- What is ODBC? What is its purpose?
- Differentiate between embedded SQL, dynamic SQL, and ODBC.

---

# Query Practice Bank

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

-- 10. Employees whose name is exactly 5 characters
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

-- 17. Years where more than 1 employee was hired
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

-- 23. All departments including ones with no employees (outer join)
SELECT ename, sal, d.deptno, dname, loc
FROM emp e, dept d
WHERE e.deptno(+) = d.deptno;

-- 24. Same, using ANSI syntax
SELECT ename, sal, d.deptno, dname, loc
FROM emp e RIGHT OUTER JOIN dept d ON e.deptno = d.deptno;

-- 25. Update comm using NVL2
UPDATE emp SET comm = NVL2(comm, comm + 500, 500);

-- 26. Employees with salary between 2000 and 5000
SELECT * FROM emp WHERE sal BETWEEN 2000 AND 5000;

-- 27. Rollup subtotal and grand total
SELECT deptno, job, SUM(sal) FROM emp GROUP BY ROLLUP(deptno, job);

-- 28. Count E in 'SLEEP'
SELECT LENGTH('SLEEP') - LENGTH(REPLACE('SLEEP', 'E')) FROM dual;

-- 29. First date of the current month
SELECT LAST_DAY(ADD_MONTHS(SYSDATE, -1)) + 1 FROM dual;

-- 30. Create a view of dept 10 employees, insert through it
CREATE OR REPLACE VIEW v1 AS SELECT * FROM emp WHERE deptno = 10;
INSERT INTO v1 (empno, ename, deptno) VALUES (1, 'murali', 30);

-- 31. Fast refresh materialized view on commit
CREATE MATERIALIZED VIEW LOG ON base;
CREATE MATERIALIZED VIEW mj3 REFRESH FAST ON COMMIT
AS SELECT * FROM base;

-- 32. Row-level trigger to back up deleted rows
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

-- 33. INSTEAD OF trigger for updating a complex view
CREATE OR REPLACE TRIGGER tv1
INSTEAD OF UPDATE ON v5
FOR EACH ROW
BEGIN
    UPDATE dept SET dname = :new.dname WHERE dname = :old.dname;
    UPDATE dept SET loc = :new.loc WHERE loc = :old.loc;
END;
/
```
