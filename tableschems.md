# Company Management Database Schema

## Overview

This database schema is designed for a Company Management System that manages:

* Companies
* Departments
* Employees
* Clients
* Projects
* Project Assignments
* Project Tasks

---

# Database Creation

```sql
CREATE DATABASE company_management;
USE company_management;
```

---

# 1. Companies Table

```sql
CREATE TABLE companies (
    company_id INT AUTO_INCREMENT PRIMARY KEY,
    company_name VARCHAR(255) NOT NULL,
    company_email VARCHAR(255),
    company_phone VARCHAR(20),
    company_address TEXT,
    website VARCHAR(255),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## Purpose

Stores company information.

### Fields

| Column          | Type         | Description          |
| --------------- | ------------ | -------------------- |
| company_id      | INT          | Primary Key          |
| company_name    | VARCHAR(255) | Company Name         |
| company_email   | VARCHAR(255) | Company Email        |
| company_phone   | VARCHAR(20)  | Contact Number       |
| company_address | TEXT         | Address              |
| website         | VARCHAR(255) | Website URL          |
| created_at      | TIMESTAMP    | Record Creation Date |

---

# 2. Departments Table

```sql
CREATE TABLE departments (
    department_id INT AUTO_INCREMENT PRIMARY KEY,
    company_id INT NOT NULL,
    department_name VARCHAR(100) NOT NULL,
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (company_id) REFERENCES companies(company_id)
);
```

## Purpose

Stores company departments.

### Examples

* IT
* HR
* Marketing
* Finance
* Sales

---

# 3. Employees Table

```sql
CREATE TABLE employees (
    employee_id INT AUTO_INCREMENT PRIMARY KEY,
    department_id INT,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100),
    email VARCHAR(255) UNIQUE,
    phone VARCHAR(20),
    designation VARCHAR(100),
    salary DECIMAL(12,2),
    joining_date DATE,
    manager_id INT NULL,
    status ENUM('Active','Inactive') DEFAULT 'Active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    FOREIGN KEY (department_id) REFERENCES departments(department_id),
    FOREIGN KEY (manager_id) REFERENCES employees(employee_id)
);
```

## Purpose

Stores employee information and reporting hierarchy.

### Examples

* Project Manager
* Team Lead
* Full Stack Developer
* UI Developer
* QA Tester

---

# 4. Clients Table

```sql
CREATE TABLE clients (
    client_id INT AUTO_INCREMENT PRIMARY KEY,
    company_name VARCHAR(255) NOT NULL,
    contact_person VARCHAR(255),
    email VARCHAR(255),
    phone VARCHAR(20),
    address TEXT,
    website VARCHAR(255),
    status ENUM('Active','Inactive') DEFAULT 'Active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## Purpose

Stores client details.

---

# 5. Projects Table

```sql
CREATE TABLE projects (
    project_id INT AUTO_INCREMENT PRIMARY KEY,
    client_id INT NOT NULL,
    project_name VARCHAR(255) NOT NULL,
    project_code VARCHAR(50) UNIQUE,
    description TEXT,
    start_date DATE,
    end_date DATE,
    budget DECIMAL(15,2),
    status ENUM(
        'Planning',
        'In Progress',
        'Completed',
        'On Hold',
        'Cancelled'
    ) DEFAULT 'Planning',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    FOREIGN KEY (client_id) REFERENCES clients(client_id)
);
```

## Purpose

Stores project information.

### Example Status

* Planning
* In Progress
* Completed
* On Hold
* Cancelled

---

# 6. Project Assignments Table

```sql
CREATE TABLE project_assignments (
    assignment_id INT AUTO_INCREMENT PRIMARY KEY,
    project_id INT NOT NULL,
    employee_id INT NOT NULL,
    role VARCHAR(100),
    assigned_date DATE,

    FOREIGN KEY (project_id) REFERENCES projects(project_id),
    FOREIGN KEY (employee_id) REFERENCES employees(employee_id)
);
```

## Purpose

Assigns employees to projects.

### Example Roles

* Project Manager
* Team Lead
* Developer
* Tester
* Designer

---

# 7. Project Tasks Table

```sql
CREATE TABLE project_tasks (
    task_id INT AUTO_INCREMENT PRIMARY KEY,
    project_id INT NOT NULL,
    employee_id INT,
    task_name VARCHAR(255) NOT NULL,
    description TEXT,
    start_date DATE,
    due_date DATE,
    status ENUM(
        'Pending',
        'In Progress',
        'Completed',
        'Cancelled'
    ) DEFAULT 'Pending',

    FOREIGN KEY (project_id) REFERENCES projects(project_id),
    FOREIGN KEY (employee_id) REFERENCES employees(employee_id)
);
```

## Purpose

Stores project tasks and employee assignments.

---

# Entity Relationship Diagram (ERD)

```text
COMPANIES
    |
    └── DEPARTMENTS
            |
            └── EMPLOYEES
                    |
                    └── MANAGER (SELF REFERENCE)

CLIENTS
    |
    └── PROJECTS
            |
            ├── PROJECT_ASSIGNMENTS
            │        |
            │        └── EMPLOYEES
            |
            └── PROJECT_TASKS
                     |
                     └── EMPLOYEES
```

---

# Sample Queries

## Employee With Department

```sql
SELECT
    e.employee_id,
    CONCAT(e.first_name,' ',e.last_name) AS employee_name,
    d.department_name,
    e.designation
FROM employees e
JOIN departments d
ON e.department_id = d.department_id;
```

## Project With Client

```sql
SELECT
    p.project_name,
    c.company_name AS client_name,
    p.status
FROM projects p
JOIN clients c
ON p.client_id = c.client_id;
```

## Employees Assigned To Projects

```sql
SELECT
    p.project_name,
    CONCAT(e.first_name,' ',e.last_name) AS employee_name,
    pa.role
FROM project_assignments pa
JOIN projects p ON pa.project_id = p.project_id
JOIN employees e ON pa.employee_id = e.employee_id;
```

---

# Future Enhancements

* Attendance Management
* Leave Management
* Payroll Management
* Timesheet Tracking
* Project Milestones
* Sprint Management
* CRM Module
* Invoice Management
* Asset Management
* Document Management

---
