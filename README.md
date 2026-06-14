# A3-B7-Football-Ticket-Booking-System

## Project Overview

The Football Ticket Booking System is a relational database designed to manage football match ticket reservations. The system stores information about users, football matches, and ticket bookings while enforcing data integrity through primary keys, foreign keys, unique constraints, and check constraints.

This project demonstrates the use of SQL Data Definition Language (DDL), Data Manipulation Language (DML), relational database design, and query operations.

---

## Database Schema

The database consists of three main tables:

### 1. Users

Stores information about registered users and ticket managers.

| Column       | Data Type    | Description          |
| ------------ | ------------ | -------------------- |
| user_id      | SERIAL       | Primary Key          |
| full_name    | VARCHAR(100) | User's full name     |
| email        | VARCHAR(100) | Unique email address |
| role         | VARCHAR(50)  | User role            |
| phone_number | VARCHAR(20)  | Contact number       |

### Constraints

* Primary Key: `user_id`
* Unique Constraint: `email`
* Allowed Roles:

  * Football Fan
  * Ticket Manager

---

### 2. Matches

Stores football match information and ticket pricing.

| Column              | Data Type     | Description               |
| ------------------- | ------------- | ------------------------- |
| match_id            | SERIAL        | Primary Key               |
| fixture             | VARCHAR(200)  | Match fixture             |
| tournament_category | VARCHAR(100)  | Tournament name           |
| base_ticket_price   | DECIMAL(10,2) | Ticket price              |
| match_status        | VARCHAR(50)   | Match availability status |

### Constraints

* Primary Key: `match_id`
* Ticket price cannot be negative
* Allowed Match Status:

  * Available
  * Selling Fast
  * Sold Out
  * Postponed

---

### 3. Bookings

Stores ticket booking transactions.

| Column         | Data Type     | Description        |
| -------------- | ------------- | ------------------ |
| booking_id     | SERIAL        | Primary Key        |
| user_id        | INT           | Foreign Key        |
| match_id       | INT           | Foreign Key        |
| seat_number    | VARCHAR(20)   | Assigned seat      |
| payment_status | VARCHAR(50)   | Payment state      |
| total_cost     | DECIMAL(10,2) | Total booking cost |

### Constraints

* Primary Key: `booking_id`
* Foreign Key: `user_id → Users(user_id)`
* Foreign Key: `match_id → Matches(match_id)`
* Total cost cannot be negative
* Allowed Payment Status:

  * Pending
  * Confirmed
  * Cancelled
  * Refunded

---

## Entity Relationship Summary

### Relationships

1. One User can have multiple Bookings.
2. One Match can have multiple Bookings.
3. Each Booking belongs to exactly one User and one Match.

Relationship Cardinality:

```
Users (1) -------- (M) Bookings (M) -------- (1) Matches
```

---

## Sample Data

### Users

* 4 sample users
* Includes both Football Fans and Ticket Managers

### Matches

* Champions League fixtures
* Premier League fixtures
* Serie A fixtures

### Bookings

* Confirmed bookings
* Pending bookings
* Records containing NULL payment status for testing queries

---

## SQL Queries Implemented

### Query 1

Retrieve all available Champions League matches.

```sql
SELECT match_id, fixture, base_ticket_price
FROM matches
WHERE tournament_category = 'Champions League'
AND match_status = 'Available';
```

---

### Query 2

Search users whose names start with "Tanvir" or contain "Haque".

```sql
SELECT user_id, full_name, email
FROM users
WHERE full_name ILIKE 'Tanvir%'
OR full_name ILIKE '%Haque%';
```

---

### Query 3

Display bookings with missing payment status and replace NULL values with "Action Required".

```sql
SELECT booking_id,
       user_id,
       match_id,
       COALESCE(payment_status, 'Action Required') AS systematic_status
FROM bookings
WHERE payment_status IS NULL;
```

---

### Query 4

Retrieve booking details with user names and match fixtures.

```sql
SELECT booking_id,
       full_name,
       fixture,
       total_cost
FROM users
INNER JOIN bookings USING(user_id)
INNER JOIN matches USING(match_id);
```

---

### Query 5

Display all users and their booking IDs, including users without bookings.

```sql
SELECT user_id,
       full_name,
       booking_id
FROM users
LEFT JOIN bookings USING(user_id);
```

---

### Query 6

Find bookings whose total cost is greater than the average booking cost.

```sql
SELECT booking_id,
       match_id,
       total_cost
FROM matches
INNER JOIN bookings USING(match_id)
WHERE total_cost >
      (SELECT AVG(total_cost)
       FROM bookings);
```

---

### Query 7

Retrieve the 2 most expensive matches while skipping the highest-priced match.

```sql
SELECT match_id,
       fixture,
       base_ticket_price
FROM matches
ORDER BY base_ticket_price DESC
LIMIT 2 OFFSET 1;
```

---

## Technologies Used

* PostgreSQL
* SQL (DDL & DML)
* Relational Database Design
* ER Modeling

---

## Learning Outcomes

This project demonstrates:

* Table creation using SQL DDL
* Primary Key and Foreign Key implementation
* Unique and Check Constraints
* Data insertion using SQL DML
* JOIN operations
* Subqueries
* Aggregate Functions
* NULL value handling using COALESCE
* Sorting, Filtering, and Pagination with ORDER BY, LIMIT, and OFFSET

---

## Author

Football Ticket Booking System Database Project

Developed as a relational database design and SQL query implementation exercise.
