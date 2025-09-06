# advanced-sql-practices
Advanced SQL Practices using PostgreSQL, MySQL and MS SQL Server

user activities table for storing daily attendance in PostgreSQL, MySQL, and SQL Server.

The schema includes fields for user ID, date, check-in/check-out timestamps, status, and optional metadata.

## User Activities Table Structure Overview

This table captures daily attendance logs for users across platforms like PostgreSQL, MySQL, and SQL Server.

## Table: `user_activities`

| Column Name     | Data Type           | Description                                      |
|-----------------|---------------------|--------------------------------------------------|
| `activity_id`   | `BIGINT` / `INT`    | Primary key, auto-incremented                   |
| `user_id`       | `VARCHAR(50)`       | Unique identifier for the user                  |
| `activity_date` | `DATE`              | Date of attendance                              |
| `check_in`      | `TIMESTAMP` / `DATETIME` | Check-in time                             |
| `check_out`     | `TIMESTAMP` / `DATETIME` | Check-out time                            |
| `status`        | `VARCHAR(20)`       | Attendance status (e.g., Present, Absent, Remote) |
| `remarks`       | `TEXT` / `NVARCHAR(MAX)` | Optional notes or metadata                |
| `created_at`    | `TIMESTAMP` / `DATETIME` | Record creation timestamp                  |
| `updated_at`    | `TIMESTAMP` / `DATETIME` | Last update timestamp                      |


## Notes

- `activity_id` is auto-incremented differently across databases:
  - PostgreSQL: `BIGSERIAL`
  - MySQL: `AUTO_INCREMENT`
  - SQL Server: `IDENTITY(1,1)`
- `updated_at` auto-update behavior:
  - MySQL: `ON UPDATE CURRENT_TIMESTAMP`
  - SQL Server: Requires a trigger for auto-update
- Consider adding a **UNIQUE constraint** on `(user_id, activity_date)` to prevent duplicates.
- Indexes on `user_id`, `activity_date`, and `status` can improve query performance.
- Foreign key constraints can link `user_id` to a `users` table for referential integrity.

## Use Cases

- Daily attendance tracking
- Shift monitoring and analytics
- Integration with HR or payroll systems

### PostgreSQL

    CREATE TABLE user_activities (
        activity_id BIGSERIAL PRIMARY KEY,
        user_id VARCHAR(50) NOT NULL,
        activity_date DATE NOT NULL,
        check_in TIMESTAMP,
        check_out TIMESTAMP,
        status VARCHAR(20),
        remarks TEXT,
        created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
        updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    );

### MySQL

    CREATE TABLE user_activities (
        activity_id BIGINT AUTO_INCREMENT PRIMARY KEY,
        user_id VARCHAR(50) NOT NULL,
        activity_date DATE NOT NULL,
        check_in DATETIME,
        check_out DATETIME,
        status VARCHAR(20),
        remarks TEXT,
        created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
        updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
    );

### MS SQL Server
    
    CREATE TABLE user_activities (
        activity_id BIGINT IDENTITY(1,1) PRIMARY KEY,
        user_id VARCHAR(50) NOT NULL,
        activity_date DATE NOT NULL,
        check_in DATETIME,
        check_out DATETIME,
        status VARCHAR(20),
        remarks NVARCHAR(MAX),
        created_at DATETIME DEFAULT GETDATE(),
        updated_at DATETIME DEFAULT GETDATE()
    );
