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
