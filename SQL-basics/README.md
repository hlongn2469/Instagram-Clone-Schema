# Learning-Database (SQL focus)
===========================================================================================
## Resources
- [ ] Setting up MySQL Cheat Sheet: https://gist.github.com/nax3t/767e06f6af0bafc70b4c4cba0c8d38e7
## Database concepts and jargons
- [ ] Database: A structured set of computerized data with an accessible interface
- [ ] Database Management System (DBMS): Interface for the client to interact with the database
    * MySQL, Oracle, PostgreSQL, SQLite are relational DBMS and all use SQL
- [ ] SQL: Language used to communicate to the database upon a request. Case insensitive
- [ ] Database Server can contain multiple individual databases
## Tables
- [ ] Holds data within a database
- [ ] Columns: header of the data
   * Data type of a column needs to be specified (numeric, string, date)
- [ ] Rows: The actual data 
- [ ] Creating a table:

   ```js
   CREATE TABLE cats
     (
       name VARCHAR(100),
       age INT
     );
   ```
- [ ] Show table column:

   ```js
   SHOW TABLES;
 
   SHOW COLUMNS FROM cats;
 
   DESC cats;
   ```
- [ ] Drop table:
   ```js
   DROP TABLE cats; 
   ```
## Insert, Retrieve, Primary Key
- [ ] Insert:
   * Order matters (column and values)
   ```js
   INSERT INTO table_name(column_name) VALUES (data);
   
   INSERT INTO cats(name, age) VALUES ('Jetson', 7);
   
   INSERT INTO table_name 
            (column_name, column_name) 
   VALUES      (value, value), 
            (value, value), 
            (value, value);
   ```
- [ ] Retrieve:
   ```js
   SELECT * FROM cats;
   
   SELECT * FROM cats where age < 10;
   ```
- [ ] If warnings shown, debug using:
   ```js
   SHOW WARNINGS; 
   ```
- [ ] NULL 
   * Value not known
   * We can create table to prevent null values
  ```js
   CREATE TABLE cats2
  (
    name VARCHAR(100) NOT NULL,
    age INT NOT NULL
  ); 
  ```
   * if inserting a missing field or value, it will automatically set the value of that field to default. Int = 0, VARCHAR = ""
- [ ] Default value:
   ```js
   CREATE TABLE cats3
  (
    name VARCHAR(20) DEFAULT 'no name provided',
    age INT DEFAULT 99
  );
   ```
   * To intentionally specify null value and still catches the missing field or value
      ```js
      CREATE TABLE cats4
      (
      name VARCHAR(20) NOT NULL DEFAULT 'unnamed',
      age INT NOT NULL DEFAULT 99
      );
      ```
- [ ] Key:
   * Primary key: unique column identifier
   * No duplicate primary key value allowed. Will throw error
      ```js
      CREATE TABLE unique_cats
     (
       cat_id INT NOT NULL,
       name VARCHAR(100),
       age INT,
       PRIMARY KEY (cat_id)
     );
      ```
   * Autoincrement primary key by 1:
      ```js
      CREATE TABLE unique_cats2 (
       cat_id INT NOT NULL AUTO_INCREMENT,
       name VARCHAR(100),
       age INT,
       PRIMARY KEY (cat_id)
      );
      ```
## CRUD commands
- [ ] Create - Create table/database
- [ ] Read - Select(pick) and Where(specify) clause
   ```js
      SELECT * FROM cats WHERE name='Egg'; 
      
      SELECT * FROM cats WHERE age=4; 
      
      SELECT * FROM cats WHERE cat_id=age; 
   ```
   * Alias: Hide column name with an alias
      ```js
      SELECT cat_id AS id, name FROM cats;
 
      SELECT name AS 'cat name', breed AS 'kitty breed' FROM cats;
      ```
- [ ] Update: **Update** table_name **Set** target_variable **Where** specify
   ```js
      UPDATE cats SET breed='Shorthair' WHERE breed='Tabby'; 
   
      UPDATE cats SET age=14 WHERE name='Misty'; 
   ```
- [ ] Delete: **Delete** **FROM** table_name **WHERE** specify
   ```js
      DELETE FROM cats WHERE name='Egg'; 
   
      DELETE FROM cats; 
   ```
## Running SQL Files
- [ ] Simple to edit SQL code
- [ ] History of work saved
```js
   mysql-ctl cli
 
   use cat_app;
   
   source first_file.sql
 
   DESC cats;
```
## String functions
- [ ] reference: https://dev.mysql.com/doc/refman/8.0/en/string-functions.html
- [ ] Concatenation: **Select** **CONCAT** column specifier for concat **From** table_name;
   ```js
      SELECT
      CONCAT(author_fname, ' ', author_lname)
      FROM books;

      SELECT
        CONCAT(author_fname, ' ', author_lname)
        AS 'full name'
      FROM books;

      SELECT author_fname AS first, author_lname AS last, 
        CONCAT(author_fname, ' ', author_lname) AS full
      FROM books;
   ```
- [ ] Substring:
   ```js
      SELECT SUBSTRING('Hello World', 1, 4);
 
      SELECT SUBSTRING('Hello World', 7);
      
      SELECT CONCAT
       (
           SUBSTRING(title, 1, 10),
           '...'
       )
      FROM books;
   ```
- [ ] Replace: Case sensitive
   ```js
      SELECT REPLACE('Hello World', 'Hell', '%$#@');
      
      SELECT REPLACE(title, 'e ', '3') FROM books;
 
      SELECT
      SUBSTRING(REPLACE(title, 'e', '3'), 1, 10)
      FROM books;
   ```
- [ ] Reverse: gives a palindrome
   ```js
      SELECT REVERSE(author_fname) FROM books;
 
      SELECT CONCAT('woof', REVERSE('woof'));
   ```
- [ ] Char_length: count string characters
   ```js
      SELECT CHAR_LENGTH('Hello World');
 
      SELECT author_lname, CHAR_LENGTH(author_lname) AS 'length' FROM books;
 
      SELECT CONCAT(author_lname, ' is ', CHAR_LENGTH(author_lname), ' characters long') FROM books;
   ```
- [ ] Upper() and Lower(): Change a string to upper case or lower case
   ```js
      SELECT LOWER('Hello World');
 
      SELECT UPPER(title) FROM books;
 
      SELECT CONCAT('MY FAVORITE BOOK IS ', UPPER(title)) FROM books;
   ```
- [ ] NOTES: DRY (DONT REPEAT YOURSELF) and ORDER MATTERS
## Refining/filtering Selection
- [ ] Distinct: remove duplicates of the selected column
   ```js
      SELECT DISTINCT author_lname FROM books;
      
      SELECT DISTINCT author_fname, author_lname FROM books;
   ```
- [ ] OrderBy: Sort data ascendingly by default 
   ```js
      SELECT author_lname FROM books ORDER BY author_lname;
      
      SELECT author_lname FROM books ORDER BY author_lname DESC;
      
      SELECT author_fname, author_lname FROM books 
      ORDER BY author_lname, author_fname;
      
      SELECT author_lname, title
      FROM books ORDER BY 2;
   ```
- [ ] Limit: Get a limited number of items. Best practices to use with ORDERBY
   ```js
      SELECT title FROM books LIMIT 10;
      
      SELECT title, released_year FROM books 
      ORDER BY released_year DESC LIMIT 5;
      
      SELECT title, released_year FROM books 
      ORDER BY released_year DESC LIMIT 1,3;
   ```
- [ ] Like: For better searching. If the user only knows the author first name but not the book title for example.
   ```js
      // as long as the author name has "da"
      SELECT title, author_fname FROM books WHERE author_fname LIKE '%da%';
      
      // starts with da only
      SELECT title, author_fname FROM books WHERE author_fname LIKE 'da%';
      
      // exact term
      SELECT title FROM books WHERE title LIKE 'the';
   ```
- [ ] Additional Wildcards:
   ```js
      // select where stock quantity has 4 char or digits
      SELECT title, stock_quantity FROM books WHERE stock_quantity LIKE '____';
      
      // select where stock quantity has 2 char or digits
      SELECT title, stock_quantity FROM books WHERE stock_quantity LIKE '__';
      
      // if title contains % or _, then use \
      SELECT title FROM books WHERE title LIKE '%\%%'
 
      SELECT title FROM books WHERE title LIKE '%\_%'
   ```
## Aggregate functions
- [ ] Count: 
   ```js
      // count all books
      SELECT COUNT(*) FROM books;
      
      // count author first name
      SELECT COUNT(author_fname) FROM books;
      
      // count distinct author first name
      SELECT COUNT(DISTINCT author_fname) FROM books;
      
      // count where title contains 'the'
      LECT COUNT(*) FROM books WHERE title LIKE '%the%';
   ```
- [ ] GroupBy: Summarize identical data into single rows
   ```js
      // pick author last name to present, count numbers of books written by each uniquely grouped author last name in each row
      SELECT author_lname, COUNT(*) 
      FROM books GROUP BY author_lname;
      
      SELECT released_year, COUNT(*) FROM books GROUP BY released_year;
   ```
- [ ] MIN and MAX: 
   ```js
      SELECT MAX(released_year) 
      FROM books;
 
      SELECT MAX(pages), title
      FROM books;
      
      // gives title and page where max page is
      SELECT title, pages FROM books 
      WHERE pages = (SELECT Max(pages) 
                FROM books);
   ```
- [ ] Using Min and Max with GROUPBY:
    ```js
      // pick the year each author published their first book
      SELECT author_fname, 
       author_lname, 
       Min(released_year) 
      FROM   books 
      GROUP  BY author_lname, 
                author_fname;
      
      // find the longest page count for each author
      SELECT
        author_fname,
        author_lname,
        Max(pages)
      FROM books
      GROUP BY author_lname,
               author_fname;
   ```
- [ ] Sum:
   ```js
   // sum all pages in the entire database
   SELECT SUM(pages) FROM books; 
   
   // sum all pages each author has written
   SELECT author_fname,
       author_lname,
       Sum(pages)
   FROM books
   GROUP BY
       author_lname,
       author_fname;
   ```
- [ ] AVG: Average
   ```js
   // calculate the average released year across all books
   SELECT AVG(released_year) 
   FROM books;
   
   // calculate average stock quantity for books released in the same year
   SELECT AVG(stock_quantity) 
   FROM books 
   GROUP BY released_year;
   ```
## Data types
- [ ] Storing text
   * VarChar: variable length
   * Char: fixed length, better performance
- [ ] Decimal 
   * Decimal(# max_digit_long, # digit_after_decimal_point) 
   ```js
      CREATE TABLE items(price DECIMAL(5,2));
      
      // 7.00
      INSERT INTO items(price) VALUES(7);
      
      // 34.88
      INSERT INTO items(price) VALUES(34.88);
      
      // 299.00
      INSERT INTO items(price) VALUES(298.9999);
   ```
- [ ] Float and double
   * Float | 4 bytes | ~7 digits
   * Double | 8 bytes | ~15 digits
   ```js
      CREATE TABLE thingies (price FLOAT);
 
      INSERT INTO thingies(price) VALUES (88.45);
      
      INSERT INTO thingies(price) VALUES (8877665544.45);
   ```
- [ ] Dates & Times
Reference: https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html OR STACKOVERFLOW
   * If you want to store a specific value you should use a datetime field.
   * Date: 'yyyy-mm-dd' 
   * Time: 'HH:MM:SS'
   * DateTime: 'yyyy-mm-dd HH:MM:SS'
   ```js
      CREATE TABLE people (name VARCHAR(100), birthdate DATE, birthtime TIME, birthdt DATETIME);
 
      INSERT INTO people (name, birthdate, birthtime, birthdt)
      VALUES('Padma', '1983-11-11', '10:07:35', '1983-11-11 10:07:35');

      INSERT INTO people (name, birthdate, birthtime, birthdt)
      VALUES('Larry', '1943-12-25', '04:10:42', '1943-12-25 04:10:42');
   ```
- [ ] Curdate, curtime, now
   * Curdate(): gives current date
   * Curtime(): gives current time
   * now(): gives current datetime
- [ ] Formating datetime
   ```js
      // weekday
      SELECT DATE_FORMAT(birthdt, 'Was born on a %W') FROM people;
 
      SELECT DATE_FORMAT(birthdt, '%m/%d/%Y') FROM people;
 
      SELECT DATE_FORMAT(birthdt, '%m/%d/%Y at %h:%i') FROM people;
   ```
- [ ] DateMath: Perform math operations on date 
   ```js
      SELECT name, birthdate, DATEDIFF(NOW(), birthdate) FROM people;
      
      SELECT birthdt, DATE_ADD(birthdt, INTERVAL 1 MONTH) FROM people;
      
      SELECT birthdt, birthdt + INTERVAL 15 MONTH + INTERVAL 10 HOUR FROM people;
   ```
- [ ] Timestamp
   * Support values from '1970-01-01' to '2038-01-19'
   * Timestamps in MySQL are generally used to track changes to records, and are often updated every time the record is changed.
- [ ] Logical Operator:
   * Same as in programming
   * !=
   ```js
   SELECT title, author_lname FROM books WHERE author_lname = 'Harris';
   ```
   * Not Like
   ```js
   SELECT title FROM books WHERE title NOT LIKE 'W%';
   ```
   
   * >=
   ```js
   SELECT title, stock_quantity FROM books WHERE stock_quantity >= 100;
   ```
   
   * &&
   ```js
   SELECT * FROM books WHERE author_lname='Eggers' 
       AND released_year > 2010 
       AND title LIKE '%novel%';
   ```
   * ||
   ```js
   SELECT title, author_lname, released_year, stock_quantity FROM books 
   WHERE  author_lname = 'Eggers' 
                 OR released_year > 2010 
   OR     stock_quantity > 100;
   ```
   * Between: CASTING RECOMMENDED
   ```js
   SELECT name, birthdt FROM people WHERE 
    birthdt BETWEEN CAST('1980-01-01' AS DATETIME)
    AND CAST('2000-01-01' AS DATETIME);
   ```
   * In/NotIn: Shorter alternatives for ||/&& respectively
   ```js
   SELECT title, author_lname FROM books
   WHERE author_lname IN ('Carver', 'Lahiri', 'Smith');
   
   SELECT title, released_year FROM books
   WHERE released_year NOT IN 
   (2000,2002,2004,2006,2008,2010,2012,2014,2016);
   ```
   
   * Modulo:
   ```js
   SELECT title, released_year FROM books
   WHERE released_year >= 2000 AND
   released_year % 2 != 0 ORDER BY released_year;
   ```
   * Case - Case/Else: if-if/else 
   ```js
   SELECT title, released_year,
       CASE 
         WHEN released_year >= 2000 THEN 'Modern Lit'
         ELSE '20th Century Lit'
       END AS GENRE
   FROM books;
   ```
