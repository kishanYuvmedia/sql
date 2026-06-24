-- Create Database
CREATE DATABASE company_management;
USE company_management;

-- =========================
-- COMPANY TABLE
-- =========================
CREATE TABLE companies (
    company_id INT AUTO_INCREMENT PRIMARY KEY,
    company_name VARCHAR(255) NOT NULL,
    company_email VARCHAR(255),
    company_phone VARCHAR(20),
    company_address TEXT,
    website VARCHAR(255),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- =========================
-- DEPARTMENT TABLE
-- =========================
CREATE TABLE departments (
    department_id INT AUTO_INCREMENT PRIMARY KEY,
    company_id INT NOT NULL,
    department_name VARCHAR(100) NOT NULL,
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (company_id) REFERENCES companies(company_id)
);

-- =========================
-- EMPLOYEE TABLE
-- =========================
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

-- =========================
-- CLIENT TABLE
-- =========================
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

-- =========================
-- PROJECT TABLE
-- =========================
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

-- =========================
-- PROJECT ASSIGNMENT TABLE
-- =========================
CREATE TABLE project_assignments (
    assignment_id INT AUTO_INCREMENT PRIMARY KEY,
    project_id INT NOT NULL,
    employee_id INT NOT NULL,
    role VARCHAR(100),
    assigned_date DATE,
    
    FOREIGN KEY (project_id) REFERENCES projects(project_id),
    FOREIGN KEY (employee_id) REFERENCES employees(employee_id)
);

-- =========================
-- PROJECT TASK TABLE
-- =========================
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
