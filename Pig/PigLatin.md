#### Overview:
PigLatin is an english scripting like language offred by Pig project:

PigLatin language looks like SQL, but it is not SQL.

It has several operators and functions which are used to perform traditional data transformations like projection, mapping, sorting, joining, grouping and aggregations etc...

#### Pig Latin Building Blocks:

The following are building Blocks of Pig Latin:

1. Comments
2. Case Sensitivity
3. Casting
4. Conventions
5. Keywords
4. Data Types
5. Storage Types
6. Relations, Schemas, Fields
7. Operators
8. Functions
9. Macros
10. Pig Latin Preprocessor
11. Pig Compiler
12. Varable/Parameter Substitution
13. Pig Commands
14. Pig Unit


Comments:

Pig Latin supports two types of comments
1. Single line - SQL Style (--)
2. Multi line - Java Style (/*....*/)

Case Sensivity:
Pig Latin neither case sensitive nor case insensitive. It is both, some of the things are case sensitive and some of the
things are case insensitive.
Case Sensitive examples are: Functions, relations, Storage types, fields etc...
Case insensitive examples are: operators, keywords etc...

Casting:
Pig Latin supports two types of casting:
1. Implicit casting: done by compiler. This works within the same data types family like int to long.
2. Explicit casting: done by developer. This works across the data types like string to int, double to int etc..
example: (datatype) exapression/field
          (double) A/B Both A and B are type int;
          
Data Types:
PigLatin has two types of data types:
1. Scalar Data Types
2. Complex Data Types

Scalar data Types: These are used to hold the scalar/atomic/primitive values. Some of the scalar data types are:
int, long, float, double, chararray, bytearray, boolean, datetime etc..

Complex data Types: These are used hold the multiple values of same or different type. We have three complex data types in Pig Latin
They are:
1. tuple
2. map
3. bag

Tuple: It is an ordered collection of fields. It is denoted by (). It may conatin scalar or complex data types as it's
field data types
Map: It is key-value pair association. It is denoted by [].
Bag: It is an unordered collection of tuples. It is also called a relation. It is denoted by {}. It uses both memeory and disk
to store it's data. Naturally bag is very big data type in terms of values.

Storage Types:
Pig Latin has seveal storage types, which are used to parse the content while reading and writing from/to File System(HDFS)
The following are some of the Storage Types:

|Storage Type:               |Read/Load           |Store/Write |
|----------------------------|--------------------|------------|
| PigStorage|                    YES|                 YES|
| HBaseStorage|                  YES|                 YES|
| JsonStorage|                   YES|                 YES|
| BinStorage|                    YES|                 YES|
| HCatLoader|                    YES|                 NO |
| HCatStorer|                    NO |                 YES|
| SequenceLoader|                YES|                 NO |
| SequenceStorer|                NO |                 YES|
| ParquetLoader|                 YES|                 NO |
| ParquetStorer|                 NO |                 YES|
| JsonStorage|                   YES|                 YES|                         
| TextLoader|                    YES|                 NO |
| AvroStorage|                   YES|                 YES|
| TrevniStorage|                 YES|                 YES|
| AccumuloStorage|               YES|                 YES|
---------------------------------------------------------

default storage type is PigStorage()
PigStorage uses new line as record delimiter, tab as field delimiter by default.

Relations, Schemas, Fields:
Realtion: A relation is a bag. It is also a variable to reference the pig latin statements. In pig script, for all pig latin statements
before the opehand is called a relation name. Relations are must start with an alphabhet.
Schema: Pig imposes schema on File System data while reading (i.e Read on Schema). This schema imposistion happens two ways,
Those are:
1. Automatic - Pig Guessed Schema
2. Manual - Developer provided

Automatic: While loading the data from file system into pig, if developer ignores schema, pig automatically guess schema
for input data. This is schema conatins bytearray as data type for all the fields. All the field names are anonymous. These
fields can be accessed by using $0....$n notation i.e $o is column1, $1 is column2 and $n is columnn+1 etc...

Manual: While loading the data from file system into pig, developer provides schema. This provided schema can be used in the
following subsequent pig latin statements

Fiedl: A field is a piece of data. It has name and value, name is aplhanumeric where as value is scalar or complex data type

Operators:
Pig Latin has two types of Operators:
1. Input/Output Operators
2. Relational Operators

Input/Output Operators: These operators are used to load, store the data from/to file system (HDFS). These operators are
only either reading or storing not for processing the data. The following are input and output operators:

load - input - read the data from file system
store - output - store the data into file system
dump - output (it prints the data in console)
```
Load Usage:
----------
relation = load 'path of the file in file system' using <storage type> as <schema>
relation = load 'path of the file in file system' as <schema> [Default storage type used]
relation = load 'path of the file in file system' using <storage type> [Automatic schema/ Pig Guessed Schema]
relation = load 'path of the file in file system' [Default storage type used and Automatic schema/ Pig Guessed Schema]

example:
mydata = load '/pig/ratings' using PigStorage(':') as (uid:int, mid:int, rating:double, time:long);
```

Pig Latin also offers two classes called LoadFunc, StoreFunc for developing custom input and output operators. These two classes
can be extension points two extend the input and output operators.

Relational Operators: These are used to perform the traditional data operations like projection, selection, filter, joining,
transformations, aggregations, etc... These operators look like SQL Operators. The following are relational operators from Pig Latin

1. foreach
2. filter
3. order by
4. group by
5. limit
6. distinct
7. sample
8. split
9. union
10. cross
11. cogroup
12. flatten
13. define
14. cube
15. assert
16. parallel
17. import
18. join
19. mapreduce
20. stream
21. rank

1. foreach: It used to iterate the relations and performs the data transformations, projections, aggregations etc..
   ```
   Usage: relation2 = foreach <relation1> generate ....
   Example: mydata = load '/pig/ratings' using PigStorage() as (uid:int, mid:int, rating:double, time:long);
            projdata = foreach mydata generate mid, rating;
   ```         
2. filter: used to filter the data based on condition
   ```
   usage: relation2 = filter relation1 by filed condition
   example: fltdata = filter projdata by rating > 0.0;
   ```
3. group: used to group the data. This grouping can be done in two ways.
          1. by field
          2. by all
   ```
   usage for by field:
          relation2 = group relation by field;
   example:
          grpdata = group fltdata by mid;
          
   usage for all:
          relation2 = group relation all;
   example:
          grpdata = group fltdata all;
   ```
4. order by: Used to sort the data either in asc order or desc order
   ```
   usage: relation2 = order relation1 by field [asc/desc] default asc order
   example: orddata = order avgdata by avgrat desc;
   ```
5. limit: used to limit the results to a particular number
   ```
   usage: relation2 = limit relation1 n
   example: lmtdata = limit orddata 10;
   ```
6. distinct: used to get the distnct values from the data. It works on only tuples.
   ```
   usage: relation2  = distinct relation1;
   Example: To get the distinct movies:
   movies = load '/pig/movies' using PigStorage(':') as (mid:int, title:chararray, genre:chararray);
   mproj = foreach movies generate title;
   uniq = distinct mproj;
   
   Wrong One:  uniq = distinct movies.title;
   ```
 7. Sample: Used to get the sample out of the total data. This sample represents the entire data. Here the sampling algorithm is
    random sampling.
    ```
    usage: relation2 = sample relation1 percentage;
    example: sampldata = sample fltdata 0.1; it gets the 10% of the total data
    ```
 8. 
