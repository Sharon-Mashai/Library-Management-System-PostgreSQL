# Library Management System

A **Library Management System** built using **PostgreSQL** to manage books, authors, and patrons.

The project covers database tables, relationships, CRUD operations, PostgreSQL arrays, filtering, searching, and advanced SQL queries.

## Technologies

- PostgreSQL
- SQL
- pgAdmin
- psql

## Running Queries

### pgAdmin

1. Open pgAdmin and connect to the PostgreSQL server.
2. Select the required database.
3. Open **Tools → Query Tool**.
4. Enter the SQL query.
5. Click **Execute** to run it.

### psql

Connect to PostgreSQL:

```bash
psql -U postgres
```

Connect to the project database:

```text
\c LibraryDB
```

Queries can then be entered and executed directly in the terminal.

---

# Sprint 1: Project Setup

Sprint 1 creates the `LibraryDB` database and the three required tables: `authors`, `books`, and `patrons`.

## Create Database

```sql
CREATE DATABASE "LibraryDB";
```

After creating the database, connect to `LibraryDB` before running the remaining queries.

## Create Authors Table

```sql
CREATE TABLE authors (
    id INT PRIMARY KEY,
    name VARCHAR(150) NOT NULL,
    nationality VARCHAR(100),
    birth_year INT,
    death_year INT
);
```

The `id` is the primary key and uniquely identifies each author.

## Create Books Table

```sql
CREATE TABLE books (
    id INT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    author_id INT NOT NULL,
    genres TEXT[],
    published_year INT,
    available BOOLEAN DEFAULT TRUE,

    CONSTRAINT fk_books_author
        FOREIGN KEY (author_id)
        REFERENCES authors(id)
);
```

`genres` uses a PostgreSQL `TEXT[]` array so a book can have multiple genres.

`author_id` is a foreign key that connects each book to an author in the `authors` table.

`available` stores `TRUE` or `FALSE` to indicate whether a book is available.

## Create Patrons Table

```sql
CREATE TABLE patrons (
    id INT PRIMARY KEY,
    name VARCHAR(150) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    borrowed_books INT[] DEFAULT ARRAY[]::INT[]
);
```

`borrowed_books` is an integer array containing the IDs of books borrowed by a patron.

## Verify Tables

```sql
SELECT table_name
FROM information_schema.tables
WHERE table_schema = 'public'
ORDER BY table_name;
```

Expected tables:

```text
authors
books
patrons
```

---
# Sprint 2: Insert Data

Sprint 2 adds the sample authors, books, and patrons to the database.

## Insert Authors

```sql
INSERT INTO authors (id, name, nationality, birth_year, death_year)
VALUES
(1, 'George Orwell', 'British', 1903, 1950),
(2, 'Harper Lee', 'American', 1926, 2016),
(3, 'F. Scott Fitzgerald', 'American', 1896, 1940),
(4, 'Aldous Huxley', 'British', 1894, 1963),
(5, 'J.D. Salinger', 'American', 1919, 2010),
(6, 'Herman Melville', 'American', 1819, 1891),
(7, 'Jane Austen', 'British', 1775, 1817),
(8, 'Leo Tolstoy', 'Russian', 1828, 1910),
(9, 'Fyodor Dostoevsky', 'Russian', 1821, 1881),
(10, 'J.R.R. Tolkien', 'British', 1892, 1973);
```

## Insert Books

```sql
INSERT INTO books (id, title, author_id, genres, published_year, available)
VALUES
(1, '1984', 1, ARRAY['Dystopian', 'Political Fiction'], 1949, TRUE),
(2, 'To Kill a Mockingbird', 2, ARRAY['Southern Gothic', 'Bildungsroman'], 1960, TRUE),
(3, 'The Great Gatsby', 3, ARRAY['Tragedy'], 1925, TRUE),
(4, 'Brave New World', 4, ARRAY['Dystopian', 'Science Fiction'], 1932, TRUE),
(5, 'The Catcher in the Rye', 5, ARRAY['Realist Novel', 'Bildungsroman'], 1951, TRUE),
(6, 'Moby-Dick', 6, ARRAY['Adventure Fiction'], 1851, TRUE),
(7, 'Pride and Prejudice', 7, ARRAY['Romantic Novel'], 1813, TRUE),
(8, 'War and Peace', 8, ARRAY['Historical Novel'], 1869, TRUE),
(9, 'Crime and Punishment', 9, ARRAY['Philosophical Novel'], 1866, TRUE),
(10, 'The Hobbit', 10, ARRAY['Fantasy'], 1937, TRUE);
```

## Insert Patrons

```sql
INSERT INTO patrons (id, name, email, borrowed_books)
VALUES
(1, 'Alice Johnson', 'alice@example.com', ARRAY[]::INT[]),
(2, 'Bob Smith', 'bob@example.com', ARRAY[1, 2]),
(3, 'Carol White', 'carol@example.com', ARRAY[]::INT[]),
(4, 'David Brown', 'david@example.com', ARRAY[3]),
(5, 'Eve Davis', 'eve@example.com', ARRAY[]::INT[]),
(6, 'Frank Moore', 'frank@example.com', ARRAY[4, 5]),
(7, 'Grace Miller', 'grace@example.com', ARRAY[]::INT[]),
(8, 'Hank Wilson', 'hank@example.com', ARRAY[6]),
(9, 'Ivy Taylor', 'ivy@example.com', ARRAY[]::INT[]),
(10, 'Jack Anderson', 'jack@example.com', ARRAY[7, 8]);
```

Verify the inserted data with:

```sql
SELECT * FROM authors;
SELECT * FROM books;
SELECT * FROM patrons;
```

Each table should contain **10 records**.