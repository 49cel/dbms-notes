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

    
