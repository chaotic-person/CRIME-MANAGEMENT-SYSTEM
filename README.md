# 🚓 Crime Management System

A web-based Crime Management System designed to help law enforcement handle criminal records in a structured and digital way. Built using **PHP**, **MySQL**, and run on **XAMPP**, this project replaces outdated paper-based processes with a modern interface for admins, officers, and general users.

---

## 🛠 Tech Stack

- **Frontend & Backend:** PHP  
- **Database:** MySQL  
- **Local Server Environment:** XAMPP (Apache + MySQL)

---

## 👥 User Roles

This system supports three main roles:

### 👑 Admin
- Manage login credentials and users  
- Assign officers to police stations

### 👮 Police Officers
- File FIRs and manage case data  
- Record details of victims and accused  
- Add court hearing info and prison records

### 🙋 General Users
- Log in to check the status of their cases

---

## 🔑 Key Features

- ✅ Role-based login and dashboard views  
- 📝 FIR management with case status tracking  
- 🧾 Court and prison record modules  
- 🗃️ Relational database with foreign key constraints  
- 🔐 Secure session management  
- 📂 Easy data retrieval and updates  
- 🧩 Scalable and modular design

---

## 🧱 Database Setup

### Step 1: Create a New Database

1. Open `phpMyAdmin`  
2. Click on **New**, name your database (e.g., `crime_db`)  
3. Make sure to use this database name in your PHP files (`config.php` or `db.php`)

### Step 2: Create Tables

Paste the following SQL into the SQL tab of phpMyAdmin to set up your tables:

<details>
<summary>📄 Click to view full SQL schema</summary>

```sql
-- 1. PoliceOfficers Table
CREATE TABLE PoliceOfficers (
    officer_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    badge_number VARCHAR(50) UNIQUE NOT NULL,
    rank VARCHAR(50),
    police_station VARCHAR(100),
    contact_number VARCHAR(20)
);

-- 2. Users Table
CREATE TABLE Users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role ENUM('Admin', 'Officer', 'User') DEFAULT 'Officer',
    officer_id INT,
    FOREIGN KEY (officer_id) REFERENCES PoliceOfficers(officer_id)
);

-- 3. FIR Table
CREATE TABLE FIR (
    fir_id INT PRIMARY KEY AUTO_INCREMENT,
    fir_number VARCHAR(50) UNIQUE NOT NULL,
    date_filed DATE NOT NULL,
    filed_by_officer_id INT,
    complaint_text TEXT,
    location VARCHAR(255),
    status ENUM('Registered', 'Under Investigation', 'Closed') DEFAULT 'Registered',
    FOREIGN KEY (filed_by_officer_id) REFERENCES PoliceOfficers(officer_id)
);

-- 4. Victims Table
CREATE TABLE Victims (
    victim_id INT PRIMARY KEY AUTO_INCREMENT,
    fir_id INT,
    name VARCHAR(100),
    age INT,
    gender VARCHAR(10),
    address VARCHAR(255),
    contact_number VARCHAR(20),
    FOREIGN KEY (fir_id) REFERENCES FIR(fir_id)
);

-- 5. Accused Table
CREATE TABLE Accused (
    accused_id INT PRIMARY KEY AUTO_INCREMENT,
    fir_id INT,
    name VARCHAR(100),
    age INT,
    gender VARCHAR(10),
    address VARCHAR(255),
    status ENUM('At Large', 'Arrested', 'Released on Bail', 'Convicted'),
    FOREIGN KEY (fir_id) REFERENCES FIR(fir_id)
);

-- 6. CourtDetails Table
CREATE TABLE CourtDetails (
    court_id INT PRIMARY KEY AUTO_INCREMENT,
    fir_id INT,
    court_name VARCHAR(100),
    hearing_date DATE,
    verdict TEXT,
    case_status ENUM('Pending', 'In Trial', 'Closed'),
    FOREIGN KEY (fir_id) REFERENCES FIR(fir_id)
);

-- 7. PrisonDetails Table
CREATE TABLE PrisonDetails (
    prison_id INT PRIMARY KEY AUTO_INCREMENT,
    accused_id INT,
    prison_name VARCHAR(100),
    sentence_years INT,
    date_of_entry DATE,
    release_date DATE,
    cell_number VARCHAR(50),
    FOREIGN KEY (accused_id) REFERENCES Accused(accused_id)
);

