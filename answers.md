# answers to the possible questions from chapter 1 and 2 (c1n2f.md)

### chapter 1

- - -

- q1: **define data and information with examples**

    data: it is a collection of raw facts, on its own data does not carry any meaning. examples: the number 45, the name "x", the date 17 sep 2026

    information: it is what you get when data is processed into something meaningful. examples: a student's marksheet, a customers invoice or a sport's scorecard

- - -

- q2: **what is a data store? name three kinds**

    a data store is any place where you can permanently store data or information in a computer system

    there are three kinds of data stores:
    1. papers and books
    2. flat files (basic computer files)
    3. databases 

- - -

- q3: **differentiate between data and information**
    
    data is the raw unprocessed collection of facts, which on their own have no meaning
    information is the after-product of processing the data, it is meaningful

- - -

- q4: **what is a flat file? explain how data is managed in a flat-file system using a diagram**

    a flat file is the traditional way of permanently storing data on a computer. 
    in a flat file system, every application program keeps its own seperate file and there is no sharing of files between applications 

    for the diagram look at chapter 2

- - -

- q5: **explain the drawbacks of a flat-file system**

    the flat-file system has several drawbacks

    1. **data retrieval**: to read out data from a flat-file you must write an entire program in a higher level language like C as there is no built in-query language

    2. **data redundancy**: the same instance of data might be sitting in multiple different files, if there is a change in one of the files, it might not be the case for the other files where the same instance is being used, this leads to a lot of redundant and inconsistent data

    3. **data integrity**: integrity means keeping the data valid and correct, unlike a database there are no constraints like primary key, not null, etc. to enforce validity on the data

    4. **data security**: flat-files have no security mechanism, whoever has access to the file can read all of the contents and change it, unlike a database which gives different levels of access through views 

    5. **data indexing**: flat files cannot be indexed, meaning to find a row you must scan the entire translation unit (file) so this leads to performance issues 

- - -

- q6: **why did organizations move from flat files to dbms?**

    as you have already read about the drawbacks of flat-file systems, organizations moved to dbms over it as:

    1. **data retrieval**: dbms uses a query language like sql, which makes data retrieval very easy. it has ddl, dml, dql, tcl and dcl built-in for efficient storage, retrieval and management of data

    2. **data redundancy**: dbms deals with the problem of data redundancy in flat-file systems through normal forms and the process of normalization, which is just a process to split larger tables into smaller, more readable and atomic tables without losing information

    3. **data integrity**: dbms maintains data validity throughout the data using constraints like primary key, foreign key, not null, check etc.

    4. **data security**: databases have role-based security where each user has their own set of views and permissions 

    5. **data indexing**: databases support indexing, so searches are extremely fast when compared to flat-file systems

- - -

- q7: **define dbms. give 5 examples of dbms software**

    a dbms (database management system) is a collection of programs written to manage data. 
    it is used to store, manage, and retrieve data efficiently. it can also define the structure of the data, it allows you to manipulate it and control which user can access which part of the data.

    examples of dbms software: oracle, mysql, sqlite, sqlplus, sybase, teradata

- - -

- q8: **explain the advantages of dbms over flat-file systems**

    same answer as q6. just frame it in a slightly different way.

- - -

- q9: **what are acid properties? explain each with an example.**

    every transaction in a database automatically has four properties known as acid, this is what keeps data consistent without programmer effort

    1. **A (Atomicity)**: a transaction either finishes fully or does not happen at all. there is no half-finished state
    example: transferring 1000 points from account A to account B involves 2 steps, decrement A by 1000 and increment B by 1000. if the system crashes after the decrement, then atomicity ensures the decrement is rolled back too

    2. **C (Consistency)**: a transaction moves the database from one valid state to another valid state, it never leaves the database in an invalid state
    example: let us suppose your database has a rule which states "total points across all accounts must always be equal to 10000". before a transfer A has 4000 and B has 6000 (total 10000), after transferring 1000 from A to B, A has 3000 and B 7000 (total is still 10000). here the database moved from one valid state to another valid state, if a transaction tried to leave the total at 9000 then the database would reject it

    3. **I (Isolation): many transactions can run at the same time without interfering with each other. it looks to each user as if they are alone in the system 
    example: if there are 2 people trying to book a ticket for the same seat on a plane, it says the seat is available for both of them and it lets them both book it, but after processing the request, only one of them gets the seat and the other gets told that the seat has already been booked

    4. **D (Durability)**: once a transaction is committed, its effects are  permanent, even if the computer crashes right after
    example: if you transfer 1000 from A to B, and the system successfully processes the transaction and one minute later the system crashes, the transfer is still recorded and the transaction result stays the same

- - -

- q10: **list and explain the main components of a dbms**

    the main components of a dbms are:

    1. **query processor**: takes the query written by the user and checks whether it is syntactically correct, translates the query into low-level instructions the database can actually execute and also decides the most efficient way to run the query 

    2. **ddl compiler**: compiles the data defintion language commands (create, alter, drop)

    3. **dml compiler**: compiles the data manipulation language commands (insert, update delete)

    4. **storage manager**: acts as the bridge between low-level data stored on the disk and queries submitted by the user. it is responsible for storing, retrieving and updating data in the database

    5. **transaction manager**: ensures that all the transactions follow acid properties, it manages concurrent transactions so that they do not interfere with each other 

    6. **buffer manager**: manages the transfer of data between the hard disk and the main memory, it keeps the frequently used data in the main memory buffers so that the queries run faster

    7. **file manager**: manages how data is actually stored in files on the disk, it handles the allocation of disk space and the data structures used to represent the stored data

    8. **data dictionary**: a special set of tables which store information about the database itself, called metadata. it keeps track of all the existing tables, data types, constraints etc.

    9. **database**: the actual place on the disk where all the user data and metadata is stored permanently

    10. **user interface**: the interface through which the user can interact with the dbms, this includes the login screen, the sql command prompt and any application programs that connect to the database

- - - 

- q11: **draw and explain the dbms approach for data management**

    when you install dbms software, some space is automatically created on the hard disk. this space is called the database. a user interface is also created so you can log in and directly interact with the database. you can also interact with the database indirectly through application programs that are connected to the database.

    for the diagram refer to chapter 3

- - -

- q12: **explain multi-user dbms architecture**

    a database management system is designed for many users and many applcation programs to connect at the same time and work on the same database. 
    it handles this using concurrency control and locking, which ensure that two users cannot corrupt each other's data
    a flat-file system does not have such mechanism 

- - -

- q13: **define database**
    
    a database is an organised collection of interrelated data. 
    this data resides in your hard disk physically, and dbms helps you manage it.

- - -

- q14: **explain the two types of structures a database has**

    every database has two kinds of structures:

    1. **logical structure**: this is the structure which is not visible in the operating system, it contains tables, views etc. and it is handled by the database developer or database administrator

    2. **physical structure**: this is the structure which is visible in the operating system, this refers to the actual files on the disk. it can only be handled by the database administrator

- - -

- q15: **who handles logical and physical structures?**

    a database developer handles the logical structure of a database whereas a database administrator can handle both the logical structure and physical structure of a database

