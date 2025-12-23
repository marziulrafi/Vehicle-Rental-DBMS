# 🚗 Vehicle Rental System – Database Design & SQL Queries

## 📌 Project Overview
This project represents a **Vehicle Rental System** database designed to demonstrate:
- Proper **ERD design** with correct relationships
- Use of **Primary Keys (PK)** and **Foreign Keys (FK)**
- Writing SQL queries using **JOIN, EXISTS, WHERE, GROUP BY, HAVING**

The system manages **Users**, **Vehicles**, and **Bookings**, following real-world rental business logic.

---

## 🧩 Entity Relationship Diagram (ERD)

The ERD includes :
- Users → Bookings (**One-to-Many**)
- Vehicles → Bookings (**One-to-Many**)
- Each booking is linked to **exactly one user and one vehicle** (logical 1:1)

🔗 **ERD Diagram :**  
[Click here to view the ERD](https://drawsql.app/teams/marziul/diagrams/vehicle-rental-system)

---

## 🗄️ Database Tables Summary

### Users
- Stores customer and admin information
- Attributes include: name, email, phone, password, role
- Email is unique for each user
- Identified by a primary key (`user_id`)

### Vehicles
- Stores rental vehicle information
- Attributes include: name, type, model, registration number, rental price per day
- Registration number is unique
- Tracks availability status (available / rented / maintenance)
- Identified by a primary key (`vehicle_id`)

### Bookings
- Stores rental booking records
- Links users and vehicles using foreign keys (`user_id`, `vehicle_id`)
- Attributes include: start date, end date, booking status, total cost
- Each booking is associated with exactly one user and one vehicle
- Identified by a primary key (`booking_id`)


---

## 📄 `queries.sql` – SQL Queries & Explanations

### Query 1 : JOIN
**Purpose :**  
Retrieve booking details along with the **customer name** and **vehicle name**.

**Concepts Used :**  
`INNER JOIN`

```sql
SELECT 
    b.booking_id,
    u.name AS customer_name,
    v.name AS vehicle_name,
    b.start_date,
    b.end_date,
    b.status
FROM bookings b
INNER JOIN users u ON b.user_id = u.user_id
INNER JOIN vehicles v ON b.vehicle_id = v.vehicle_id;
```

### Query 2 : EXISTS
**Purpose :**  
Find all vehicles that have **never been booked**.

**Concepts Used :**  
`NOT EXISTS`

```sql
SELECT *
FROM vehicles v
WHERE NOT EXISTS (
    SELECT 1
    FROM bookings b
    WHERE b.vehicle_id = v.vehicle_id
);
```

### Query 3 : WHERE
**Purpose :**  
Retrieve all available vehicles of a **specific type** (example: cars).

**Concepts Used :**  
`SELECT`, `WHERE`

```sql
SELECT *
FROM vehicles
WHERE status = 'available'
  AND type = 'car';
```

### Query 4 : GROUP BY & HAVING
**Purpose:**  
Find vehicles that have been booked **more than 2 times**.

**Concepts Used:**  
`GROUP BY`, `HAVING`, `COUNT`

```sql
SELECT 
    v.name AS vehicle_name,
    COUNT(b.booking_id) AS total_bookings
FROM vehicles v
INNER JOIN bookings b ON v.vehicle_id = b.vehicle_id
GROUP BY v.vehicle_id, v.name
HAVING COUNT(b.booking_id) > 2;
```

