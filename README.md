# Nthabiseng Maya

     LIBRARY MANAGEMENT SYSTEM DATABASE
1.DATABASE PURPOSE
-The purpose of the library is to store and manage information about books,library members and their records.This will help the library to be aware of their available books and registered members or returned books efficiently

2.TABLE SEPARATION
BOOKS-Stores information about books,title of the books,authors and the year the book is published

MEMBERS-Stores information about registered library members 

BORROWINGS-Stores information about books that are being borrowed by members including the borrowing dates and returned dates

3.FIELDS & PRIMARY KEYS
- member_id----SERIAL PRIMARY KEY
- first_name---VARCHAR(50) NOT NULL
- last_name---VARCHAR(50) NOT NULL
- email_address---VARCHAR(100) UNIQUE NOT NULL
- phone_number---INTEGER(15)

PRIMARY KEY--member_id
-the member_id simply identifies each member in the library and prevents unneeded information

postgres=# CREATE DATABASE library_db;
CREATE DATABASE
postgres=# \c library_db
You are now connected to database "library_db" as user "postgres".
library_db=# CREATE TABLE authors(
library_db(# author_id SERIAL PRIMARY KEY,
library_db(# first_name VARCHAR(50) NOT NULL,
library_db(# last_name VARCHAR(50) NOT NULL
library_db(# );
CREATE TABLE
library_db=# \d authors
                                         Table "public.authors"
   Column   |         Type          | Collation | Nullable |                  Default
------------+-----------------------+-----------+----------+--------------------------------------------
 author_id  | integer               |           | not null | nextval('authors_author_id_seq'::regclass)
 first_name | character varying(50) |           | not null |
 last_name  | character varying(50) |           | not null |
Indexes:
    "authors_pkey" PRIMARY KEY, btree (author_id)


library_db=# CREATE TABLE books(
library_db(# book_id SERIAL PRIMARY KEY,
library_db(# book_title VARCHAR(100) NOT NULL,
library_db(# publication_year INTEGER,
library_db(# author_id INTEGER REFERENCES authors(author_id)
library_db(# );
CREATE TABLE
library_db=# \d books
                                           Table "public.books"
      Column      |          Type          | Collation | Nullable |                Default
------------------+------------------------+-----------+----------+----------------------------------------
 book_id          | integer                |           | not null | nextval('books_book_id_seq'::regclass)
 book_title       | character varying(100) |           | not null |
 publication_year | integer                |           |          |
 author_id        | integer                |           |          |
Indexes:
    "books_pkey" PRIMARY KEY, btree (book_id)
Foreign-key constraints:
    "books_author_id_fkey" FOREIGN KEY (author_id) REFERENCES authors(author_id)


library_db=# INSERT INTO authors(author_id, first_name, last_name) VALUES(1, 'Kabelo', 'Mabalane'),
library_db-# (2, 'Bonolo', 'Makume'),
library_db-# (3, 'Lesego', 'Smith'),
library_db-# (4, 'Mpho', 'Malindi'),
library_db-# (5, 'Denilson', 'Fox');
INSERT 0 5
library_db=# SELECT * FROM authors;
 author_id | first_name | last_name
-----------+------------+-----------
         1 | Kabelo     | Mabalane
         2 | Bonolo     | Makume
         3 | Lesego     | Smith
         4 | Mpho       | Malindi
         5 | Denilson   | Fox
(5 rows)


library_db=# INSERT INTO books(book_id, book_title, publication_year, author_id) VALUES(087, 'Harry Potter', 1997, 1),
library_db-# (1435, 'Animal Farm', 1945, 2),
library_db-# (698, 'Things Fall Apart', 1958, 3),
library_db-# (12, 'The Great Gatsby', 1925, 4),
library_db-# (987, 'Purple Hibiscus', 2003, 5);
INSERT 0 5
library_db=# SELECT * FROM books;
 book_id |    book_title     | publication_year | author_id
---------+-------------------+------------------+-----------
      87 | Harry Potter      |             1997 |         1
    1435 | Animal Farm       |             1945 |         2
     698 | Things Fall Apart |             1958 |         3
      12 | The Great Gatsby  |             1925 |         4
     987 | Purple Hibiscus   |             2003 |         5
(5 rows)


library_db=# SELECT books.title,
library_db-# authors.first_name,
library_db-# authors.last_name
library_db-# FROM books
library_db-# INNER JOIN authors
library_db-# ON books.author_id = authors.author_id;
ERROR:  column books.title does not exist
LINE 1: SELECT books.title,
               ^
library_db=# \dt
           List of tables
 Schema |  Name   | Type  |  Owner
--------+---------+-------+----------
 public | authors | table | postgres
 public | books   | table | postgres
(2 rows)


library_db=# SELECT books.book_title,
library_db-# authors.first_name,
library_db-# authors.last_name
library_db-# FROM books
library_db-# INNER JOIN authors
library_db-# ON books.author_id = authors.author_id;
    book_title     | first_name | last_name
-------------------+------------+-----------
 Harry Potter      | Kabelo     | Mabalane
 Animal Farm       | Bonolo     | Makume
 Things Fall Apart | Lesego     | Smith
 The Great Gatsby  | Mpho       | Malindi
 Purple Hibiscus   | Denilson   | Fox
(5 rows)
