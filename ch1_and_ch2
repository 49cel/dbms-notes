# Database Management Systems — Unit 1 and Unit 2

Reference tables used throughout: `EMP`(empno, ename, job, mgr, hiredate, sal, comm, deptno) and `DEPT`(deptno, dname, loc)

---

# UNIT 1 — Introduction to DBMS

## 1.1 Data, Information, and Data Stores

- **Data** is a collection of raw facts, with no meaning attached yet. Example: student marks, customer names.
- **Information** is data that has been processed into something meaningful. Example: a student's marksheet, a customer's invoice.
- A **data store** is any place where data or information is kept. There are three kinds:
  1. Papers and books
  2. Flat files
  3. Databases

## 1.2 Flat Files and Their Drawbacks

A flat file is the traditional way of storing data permanently in secondary storage. In this approach, **every application program keeps its own separate file**, instead of sharing one central store.

This causes five problems:

1. **Data Retrieval** — there is no query language. To read data back out, you have to write a full application program in a high-level language.
2. **Data Redundancy** — the same piece of data often ends up copied into more than one file. If you update it in one place but not the other, the copies fall out of sync. This is called **inconsistency**.
3. **Data Integrity** — flat files have no built-in rules (like "this field can't be empty" or "this value must exist elsewhere first"). Any such rule has to be coded by hand into every application that touches the file.
4. **Data Security** — flat files have no permission system. Anyone with access to the file can read or change everything in it.
5. **Data Indexing** — flat files can't be indexed, so searching through them is slow. A database can be indexed, which makes searches fast.

Because of these five problems, organizations moved away from flat files toward using a **DBMS**.

## 1.3 DBMS — Definition, Structure, and Advantages

A **DBMS** (Database Management System) is a collection of programs that let you store, manage, and retrieve data efficiently, while also defining the data's structure, letting you manipulate it, and controlling who can access it.

Examples of DBMS software: Oracle, Teradata, SQL Server, MySQL, SQLite, Informix, Sybase.

Here is how a DBMS directly solves each flat-file problem:

| Flat-file problem | How DBMS solves it |
|---|---|
| No query language, manual retrieval | SQL — a simple, non-procedural language to fetch data |
| Redundant, inconsistent data | Normalization organizes data to reduce duplication |
| No integrity rules | Constraints — primary key, foreign key, unique, check, not null |
| No security | Role-based access control |
| No indexing | Built-in indexing for fast searches |
| No guaranteed consistency | Transactions with ACID properties |

**ACID properties** — every transaction in a DBMS automatically has these four properties, which is what keeps the data consistent without extra effort from the programmer:

- **Atomicity** — a transaction either completes fully, or not at all. There's no "half-done" state.
- **Consistency** — a transaction moves the database from one valid state to another valid state.
- **Isolation** — transactions running at the same time don't interfere with each other.
- **Durability** — once a transaction is committed, it stays saved even if the system crashes right after.

**Main components of a DBMS**: query processor, DDL/DML compiler, storage manager, transaction manager, buffer manager, data dictionary (the system's own catalog of what tables/columns exist), the database itself, and a user interface for logging in and issuing commands.

**Multi-user DBMS architecture**: a DBMS is built to let many users and application programs connect and work with the same database at the same time. It manages this using concurrency control and locking, so two users can't corrupt each other's changes. A plain file system has no such mechanism.

## 1.4 Data Abstraction and Database Languages

**Data abstraction** means hiding the complicated storage details from the user and only showing them what they need to see. This idea is what the 3-level architecture below is built around.

**Database languages** — covered in full in Unit 2:
- DDL (Data Definition Language)
- DML (Data Manipulation Language)
- DQL (Data Query Language)
- DCL (Data Control Language)
- TCL (Transaction Control Language)

## 1.5 DBMS Architecture — ANSI/SPARC 3-Level Architecture

ANSI (American National Standards Institute) defined a standard 3-level architecture for how a DBMS should be organized, also called **ANSI/SPARC** architecture.

The whole point of this architecture is to **separate what the user sees from how the data is actually stored on disk**.

```
       View 1      View 2      View 3          <- External Level
             \        |        /
                Conceptual Level
                       |
                 Internal Level
                       |
                    Database
```

1. **External Level (top)** — this is what an individual user sees. It provides security: through **views**, a user only sees the part of the data relevant to them. Example: a Customer's view might show {item name, price}, while a Purchase Manager's view shows {item name, price, quantity}.

2. **Conceptual Level (middle)** — this describes the logical structure of the *entire* database, for all users combined. It defines the data types, sizes, and constraints (like primary and foreign keys), and the relationships between tables. It does **not** say anything about how the data is physically stored.

3. **Internal Level (bottom)** — this describes how the data is physically stored on disk, including indexes and clusters. Only the **DBA** (Database Administrator) works at this level.

## 1.6 Data Independence

Data independence means that changing something at a lower level does not force you to change anything at a higher level.

- **Logical Data Independence**: if you change the conceptual level (for example, adding a new table), the external level (the views users already rely on) does not need to change.
- **Physical Data Independence**: if you change the internal level (for example, adding a new index), the conceptual level does not need to change. Performance might improve or degrade, but the logical structure stays the same.

## 1.7 Data Models — A Short History

A **data model** is the way data is represented at the conceptual level. There have been three major models historically.

**(a) Hierarchical Data Model (1960s)**
Data is arranged like a tree. Each parent can have many children (a 1-to-many relationship), but each child has only one parent. Because of this, child data often gets repeated, causing more duplication. IBM introduced a product called **IMS** based on this model. It required a **procedural language** to operate — meaning you had to describe step-by-step how to retrieve data, not just what to retrieve.

**(b) Network Data Model (1970s)**
Introduced by the **CODASYL** committee. Here the parent-child relationship can be **many-to-many**, so there is less duplication than in the hierarchical model. IBM's **IDMS** product was based on this model. It also required a procedural language.

**(c) Relational Data Model (1970)**
Introduced by **E.F. Codd** in a paper titled *"A Relational Model of Data for Large Shared Data Banks"*. Data is represented as a collection of relations — simple two-dimensional tables of rows and columns. This model has three parts: (1) a collection of objects such as tables, views, indexes, and synonyms, (2) a set of operators to work on them, and (3) a set of integrity rules. Codd's work eventually led to the idea of Normal Forms (1NF, 2NF, 3NF), which are about organizing tables to reduce redundancy.

The line of products that came from this idea: IBM's System/R → INGRES → Oracle (1977) → Oracle Corporation (1982). Larry Ellison is Oracle's CEO.

A short Oracle timeline, in case it comes up: 1977, Larry Ellison, Bob Miner, and Ed Oates start a company called SDL; 1979, renamed to RSI, and Oracle 2.0 becomes the first public release; 1982, renamed to Oracle Corporation; 1988, Oracle 6.0 introduces PL/SQL; 1992, Oracle 7.0 introduces VARCHAR2; 1999, Oracle 8i; 2001, Oracle 9i introduces the MERGE statement; 2003, Oracle 10g introduces the recycle bin; 2013, Oracle 12c (the "c" stands for cloud).

## 1.8 Entities, Attributes, Relationships, Constraints, and Keys

- **Entity**: a real-world object or thing that data is stored about, such as a Student or an Employee.
- **Attribute**: a property that describes an entity, such as a name or an age. Attributes can be simple or composite, single-valued or multi-valued, and stored or derived.
- **Relationship**: an association between two or more entities, such as "works in" connecting Employee to Department. Relationships can be one-to-one, one-to-many, or many-to-many.
- **Constraints**: rules that restrict what values are valid, such as NOT NULL, UNIQUE, CHECK, PRIMARY KEY, and FOREIGN KEY.
- **Candidate Key**: the smallest possible set of attributes that can uniquely identify a row.
- **Primary Key**: the candidate key that is actually chosen to identify rows; it can never be NULL.
- **Alternate Key**: any candidate key that was not chosen as the primary key.
- **Super Key**: any set of attributes that can uniquely identify a row, even if it includes extra, unnecessary attributes.
- **Foreign Key**: an attribute in one table that refers to the primary key of another table. This is what enforces referential integrity.
- **Composite Key**: a primary key that is made up of two or more attributes together.

## 1.9 ER Diagrams and Converting Them to Tables

**Standard ER diagram symbols:**
- Rectangle = entity
- Ellipse = attribute (a double ellipse means multi-valued, a dashed ellipse means derived)
- Diamond = relationship
- Double rectangle = weak entity
- Underline = key attribute

**How to convert an ER diagram into tables:**
1. Each strong entity becomes its own table; its key attribute becomes the primary key.
2. Each weak entity becomes its own table, but you must also add the primary key of its owning (strong) entity as a foreign key, combined with the weak entity's own partial key, to form a composite primary key.
3. A 1-to-1 relationship: add the primary key of either entity as a foreign key in the other entity's table.
4. A 1-to-many relationship: add the primary key of the "one" side as a foreign key in the table on the "many" side.
5. A many-to-many relationship: create a brand-new table that holds the primary keys of both entities together (as a composite key), plus any attributes that belong to the relationship itself.
6. A multi-valued attribute: create a separate table containing that attribute's value along with the primary key of the entity it belongs to.

## 1.10 The EER Model and Converting It to Tables

The EER (Enhanced/Extended ER) model adds a few extra ideas on top of the basic ER model:

- **Generalization**: combining several lower-level entities that share common features into one higher-level entity. This works bottom-up.
- **Specialization**: splitting a higher-level entity into more specific lower-level sub-entities. This works top-down.
- **Aggregation**: treating an entire relationship as if it were itself a higher-level entity.
- **Inheritance**: sub-entities automatically inherit the attributes of their parent (super) entity.

**Converting EER diagrams to tables**: create one table for the superclass, holding all the shared attributes plus the primary key. Then create a separate table for each subclass, holding only that subclass's own attributes, plus the superclass's primary key (which also acts as a foreign key linking back to the superclass table).

## 1.11 Relational Model — Basic Concepts

- A **relation** is simply a table.
- A **tuple** is a row in that table.
- An **attribute** is a column.
- A **domain** is the set of all legal values that an attribute is allowed to take. For example, the domain of "age" might be all positive whole numbers.
- The **degree** of a relation is how many attributes (columns) it has.
- The **cardinality** of a relation is how many tuples (rows) it has.

## 1.12 Codd's Rules

E.F. Codd proposed 12 rules that define what makes a database system genuinely "relational." Briefly:

1. All data must be represented as values inside tables.
2. Every value must be reachable using the table name, the primary key, and the column name together.
3. NULL values must be handled consistently, and treated as different from zero or an empty string.
4. The database's own description (its catalog) must be stored as tables too, so it can be queried the same way as regular data.
5. There should be one comprehensive language to define, manipulate, and control the data.
6. Any view that is theoretically updatable must actually be updatable by the system.
7. Insert, update, and delete operations should work on whole sets of rows at once, not one row at a time.
8. Physical storage changes shouldn't affect applications.
9. Logical schema changes shouldn't affect applications, as far as possible.
10. Integrity rules should be stored in the catalog, not hard-coded into application programs.
11. How data is distributed across locations should be invisible to the user.
12. Low-level, record-at-a-time access must not be able to bypass the integrity rules enforced at the higher level.

## 1.13 Relational Integrity

- **Null**: represents an undefined, unknown, or missing value. It is **not** the same thing as zero or a blank string. Any arithmetic involving a null becomes null — for example, `null + 50 = null`.
- **Entity Integrity**: the primary key of a table must always be unique and must never be null. This is what guarantees every row can be told apart from every other row.
- **Referential Integrity**: a foreign key value must either match a primary key value that actually exists in the referenced table, or be null. This prevents "orphan" rows that point to something that doesn't exist.
- **Enterprise Constraints**: extra business rules specific to an organization, beyond entity and referential integrity. For example, "a manager's salary must be above 10000."

Finally, a **view** provides a layer of security by only exposing part of the data (details are in Unit 2, section 2.14), and a **schema diagram** is simply a picture showing all the tables, their columns, and how they're linked together through primary and foreign keys.

---

# UNIT 2 — Relational Query Languages

## 2.1 Relational Algebra

Relational algebra is a **procedural** query language: you describe a sequence of operations, and each operation on a relation (table) produces a new relation.

| Operation | Symbol | Meaning |
|---|---|---|
| Select | σ | picks rows matching a condition |
| Project | π | picks specific columns |
| Union | ∪ | combines rows from two compatible relations, removing duplicates |
| Set Difference | − | rows that are in the first relation but not the second |
| Intersection | ∩ | rows common to both relations |
| Cartesian Product | × | pairs every row of one relation with every row of the other |
| Rename | ρ | renames a relation or an attribute |
| Join | ⋈ | combines related rows from two relations based on a condition |
| Division | ÷ | used for "for all" style queries |

## 2.2 Relational Calculus

Relational calculus is a **non-procedural** (declarative) language: you describe *what* result you want, not the steps to get there.

- **Tuple Relational Calculus**: written as `{ t | P(t) }`, meaning the set of all tuples `t` for which the condition `P(t)` is true.
- **Domain Relational Calculus**: written as `{ <x1, x2, ..., xn> | P(x1, x2, ..., xn) }`, meaning the set of value combinations that satisfy the condition `P`.

## 2.3 Introduction to SQL

**SQL** stands for Structured Query Language. It is a non-procedural language used to work with relational databases.

**Characteristics**: non-procedural, set-based rather than row-by-row, reads like plain English, works across almost every relational database vendor, and can be used both interactively and inside programs.

**Advantages**: easy to learn, standardized across vendors, retrieves large amounts of data quickly, and needs far less code than a procedural language would.

**A short history**: in 1970, E.F. Codd introduced a language called DSL/Alpha. IBM's System/R team then built a simplified version called "Square," which was renamed **SEQUEL** (Structured English Query Language), and finally shortened to **SQL**. The language was standardized by **ANSI in 1986** and **ISO in 1987**, with later revisions known as SQL89, SQL92, SQL99, and SQL2003.

## 2.4 The Sub-Languages of SQL

| Category | Full form | Commands |
|---|---|---|
| DDL | Data Definition Language | CREATE, ALTER, DROP, TRUNCATE, RENAME |
| DML | Data Manipulation Language | INSERT, UPDATE, DELETE, MERGE |
| DQL | Data Query Language | SELECT |
| TCL | Transaction Control Language | COMMIT, ROLLBACK, SAVEPOINT |
| DCL | Data Control Language | GRANT, REVOKE |

Note: in every database, **DDL commands are automatically committed** — you don't need to run `COMMIT` after them, and you can't roll them back.

## 2.5 SQL Data Types

- `NUMBER(P,S)` — stores fixed or floating point numbers. `P` is the precision (total number of digits allowed), and `S` is the scale (digits allowed after the decimal point). The maximum digits allowed before the decimal point is `P − S`. If you insert more decimal digits than `S` allows, Oracle rounds automatically instead of raising an error. Maximum precision is 38 digits.
- `NUMBER(P)` — stores only whole numbers, with `P` total digits.
- `CHAR(size)` — fixed-length text, up to 2000 bytes. If the value is shorter than the declared size, Oracle pads the rest with blank spaces (this is called blank padding).
- `VARCHAR2(size)` — variable-length text, up to 4000 bytes, introduced in Oracle 7.0. Unlike CHAR, it does not pad with blanks, so it saves space.
- `VARCHAR(size)` — an older variable-length text type, up to 2000 bytes, used before Oracle 7.0 introduced VARCHAR2.
- `DATE` — stores both a date and a time. The default display format is `DD-MON-YY`.

## 2.6 DDL Commands

**CREATE** — defines a new database object.
```sql
CREATE TABLE tablename (columnname1 datatype(size), columnname2 datatype(size), ...);

CREATE TABLE first (sno NUMBER(10), name VARCHAR2(10));
DESC first;   -- shows the table's structure
```

**ALTER** — changes the structure of an existing table. It has three forms: add, modify, drop.
```sql
-- add a new column
ALTER TABLE tablename ADD (col1 datatype(size), col2 datatype(size));
ALTER TABLE first ADD sal NUMBER(10);

-- modify a column's datatype or size (this is the only thing MODIFY can change)
ALTER TABLE tablename MODIFY (col1 datatype(size));
ALTER TABLE first MODIFY sno DATE;

-- drop a column
ALTER TABLE tablename DROP COLUMN columnname;
ALTER TABLE tablename DROP (col1, col2);
```
You can never drop every column of a table — Oracle will raise the error "cannot drop all columns in a table."

**DROP** — removes a database object entirely.
```sql
DROP TABLE tablename;
DROP VIEW viewname;
```
Before Oracle 10g, dropping a table removed it permanently right away. From 10g onward, a dropped table first goes into the **Recycle Bin**, a read-only system table that keeps track of dropped objects.
```sql
DESC recyclebin;
SELECT original_name FROM recyclebin;

FLASHBACK TABLE tablename TO BEFORE DROP;   -- restores it from the recycle bin
DROP TABLE tablename PURGE;                 -- deletes permanently, skipping the recycle bin
PURGE TABLE tablename;                       -- removes one table that's already in the recycle bin
PURGE RECYCLEBIN;                            -- empties the whole recycle bin
```

**TRUNCATE** — deletes every row in a table permanently, but keeps the table's structure.
```sql
TRUNCATE TABLE tablename;
```

**RENAME** — renames a table, or (from Oracle 9i) a column.
```sql
RENAME oldtablename TO newtablename;
ALTER TABLE tablename RENAME COLUMN oldcolumnname TO newcolumnname;
```

**DELETE versus TRUNCATE** — a classic comparison question:

| | DELETE | TRUNCATE |
|---|---|---|
| Type | DML command | DDL command |
| Recovery | Rows are held in a buffer temporarily, so `ROLLBACK` can bring them back | Rows are permanently gone; `ROLLBACK` cannot recover them, because DDL auto-commits |
| Scope | Can remove specific rows using WHERE | Only removes all rows at once |

## 2.7 DML Commands

**INSERT** has three common methods:
```sql
-- method 1: give the values directly
INSERT INTO tablename VALUES (value1, value2);
INSERT INTO first VALUES (1, 'murali');

-- method 2: substitution operator (&), prompts for a value each time it runs
INSERT INTO tablename VALUES (&columnname1, '&columnname2');
INSERT INTO first VALUES (&sno, '&name');
-- typing "/" re-runs the last command and asks again

-- method 3: insert into specific columns only, leaving the rest NULL
INSERT INTO tablename (col1, col2) VALUES (val1, val2);
INSERT INTO first (name) VALUES ('zzz');
```

**UPDATE** — changes existing data.
```sql
UPDATE tablename SET columnname = newvalue WHERE condition;
UPDATE emp SET sal = 1000 WHERE ename = 'SMITH';
UPDATE first SET address = NULL WHERE name = 'sud';   -- clears a value
```

**DELETE**
```sql
DELETE FROM tablename;                  -- removes every row
DELETE FROM tablename WHERE condition;   -- removes only matching rows
ROLLBACK;                                -- can bring deleted rows back, before commit
```

## 2.8 DQL — the SELECT Statement

The full syntax, in the order the clauses must appear:
```sql
SELECT col1, col2
FROM tablename
WHERE condition
GROUP BY columnname
HAVING condition
ORDER BY columnname [ASC | DESC];
```

There are four basic forms a SELECT can take: all columns and all rows, all columns and specific rows, specific columns and all rows, or specific columns and specific rows.

You can also copy a table's structure and data into a brand-new table:
```sql
CREATE TABLE newtable AS SELECT * FROM existingtable;
-- note: constraints such as primary key and foreign key are never copied

-- to copy only the structure, with no rows, use a condition that is always false:
CREATE TABLE newtable AS SELECT * FROM existingtable WHERE 1=2;
```

## 2.9 Operators

**Arithmetic operators**: `+  −  *  /`
```sql
SELECT ename, sal, sal*12 annsal FROM emp;   -- annsal is a column alias
```

**Relational operators**: `=  <  >  >=  <=  <>`
```sql
SELECT * FROM emp WHERE job <> 'CLERK';
```

**Logical operators**: `AND`, `OR`, `NOT`. `AND` narrows the results down to rows matching every condition. `OR` widens the results to rows matching any one of the conditions.
```sql
SELECT * FROM emp WHERE job='CLERK' AND sal>2000;
SELECT * FROM emp WHERE job='CLERK' OR sal>2000;
```

**Special operators**:

| Operator | Opposite | What it does |
|---|---|---|
| `IN` | `NOT IN` | checks against a list of values, instead of writing several `OR`s |
| `BETWEEN` | `NOT BETWEEN` | checks a value falls inside a range (inclusive) |
| `IS NULL` | `IS NOT NULL` | checks whether a column has no value |
| `LIKE` | `NOT LIKE` | matches a text pattern using wildcards |

```sql
SELECT * FROM emp WHERE deptno IN (20,30,50,70,80);
SELECT * FROM emp WHERE sal BETWEEN 2000 AND 5000;
SELECT * FROM emp WHERE comm IS NULL;
```
One thing to remember: `NOT IN` returns no rows at all if the list you give it contains a null value.

**LIKE wildcards**: `%` stands for any string of any length, including an empty one; `_` stands for exactly one character.
```sql
SELECT * FROM emp WHERE ename LIKE 'M%';        -- starts with M
SELECT * FROM emp WHERE ename LIKE '%M%';        -- contains M anywhere
SELECT * FROM emp WHERE ename LIKE '_L%';        -- second letter is L
SELECT * FROM emp WHERE hiredate LIKE '%DEC%';   -- hired in December
```
If the actual data contains a literal `%` or `_` character, use `ESCAPE` so it's treated as a normal character instead of a wildcard:
```sql
SELECT * FROM emp WHERE ename LIKE 'S?_%' ESCAPE '?';
```

**Concatenation operator `||`** joins column values with literal text.
```sql
SELECT 'my employee names are: ' || ename FROM emp;
```

**Handling nulls — NVL and NVL2**:
```sql
-- NVL(exp1, exp2): if exp1 is null, return exp2; otherwise return exp1
SELECT NVL(comm, 0) FROM dual;
SELECT ename, sal, sal + NVL(comm,0) FROM emp;

-- NVL2(exp1, exp2, exp3): if exp1 is NOT null, return exp2; if it IS null, return exp3
UPDATE emp SET comm = NVL2(comm, comm+500, 500);
```
`DUAL` is a built-in table with just one row and one column, used to test what a function returns without needing a real table: `SELECT ABS(-50) FROM dual;`

## 2.10 Functions

Functions carry out a specific task and always return a value. Oracle has predefined functions (grouped below by category) and lets you write your own user-defined functions too.

**Number functions**

| Function | What it does | Example |
|---|---|---|
| `ABS(n)` | turns a negative number positive | `ABS(-50)` gives 50 |
| `ROUND(n,d)` | rounds to d decimal places | `ROUND(45.926,2)` gives 45.93 |
| `TRUNC(n,d)` | cuts off after d decimal places, without rounding | `TRUNC(45.926,2)` gives 45.92 |
| `MOD(m,n)` | remainder after dividing m by n | `MOD(10,3)` gives 1 |
| `POWER(m,n)` | m raised to the power n | `POWER(2,3)` gives 8 |
| `SQRT(n)` | square root | `SQRT(16)` gives 4 |

**Character functions**

| Function | What it does | Example |
|---|---|---|
| `LENGTH(s)` | number of characters | `LENGTH('SMITH')` gives 5 |
| `UPPER(s)` / `LOWER(s)` | changes case | `UPPER('abc')` gives ABC |
| `INITCAP(s)` | capitalizes the first letter of each word | `INITCAP('smith')` gives Smith |
| `SUBSTR(s,pos,len)` | pulls out part of a string | `SUBSTR(ename,2,2)` |
| `INSTR(s,'char')` | finds the position of a character or string inside another string | `INSTR('ABC*D','*')` gives 4 |
| `LPAD(s,len,'char')` | pads the string on the left until it reaches len | `LPAD('ABCD',10,'#')` gives `######ABCD` |
| `RPAD(s,len,'char')` | pads the string on the right | `RPAD('ABCD',10,'#')` gives `ABCD######` |
| `LTRIM(s,'set')` | removes characters from the left | `LTRIM('SSMISS','S')` gives MISS |
| `RTRIM(s,'set')` | removes characters from the right | `RTRIM('SSMISS','S')` gives SSMI |
| `TRIM('c' FROM 's')` | removes characters from both ends | `TRIM('S' FROM 'STHS')` gives TH |
| `TRANSLATE(s1,s2,s3)` | replaces individual characters, one by one | `TRANSLATE('india','in','xy')` gives xydxa |
| `REPLACE(s1,s2,s3)` | replaces one whole substring with another | `REPLACE('india','in','xy')` gives xydia |
| `CONCAT(s1,s2)` | joins two strings together | `CONCAT('wel','come')` gives welcome |

A couple of things worth remembering: if `REPLACE()` is only given two arguments, the second string is simply deleted from the first, rather than replaced with anything. And `INSTR()` always returns a number (a position), counting characters from the left, even when you tell it to start searching from the right using a negative position.

**Date functions**

Oracle's default date display format is `DD-MON-YY`.

| Function | What it does |
|---|---|
| `SYSDATE` | the current system date |
| `ADD_MONTHS(date,n)` | adds n months (or subtracts, if n is negative) |
| `LAST_DAY(date)` | the last date of that month |
| `NEXT_DAY(date,'day')` | the next date that falls on the named weekday |
| `MONTHS_BETWEEN(d1,d2)` | number of months between two dates (d1 should be later than d2, or the result is negative) |
| `ROUND(date,'fmt')` | rounds a date to the nearest year, month, or day |
| `TRUNC(date,'fmt')` | truncates a date, and never rounds upward |

Date arithmetic rules: `date + number` works, `date − number` works, `date1 + date2` is not allowed, and `date1 − date2` works and gives you the number of days between them.
```sql
SELECT SYSDATE + 1 FROM dual;
SELECT LAST_DAY(ADD_MONTHS(SYSDATE,-1)) + 1 FROM dual;   -- the first day of the current month
```
When rounding a date, if the time portion is 12 noon or later, Oracle rounds up to the next day and resets the time to midnight; otherwise it rounds down to the same day at midnight. `TRUNC()` never adds a day — it simply resets the time portion to midnight, no matter what the original time was.

**Date conversion functions**

`TO_CHAR(date, 'format')` turns a date into a text string in whatever format you specify. It is case-sensitive: `'DAY'` gives WEDNESDAY, while `'day'` gives wednesday.
```sql
SELECT TO_CHAR(sysdate,'DD/MM/YY') FROM dual;
SELECT TO_CHAR(sysdate,'HH24:MI:SS') FROM dual;
SELECT * FROM emp WHERE TO_CHAR(hiredate,'MON')='DEC';
SELECT * FROM emp WHERE TO_CHAR(hiredate,'YYYY')='1981';
```
Useful format codes: `D DD DDD` for day of week/month/year, `MM MON MONTH` for month, `YY YYYY YEAR` for year, `HH HH24 MI SS` for time.

`TO_DATE('string', 'format')` does the reverse — it turns a text string into an Oracle date. If the string's format doesn't match the default date format and you don't supply an explicit format string, Oracle raises an error.
```sql
SELECT TO_DATE('12/06/05','DD/MM/YY') FROM dual;
```

**Group (aggregate) functions** — these work across many rows in a column and return one single value.

| Function | What it does |
|---|---|
| `MAX(col)` | the largest value |
| `MIN(col)` | the smallest value |
| `AVG(col)` | the average (nulls are ignored) |
| `SUM(col)` | the total |
| `COUNT(*)` | how many rows exist, including ones with nulls |
| `COUNT(col)` | how many non-null values exist in that column |

```sql
SELECT MAX(sal) FROM emp;
SELECT AVG(NVL(comm,0)) FROM emp;       -- treats null commissions as 0
SELECT COUNT(DISTINCT deptno) FROM emp;
```
An important rule: **group functions cannot be used inside a WHERE clause.** They can only appear in the SELECT list or in a HAVING clause. Writing `WHERE sal = MAX(sal)` will always raise an error.

## 2.11 GROUP BY, HAVING, ORDER BY, ROLLUP, and CUBE

**GROUP BY** arranges rows into groups that share the same value in a column, so a group function can be applied to each group separately.
```sql
SELECT deptno, COUNT(*) FROM emp GROUP BY deptno;
SELECT deptno, MIN(sal), MAX(sal) FROM emp GROUP BY deptno;
```
A key rule: every column you select that is *not* wrapped in a group function must also be listed in the GROUP BY clause, otherwise Oracle raises the error "not a GROUP BY expression."

**HAVING** filters *groups*, the same way WHERE filters individual rows. It runs after GROUP BY, and — unlike WHERE — it is allowed to use group functions.
```sql
SELECT deptno, COUNT(*) FROM emp GROUP BY deptno HAVING COUNT(*) > 3;
SELECT deptno, SUM(sal) FROM emp GROUP BY deptno HAVING SUM(sal) > 9000;
```

**ORDER BY** sorts the final result, ascending (`ASC`, the default) or descending (`DESC`).
```sql
SELECT * FROM emp ORDER BY ename ASC;
SELECT deptno, sal FROM emp ORDER BY deptno, sal DESC;   -- sorts by deptno first, then by sal within each deptno
```

**ROLLUP and CUBE** (introduced in Oracle 8i) are used together with GROUP BY to automatically add subtotal and grand-total rows.
- `ROLLUP(col1, col2)` calculates subtotals working down a hierarchy of columns.
- `CUBE(col1, col2)` calculates subtotals for every possible combination of the given columns.
```sql
SELECT deptno, job, SUM(sal) FROM emp GROUP BY ROLLUP(deptno, job);
SELECT deptno, job, SUM(sal), COUNT(*) FROM emp GROUP BY CUBE(deptno, job);
```

## 2.12 Nested Queries (Sub-queries)

A sub-query is a query written inside another query. The inner query runs first, and its result is used by the outer query.

A **single-row sub-query** returns exactly one value, so it can be compared using `=`, `>`, `<`, and so on:
```sql
SELECT * FROM emp WHERE sal > (SELECT sal FROM emp WHERE ename='JONES');
```
A **multi-row sub-query** returns several values, so it needs `IN`, `ANY`, or `ALL`:
```sql
SELECT * FROM emp WHERE deptno IN (SELECT deptno FROM dept WHERE loc='CHICAGO');
SELECT * FROM emp WHERE sal > ANY (SELECT sal FROM emp WHERE deptno=30);  -- greater than the smallest value in the list
SELECT * FROM emp WHERE sal > ALL (SELECT sal FROM emp WHERE deptno=30); -- greater than the largest value in the list
```
A **correlated sub-query** refers back to a column from the outer query, so it effectively runs once for every row the outer query looks at:
```sql
SELECT ename, sal, deptno FROM emp e
WHERE sal > (SELECT AVG(sal) FROM emp WHERE deptno = e.deptno);
```
`EXISTS` simply checks whether a sub-query returns any rows at all:
```sql
SELECT * FROM dept d WHERE EXISTS (SELECT 1 FROM emp e WHERE e.deptno=d.deptno);
```

## 2.13 Joins

A join retrieves data from more than one table at once. Oracle supports two styles of writing joins: the older **Oracle-style** syntax, and the newer **ANSI (9i)** syntax.

If you list two tables in the FROM clause without giving any join condition at all, Oracle silently performs a **cross join**, pairing every row of one table with every row of the other.
```sql
SELECT ename, sal, dname, loc FROM emp, dept;   -- cross join
```

### Oracle-style joins

**Equi join (also called inner join)** — matches rows based on equal values in a column that both tables share.
```sql
SELECT e.ename, e.sal, d.deptno, d.dname, d.loc
FROM emp e, dept d
WHERE e.deptno = d.deptno;

-- employees working in Chicago
SELECT ename, loc FROM emp, dept
WHERE emp.deptno = dept.deptno AND loc = 'CHICAGO';
```
An equi join only returns matching rows — a department with no employees simply won't appear in the results.

**Non-equi join** — uses a condition other than equality, such as `<`, `>`, or `BETWEEN`.
```sql
SELECT ename, sal, losal, hisal FROM emp, salgrade
WHERE sal BETWEEN losal AND hisal;
```

**Self join** — a table joined to itself, comparing two different rows within the same table. This requires creating two aliases for the same table.
```sql
-- each employee's name next to their manager's name
SELECT e1.ename AS employee, e2.ename AS manager
FROM emp e1, emp e2
WHERE e1.mgr = e2.empno;

-- employees who earn more than their own manager
SELECT e1.ename, e2.ename AS manager FROM emp e1, emp e2
WHERE e1.mgr = e2.empno AND e1.sal > e2.sal;
```

**Outer join** — returns all rows from one table, plus only the matching rows from the other. The `(+)` symbol marks which side is allowed to have missing matches, and it can only be used on one side of the condition at a time.
```sql
SELECT ename, sal, d.deptno, dname, loc
FROM emp e, dept d
WHERE e.deptno(+) = d.deptno;   -- keeps departments even if they have no employees
```

### ANSI (9i) joins

**Inner join** — the same result as an equi join, but generally faster, and uses an `ON` clause.
```sql
SELECT ename, sal, d.deptno, dname, loc
FROM emp e JOIN dept d ON e.deptno = d.deptno;
```
**USING clause** — an alternative to `ON`, usable only when the column name is identical in both tables; it returns that shared column just once.
```sql
SELECT ename, sal, deptno, dname, loc
FROM emp JOIN dept USING (deptno);
```
**Left outer join** — every row from the left table, plus matching rows from the right (missing matches show as null).
```sql
SELECT * FROM emp e LEFT OUTER JOIN dept d ON e.deptno = d.deptno;
```
**Right outer join** — every row from the right table, plus matching rows from the left.
```sql
SELECT * FROM emp e RIGHT OUTER JOIN dept d ON e.deptno = d.deptno;
```
**Full outer join** — every row from both tables, whether or not they match.
```sql
SELECT * FROM emp e FULL OUTER JOIN dept d ON e.deptno = d.deptno;
```
**Natural join** — automatically joins two tables based on every identically-named column they share, without you writing a join condition. It behaves like the USING clause internally.
```sql
SELECT ename, sal, deptno, dname, loc FROM emp NATURAL JOIN dept;
```
**Cross join** (written explicitly) — every row of one table paired with every row of the other.
```sql
SELECT ename, sal, dname, loc FROM emp CROSS JOIN dept;
```

**Joining three or more tables**
```sql
SELECT col1, col2 FROM table1
JOIN table2 ON table1.commoncol = table2.commoncol
JOIN table3 ON table2.commoncol = table3.commoncol;
```

## 2.14 Views and Materialized Views

A **view** is a database object that gives a layer of security. It behaves like a virtual table built from a `SELECT` statement on one or more base tables, but it does not actually store any data of its own.

- A **simple view** is built from a single base table.
- A **complex (join) view** is built from more than one base table, or uses a group function, GROUP BY, DISTINCT, ROWNUM, joins, or set operators.

```sql
CREATE OR REPLACE VIEW v1 AS SELECT * FROM emp WHERE deptno = 10;
SELECT * FROM v1;
```

You can perform DML (insert/update/delete) through a **simple view**, but only if two conditions hold: the view's query does not contain a group function, GROUP BY, ROWNUM, DISTINCT, a join, or a set operator; and if a base table column is NOT NULL, that column must be included in the view for an INSERT to work.

DML through a **complex or join view** is generally blocked:
```sql
UPDATE v5 SET dname = 'xyz' WHERE dname = 'SALES';   -- error: cannot modify a join column
```
You can check exactly which columns of a complex view are updatable:
```sql
SELECT column_name, updatable FROM user_updatable_columns WHERE table_name = 'V5';
```
An **INSTEAD OF trigger** can be created on a complex view to intercept a DML statement and redirect it manually to the correct base table (see section 2.15).

**Materialized views**, introduced in Oracle 8i, are mainly used in data warehousing. Unlike a normal view, a materialized view **does store a physical copy of the query's result**, which makes it much faster for expensive joined or aggregated queries.

| | View | Materialized view |
|---|---|---|
| Stores data? | No | Yes |
| Main purpose | Security | Performance |
| If base table is dropped | Becomes inaccessible | Stays accessible |
| DML allowed? | Yes, with restrictions | No |

```sql
GRANT CREATE ANY MATERIALIZED VIEW TO username;
CREATE MATERIALIZED VIEW viewname AS SELECT_statement;
EXEC dbms_mview.refresh('viewname');
```

There are two kinds of refresh:
- **Complete refresh** (the default) rebuilds the whole materialized view from scratch every time, so its row identifiers change, and it can be slow if refreshed often.
- **Fast refresh** (also called incremental refresh) only applies the changes since the last refresh, so it performs much better. It requires a materialized view log to be created on the base table first.
```sql
CREATE MATERIALIZED VIEW LOG ON basetable;
CREATE MATERIALIZED VIEW mv1 REFRESH FAST AS SELECT * FROM basetable;
```
There are also two refresh methods:
- **On demand** (the default): you must call `dbms_mview.refresh()` manually.
- **On commit**: the materialized view refreshes automatically whenever the base table's transaction is committed.
```sql
CREATE MATERIALIZED VIEW mv2 REFRESH FAST ON COMMIT AS SELECT * FROM basetable;
```

## 2.15 Triggers

A trigger is a stored block of PL/SQL code that runs automatically whenever a specified DML event (INSERT, UPDATE, or DELETE) happens on a table.

- A **statement-level trigger** runs its body exactly once per DML statement, no matter how many rows the statement affects.
- A **row-level trigger** runs its body once for every single row affected, and requires the phrase `FOR EACH ROW` in its definition.

```sql
CREATE OR REPLACE TRIGGER triggername
BEFORE/AFTER INSERT/DELETE/UPDATE ON tablename
[FOR EACH ROW]
BEGIN
   -- trigger body goes here
END;
/
```

Inside a row-level trigger, `:OLD` and `:NEW` refer to the row's values before and after the change:

| Event | :OLD available? | :NEW available? |
|---|---|---|
| INSERT | No | Yes |
| UPDATE | Yes | Yes |
| DELETE | Yes | No |

```sql
-- automatically backs up any row that gets deleted from emp
CREATE TABLE backup AS SELECT * FROM emp WHERE 1=2;

CREATE OR REPLACE TRIGGER tk1
AFTER DELETE ON emp
FOR EACH ROW
BEGIN
   INSERT INTO backup VALUES (:old.empno, :old.ename, :old.job, :old.mgr,
                               :old.hiredate, :old.sal, :old.comm, :old.deptno);
END;
/
```

An **INSTEAD OF trigger** is a special row-level trigger created on a view (not a table). It intercepts a DML statement aimed at a complex view and manually applies the equivalent change to the correct base table, which is otherwise not possible directly.
```sql
CREATE OR REPLACE TRIGGER tv1
INSTEAD OF UPDATE ON v5
FOR EACH ROW
BEGIN
   UPDATE dept SET dname = :new.dname WHERE dname = :old.dname;
END;
/
```

## 2.16 Stored Procedures and Functions

A **stored procedure** is a named block of PL/SQL, saved in the database, that can be called repeatedly to perform a task.
```sql
CREATE OR REPLACE PROCEDURE proc_name (p1 IN datatype, p2 OUT datatype)
IS
BEGIN
   -- statements
END;
/
EXEC proc_name(value1, value2);
```
A **stored function** works the same way, except it must return a value using `RETURN`.
```sql
CREATE OR REPLACE FUNCTION func_name (p1 IN datatype) RETURN datatype
IS
BEGIN
   RETURN some_value;
END;
/
```

## 2.17 QBE and QUEL

- **QBE (Query By Example)** is a visual query language: instead of typing SQL text, the user fills sample values and conditions into a grid that looks like the table itself. It is used in tools such as Microsoft Access.
- **QUEL** was the query language used by the INGRES database system, based on tuple relational calculus. It came before SQL became the standard, and it uses different keywords, such as `RANGE OF` and `RETRIEVE`, instead of `SELECT` and `FROM`.

## 2.18 DCL and TCL

**DCL — GRANT and REVOKE**
```sql
CONN sys AS sysdba;
CREATE USER username IDENTIFIED BY password;
GRANT CONNECT, RESOURCE TO username;
CONN username/password;

GRANT SELECT, INSERT ON tablename TO username;
REVOKE SELECT, INSERT ON tablename FROM username;
```

**TCL — COMMIT, ROLLBACK, SAVEPOINT**
```sql
COMMIT;              -- permanently saves changes since the last commit
ROLLBACK;             -- undoes changes made since the last commit
SAVEPOINT sp1;        -- marks a point you can roll back to
ROLLBACK TO sp1;      -- undoes only the changes made after sp1
```

## 2.19 Programmatic SQL

- **Embedded SQL**: SQL statements written directly inside the source code of a host language such as C, COBOL, or Java, and compiled together with that code. The SQL's structure is fixed at compile time.
- **Dynamic SQL**: SQL statements that are built and run as plain text strings while the program is running, which makes it possible to write flexible queries whose exact shape isn't known in advance. In PL/SQL this is done with `EXECUTE IMMEDIATE`.
- **ODBC (Open DataBase Connectivity)**: a standard interface that lets an application talk to different database systems using the same set of function calls, regardless of which specific database is being used underneath.

---

## Practice Queries

Try writing each of these out by hand before looking at the answer.

```sql
-- annual salary
SELECT ename, sal, sal*12 annsal FROM emp;

-- everyone except clerks
SELECT * FROM emp WHERE job <> 'CLERK';

-- clerks earning more than 2000
SELECT * FROM emp WHERE job='CLERK' AND sal>2000;

-- employees in specific departments
SELECT * FROM emp WHERE deptno IN (20,30,50,70,80);

-- employees with no commission
SELECT * FROM emp WHERE comm IS NULL;

-- name starts with M / contains M / second letter is L
SELECT * FROM emp WHERE ename LIKE 'M%';
SELECT * FROM emp WHERE ename LIKE '%M%';
SELECT * FROM emp WHERE ename LIKE '_L%';

-- names exactly 5 letters long
SELECT * FROM emp WHERE LENGTH(ename) = 5;

-- hired in December / hired in 1981
SELECT * FROM emp WHERE TO_CHAR(hiredate,'MON')='DEC';
SELECT * FROM emp WHERE TO_CHAR(hiredate,'YYYY')='1981';

-- number of employees per department / per job
SELECT deptno, COUNT(*) FROM emp GROUP BY deptno;
SELECT job, COUNT(*) FROM emp GROUP BY job;

-- departments with more than 3 employees / total salary over 9000
SELECT deptno, COUNT(*) FROM emp GROUP BY deptno HAVING COUNT(*) > 3;
SELECT deptno, SUM(sal) FROM emp GROUP BY deptno HAVING SUM(sal) > 9000;

-- years in which more than 1 employee was hired
SELECT TO_CHAR(hiredate,'YYYY') AS hire_year, COUNT(*) FROM emp
GROUP BY TO_CHAR(hiredate,'YYYY') HAVING COUNT(*) > 1;

-- employees working in Chicago (equi join)
SELECT ename, loc FROM emp, dept WHERE emp.deptno=dept.deptno AND loc='CHICAGO';

-- total salary per department name
SELECT dname, SUM(sal) FROM emp e, dept d WHERE e.deptno=d.deptno GROUP BY dname;

-- employee count, minimum, and maximum salary per location
SELECT loc, COUNT(*), MIN(sal), MAX(sal) FROM emp e, dept d
WHERE e.deptno=d.deptno GROUP BY loc;

-- employee name next to manager name (self join)
SELECT e1.ename AS employee, e2.ename AS manager FROM emp e1, emp e2
WHERE e1.mgr = e2.empno;

-- employees earning more than their manager
SELECT e1.ename, e2.ename AS manager FROM emp e1, emp e2
WHERE e1.mgr=e2.empno AND e1.sal>e2.sal;

-- all departments, including ones with no employees (outer join)
SELECT ename, sal, d.deptno, dname, loc FROM emp e, dept d
WHERE e.deptno(+) = d.deptno;

-- same result, written the ANSI way
SELECT ename, sal, d.deptno, dname, loc FROM emp e
RIGHT OUTER JOIN dept d ON e.deptno = d.deptno;

-- update commission safely, whether it is null or not
UPDATE emp SET comm = NVL2(comm, comm+500, 500);

-- department and job-wise total with automatic subtotals and grand total
SELECT deptno, job, SUM(sal) FROM emp GROUP BY ROLLUP(deptno, job);

-- count how many times a letter appears in a string
SELECT LENGTH('SLEEP') - LENGTH(REPLACE('SLEEP','E')) FROM dual;

-- first date of the current month
SELECT LAST_DAY(ADD_MONTHS(SYSDATE,-1)) + 1 FROM dual;

-- create a view, then insert through it
CREATE OR REPLACE VIEW v1 AS SELECT * FROM emp WHERE deptno=10;
INSERT INTO v1 (empno, ename, deptno) VALUES (1,'murali',30);

-- fast-refresh materialized view, refreshed automatically on commit
CREATE MATERIALIZED VIEW LOG ON base;
CREATE MATERIALIZED VIEW mv3 REFRESH FAST ON COMMIT AS SELECT * FROM base;

-- row-level trigger that logs every deleted row into a backup table
CREATE OR REPLACE TRIGGER tk1
AFTER DELETE ON emp
FOR EACH ROW
BEGIN
  INSERT INTO backup VALUES (:old.empno,:old.ename,:old.job,:old.mgr,
                              :old.hiredate,:old.sal,:old.comm,:old.deptno);
END;
/
```
