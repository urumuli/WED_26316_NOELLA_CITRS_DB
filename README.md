# Campus Issue Tracking and Resolution System (CITRS)

## 📋 Project Information

**Student Name:** Urumuri Gasana Marie Noella  
**Student ID:** 26326  
**Group:** Wednesday  
**Course:** Database Development with PL/SQL (INSY 8311)  
**Lecturer:** Eric Maniraguha  
**Institution:** Adventist University of Central Africa (AUCA)  
**Academic Year:** 2025-2026 | Semester I  
**Submission Date:** December 7, 2025

---

## 📖 Table of Contents

1. [Project Overview](#project-overview)
2. [Problem Statement](#problem-statement)
3. [System Architecture](#system-architecture)
4. [Database Design](#database-design)
5. [Installation Guide](#installation-guide)
6. [How to Use](#how-to-use)
7. [Testing Instructions](#testing-instructions)
8. [Project Structure](#project-structure)
9. [Technologies Used](#technologies-used)
10. [Screenshots](#screenshots)

---

## 🎯 Project Overview

The **Campus Issue Tracking and Resolution System (CITRS)** is a database-driven solution designed to manage and track student complaints at AUCA. The system replaces the traditional paper-based complaint process with a digital, automated PL/SQL solution that ensures transparency, accountability, and faster resolution times.

### Key Features:
- ✅ Digital complaint submission by students
- ✅ Automatic status tracking using triggers
- ✅ Admin dashboard for viewing and resolving complaints
- ✅ Comprehensive reporting and analytics
- ✅ Complete audit trail of all changes
- ✅ Real-time status updates

---

## 🔍 Problem Statement

### Current Situation:
At AUCA, students submit complaints about academic, facility, or discipline issues using paper forms. This manual process has several problems:

- **Lost Complaints:** Paper forms often get misplaced or lost
- **No Tracking:** Students cannot check the status of their complaints
- **Slow Response:** Administrators forget or delay responding to complaints
- **No Analytics:** University management cannot identify recurring issues or trends
- **Poor Communication:** No systematic way to notify students about resolutions

### Our Solution:
CITRS provides a digital platform where:
- Students submit complaints online 24/7
- System automatically assigns unique IDs and tracks status
- Administrators receive organized complaint lists
- Automatic status updates when actions are taken
- Management gets detailed reports and analytics

### Expected Benefits:
- 📉 Reduce complaint resolution time by **60%**
- 📊 Enable **data-driven decision making** for university management
- 🎯 Improve **student satisfaction** through transparency
- 🔄 Create **accountability** with complete audit trails
- 📈 Identify **systemic issues** before they escalate

---

## 🏗️ System Architecture

### Business Process Flow:

```
STUDENT → Submit Complaint
    ↓
SYSTEM → Validate Data → Generate ID → Set Status="Pending"
    ↓
DATABASE → Store Complaint
    ↓
ADMINISTRATOR → View Dashboard → Select Complaint → Take Action
    ↓
SYSTEM → Trigger Updates Status → Record Action
    ↓
DATABASE → Update Status="Resolved" → Log Audit Trail
    ↓
STUDENT → View Resolution
```

### System Components:

1. **Data Layer:** Oracle Database 21c with 4 normalized tables
2. **Logic Layer:** PL/SQL procedures, functions, triggers, and packages
3. **Business Rules:** Automated workflows using triggers
4. **Security Layer:** User authentication and role-based access
5. **Reporting Layer:** Analytics queries and dashboard views

---

## 🗄️ Database Design

### Entity-Relationship Model:

The system uses **4 main tables** designed in **Third Normal Form (3NF)**:

#### 1. STUDENTS Table
Stores student information who submit complaints.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| Student_ID | NUMBER(10) | PRIMARY KEY | Unique student identifier |
| Name | VARCHAR2(100) | NOT NULL | Full name of student |
| Department | VARCHAR2(50) | NOT NULL | Academic department |
| Email | VARCHAR2(100) | UNIQUE, NOT NULL | University email |
| Phone | VARCHAR2(20) | NULL | Contact phone number |
| Registration_Date | DATE | DEFAULT SYSDATE | Date student registered |

#### 2. COMPLAINT_CATEGORY Table
Defines types of complaints (lookup table).

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| Category_ID | NUMBER(5) | PRIMARY KEY | Unique category identifier |
| Category_Name | VARCHAR2(50) | UNIQUE, NOT NULL | Category name |
| Description | VARCHAR2(200) | NULL | Category description |

**Categories:**
1. Academic (courses, exams, grades)
2. Facilities (infrastructure, equipment)
3. Discipline (behavioral issues)
4. Finance (fees, scholarships)
5. Technology (IT services, internet)

#### 3. COMPLAINTS Table
Main transaction table storing all student complaints.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| Complaint_ID | NUMBER(10) | PRIMARY KEY | Unique complaint identifier |
| Student_ID | NUMBER(10) | FOREIGN KEY, NOT NULL | Student who submitted |
| Category_ID | NUMBER(5) | FOREIGN KEY, NOT NULL | Type of complaint |
| Description | VARCHAR2(500) | NOT NULL | Detailed description |
| Date_Submitted | DATE | DEFAULT SYSDATE | Submission date |
| Status | VARCHAR2(20) | CHECK constraint | Current status |
| Priority | VARCHAR2(10) | CHECK constraint | Urgency level |

**Valid Statuses:** Pending, In Progress, Resolved, Closed  
**Valid Priorities:** Low, Normal, High, Urgent

#### 4. ADMIN_ACTIONS Table
Records all actions taken by administrators.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| Action_ID | NUMBER(10) | PRIMARY KEY | Unique action identifier |
| Complaint_ID | NUMBER(10) | FOREIGN KEY, NOT NULL | Related complaint |
| Admin_Name | VARCHAR2(100) | NOT NULL | Administrator name |
| Action_Taken | VARCHAR2(500) | NOT NULL | Description of solution |
| Date_Resolved | DATE | DEFAULT SYSDATE | Action date |
| Notes | VARCHAR2(500) | NULL | Additional notes |

#### 5. COMPLAINT_AUDIT_LOG Table
Tracks all changes for audit purposes.

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| Audit_ID | NUMBER | PRIMARY KEY | Unique audit entry |
| Complaint_ID | NUMBER | NULL | Related complaint |
| Changed_By | VARCHAR2(100) | NULL | User who made change |
| Change_Date | DATE | DEFAULT SYSDATE | When change occurred |
| Old_Status | VARCHAR2(20) | NULL | Previous status |
| New_Status | VARCHAR2(20) | NULL | New status |
| Action | VARCHAR2(50) | NULL | Type of action |

### Database Relationships:

- **STUDENTS** (1) → (M) **COMPLAINTS** (One student can submit many complaints)
- **COMPLAINT_CATEGORY** (1) → (M) **COMPLAINTS** (One category has many complaints)
- **COMPLAINTS** (1) → (M) **ADMIN_ACTIONS** (One complaint can have multiple follow-up actions)

### Indexes Created:
- `idx_complaints_student` on COMPLAINTS(Student_ID)
- `idx_complaints_category` on COMPLAINTS(Category_ID)
- `idx_complaints_status` on COMPLAINTS(Status)
- `idx_complaints_date` on COMPLAINTS(Date_Submitted)
- `idx_admin_actions_complaint` on ADMIN_ACTIONS(Complaint_ID)
- `idx_students_dept` on STUDENTS(Department)

  ![here are ERdiagram for table]()

---

## 📥 Installation Guide

### Prerequisites:
- Oracle Database 21c (or higher)
- Oracle SQL Developer (recommended) OR SQL*Plus
- At least 500MB free disk space
- Basic knowledge of SQL and PL/SQL

### Step-by-Step Installation:

#### **Option 1: Using Oracle SQL Developer (Recommended)**

1. **Open Oracle SQL Developer**
2. **Create New Connection:**
   - Connection Name: `CITRS_Project`
   - Username: `SYSTEM` (or your admin username)
   - Password: (your password)
   - Hostname: `localhost`
   - Port: `1521`
   - SID/Service Name: Use your database name
3. **Test connection** and click **Connect**
4. **Open SQL File:**
   - Click File → Open
   - Select `Complete_CITRS_Database.sql`
5. **Run Script:**
   - Click the **Run Script** button (F5)
   - Wait 1-2 minutes for completion
6. **Verify Installation:**
   - Run: `SELECT COUNT(*) FROM STUDENTS;`
   - Should show: 25 students

#### **Option 2: Using SQL*Plus Command Line**

```cmd
# 1. Open Command Prompt (Windows) or Terminal (Mac/Linux)

# 2. Connect as SYSDBA
C:\> sqlplus / as sysdba

# 3. If using pluggable database, switch to it
SQL> ALTER SESSION SET CONTAINER = WED_26316_NOELLA_CITRS_DB;

# 4. Run the script
SQL> @C:\path\to\Complete_CITRS_Database.sql

# 5. Verify
SQL> SELECT COUNT(*) FROM STUDENTS;
```

### Database Configuration (Phase IV):

**Pluggable Database Details:**
- **Database Name:** WED_26316_NOELLA_CITRS_DB
- **Admin User:** URUMULI
- **Password:** Noella2025
- **Location:** F:\APP\URUMURI\ORADATA\ORCL\WED_26316_NOELLA_CITRS_DB\

**Privileges Granted:**
- CREATE SESSION
- CREATE TABLE
- CREATE SEQUENCE
- CREATE PROCEDURE
- CREATE TRIGGER
- CREATE VIEW
- DBA (full access)

---

## 🚀 How to Use

### For Students:

#### Submit a New Complaint:
```sql
SET SERVEROUTPUT ON;

DECLARE
    v_complaint_id NUMBER;
BEGIN
    submit_complaint(
        p_student_id => 26326,
        p_category_id => 1,  -- 1=Academic, 2=Facilities, 3=Discipline, 4=Finance, 5=Technology
        p_description => 'Need extension for database project due to technical issues',
        p_priority => 'High',  -- Low, Normal, High, Urgent
        p_complaint_id => v_complaint_id
    );
    DBMS_OUTPUT.PUT_LINE('Your complaint ID is: ' || v_complaint_id);
END;
/
```

#### Check Your Complaints:
```sql
SELECT Complaint_ID, Description, Status, Priority, Date_Submitted
FROM COMPLAINTS
WHERE Student_ID = 26326
ORDER BY Date_Submitted DESC;
```

#### Check Complaint Status:
```sql
SELECT get_complaint_status(1000) AS Status FROM DUAL;
```

### For Administrators:

#### View All Pending Complaints:
```sql
SELECT c.Complaint_ID, s.Name, s.Department, cat.Category_Name,
       c.Description, c.Priority, c.Date_Submitted
FROM COMPLAINTS c
JOIN STUDENTS s ON c.Student_ID = s.Student_ID
JOIN COMPLAINT_CATEGORY cat ON c.Category_ID = cat.Category_ID
WHERE c.Status = 'Pending'
ORDER BY c.Priority DESC, c.Date_Submitted ASC;
```

#### Resolve a Complaint:
```sql
BEGIN
    resolve_complaint(
        p_complaint_id => 1000,
        p_admin_name => 'Eric Maniraguha',
        p_action_taken => 'Extended project deadline by 3 days after reviewing documentation',
        p_notes => 'Valid reason provided with medical certificate'
    );
END;
/
```

#### View Complaints by Department:
```sql
BEGIN
    list_complaints_by_department('Computer Science');
END;
/
```

#### Generate Department Report:
```sql
BEGIN
    generate_department_report();
END;
/
```

### For Management:

#### View Overall Statistics:
```sql
SELECT 
    (SELECT COUNT(*) FROM STUDENTS) AS Total_Students,
    (SELECT COUNT(*) FROM COMPLAINTS) AS Total_Complaints,
    (SELECT COUNT(*) FROM COMPLAINTS WHERE Status = 'Pending') AS Pending,
    (SELECT COUNT(*) FROM COMPLAINTS WHERE Status = 'Resolved') AS Resolved,
    (SELECT ROUND(AVG(Date_Resolved - Date_Submitted), 2) 
     FROM ADMIN_ACTIONS aa 
     JOIN COMPLAINTS c ON aa.Complaint_ID = c.Complaint_ID) AS Avg_Resolution_Days
FROM DUAL;
```

#### View Complaints by Category:
```sql
SELECT cat.Category_Name, 
       COUNT(*) AS Total_Complaints,
       SUM(CASE WHEN c.Status = 'Resolved' THEN 1 ELSE 0 END) AS Resolved,
       SUM(CASE WHEN c.Status = 'Pending' THEN 1 ELSE 0 END) AS Pending
FROM COMPLAINTS c
JOIN COMPLAINT_CATEGORY cat ON c.Category_ID = cat.Category_ID
GROUP BY cat.Category_Name
ORDER BY Total_Complaints DESC;
```

---

## 🧪 Testing Instructions

### 1. Test Tables Created Successfully:
```sql
SELECT table_name FROM user_tables 
WHERE table_name IN ('STUDENTS', 'COMPLAINTS', 'COMPLAINT_CATEGORY', 'ADMIN_ACTIONS', 'COMPLAINT_AUDIT_LOG');
```
**Expected Result:** All 5 tables should appear

### 2. Test Data Insertion:
```sql
SELECT COUNT(*) AS Students FROM STUDENTS;
SELECT COUNT(*) AS Complaints FROM COMPLAINTS;
SELECT COUNT(*) AS Categories FROM COMPLAINT_CATEGORY;
```
**Expected Results:**
- Students: 25
- Complaints: 25
- Categories: 5

### 3. Test Procedure: Submit Complaint
```sql
SET SERVEROUTPUT ON;

DECLARE
    v_id NUMBER;
BEGIN
    submit_complaint(
        p_student_id => 26326,
        p_category_id => 1,
        p_description => 'Test complaint for system validation',
        p_priority => 'Normal',
        p_complaint_id => v_id
    );
    DBMS_OUTPUT.PUT_LINE('New Complaint ID: ' || v_id);
END;
/
```
**Expected Output:** "Complaint submitted successfully ID: [number]"

### 4. Test Procedure: Resolve Complaint
```sql
BEGIN
    resolve_complaint(
        p_complaint_id => 1000,
        p_admin_name => 'Test Admin',
        p_action_taken => 'Test resolution for validation'
    );
END;
/
```
**Expected Output:** "Complaint resolved successfully"

### 5. Test Trigger: Auto-Status Update
```sql
-- First, check current status
SELECT Complaint_ID, Status FROM COMPLAINTS WHERE Complaint_ID = 1001;

-- Insert admin action (this should trigger status update)
INSERT INTO ADMIN_ACTIONS VALUES (
    seq_action_id.NEXTVAL, 
    1001, 
    'Test Admin', 
    'Testing automatic status update',
    SYSDATE,
    'Trigger test'
);

-- Check status again (should be 'Resolved')
SELECT Complaint_ID, Status FROM COMPLAINTS WHERE Complaint_ID = 1001;
```
**Expected Result:** Status changes from 'Pending' to 'Resolved' automatically

### 6. Test Functions:
```sql
-- Count unresolved complaints
SELECT count_unresolved_complaints() AS Unresolved FROM DUAL;

-- Count complaints by category
SELECT count_complaints_by_category(1) AS Academic_Complaints FROM DUAL;

-- Get student complaint count
SELECT get_student_complaint_count(26326) AS My_Complaints FROM DUAL;

-- Average resolution time
SELECT avg_resolution_time() AS Avg_Days FROM DUAL;
```

### 7. Test Audit Log:
```sql
SELECT * FROM COMPLAINT_AUDIT_LOG ORDER BY Change_Date DESC;
```
**Expected Result:** Should show all status changes and new complaint creations

### 8. Test Cursor Procedure:
```sql
SET SERVEROUTPUT ON;
BEGIN
    list_complaints_by_department('Computer Science');
END;
/
```
**Expected Output:** List of all complaints from Computer Science department

---

## 📂 Project Structure

```
CITRS_Project/
│
├── README.md                          # This file
│
├── Phase_I_Problem_Statement/
│   └── 26326_Urumuri_CITRS_Presentation.pptx
│
├── Phase_II_Business_Process/
│   ├── 26326_Urumuri_BusinessProcess_Diagram.pdf
│   ├── 26326_Urumuri_Process_Explanation.pdf
│   └── Lucidchart_Instructions.md
│
├── Phase_III_Logical_Design/
│   ├── 26326_Urumuri_ER_Diagram.pdf
│   ├── 26326_Urumuri_DataDictionary.pdf
│   └── Normalization_Documentation.pdf
│
├── Phase_IV_Database_Creation/
│   ├── Database_Creation_Script.sql
│   ├── Tablespace_Configuration.sql
│   └── User_Setup.sql
│
├── Phase_V_VI_VII_Implementation/
│   ├── Complete_CITRS_Database.sql    # MAIN FILE - Run this!
│   ├── 01_Create_Tables.sql
│   ├── 02_Insert_Data.sql
│   ├── 03_Create_Procedures.sql
│   ├── 04_Create_Functions.sql
│   ├── 05_Create_Triggers.sql
│   └── 06_Test_Queries.sql
│
├── Phase_VIII_Documentation/
│   ├── Final_Presentation.pptx
│   ├── Technical_Documentation.pdf
│   ├── User_Manual.pdf
│   └── BI_Dashboard_Mockups.pdf
│
├── Screenshots/
│   ├── 01_Database_Structure.png
│   ├── 02_Tables_With_Data.png
│   ├── 03_Procedure_Execution.png
│   ├── 04_Trigger_Demo.png
│   ├── 05_Reports_Dashboard.png
│   └── 06_Audit_Log.png
│
└── Testing/
    ├── Test_Results.txt
    ├── Performance_Tests.sql
    └── Validation_Checklist.pdf
```

---

## 🛠️ Technologies Used

### Database:
- **Oracle Database 21c Enterprise Edition**
- **PL/SQL** (Procedural Language extension to SQL)

### Development Tools:
- **Oracle SQL Developer** (for database development)
- **SQL*Plus** (command-line interface)
- **Lucidchart** (for creating business process diagrams)
- **draw.io** (alternative for ER diagrams)
- **Microsoft PowerPoint** (for presentations)

### Key PL/SQL Features Implemented:

1. **Stored Procedures (5 total):**
   - `submit_complaint` - Register new complaints
   - `resolve_complaint` - Record resolution actions
   - `update_complaint_status` - Manually update status
   - `list_complaints_by_department` - Department-specific reports
   - `generate_department_report` - Statistical analysis

2. **Functions (5 total):**
   - `count_unresolved_complaints` - Count pending/in-progress complaints
   - `count_complaints_by_category` - Category-wise statistics
   - `get_student_complaint_count` - Individual student metrics
   - `avg_resolution_time` - Performance metric calculation
   - `get_complaint_status` - Quick status lookup

3. **Triggers (4 total):**
   - `trg_auto_resolve` - Automatically update status when admin acts
   - `trg_validate_complaint` - Data validation before insertion
   - `trg_audit_changes` - Log all status changes
   - `trg_log_new` - Log new complaint creation

4. **Cursors:**
   - Explicit cursors in procedures for multi-row processing
   - Used in `list_complaints_by_department` and `generate_department_report`

5. **Sequences:**
   - `seq_complaint_id` - Auto-increment for complaints
   - `seq_action_id` - Auto-increment for admin actions
   - `seq_audit_id` - Auto-increment for audit entries

6. **Exception Handling:**
   - Custom error messages with `RAISE_APPLICATION_ERROR`
   - Transaction rollback on errors
   - Proper error logging

---

## 📸 Screenshots

### Required Screenshots (Include in Screenshots folder):

1. **Database Structure**
   - Screenshot showing all 5 tables in SQL Developer
   - Object Browser view

2. **ER Diagram**
   - Complete entity-relationship diagram
   - Show relationships between tables

3. **Sample Data**
   - SELECT * FROM STUDENTS (show 10 rows)
   - SELECT * FROM COMPLAINTS (show 10 rows)
   - Demonstrate foreign key relationships

4. **Procedure Execution**
   - Show `submit_complaint` running successfully
   - Display output message with new complaint ID

5. **Trigger in Action**
   - Before: Complaint status = 'Pending'
   - Insert into ADMIN_ACTIONS
   - After: Complaint status automatically = 'Resolved'

6. **Reports/Analytics**
   - Department-wise complaint summary
   - Category distribution chart
   - Resolution time statistics

7. **Audit Log**
   - Show COMPLAINT_AUDIT_LOG entries
   - Demonstrate complete trail of changes

### How to Take Screenshots:

**In SQL Developer:**
1. Run your query
2. Press `Alt + PrtScn` (Windows) or `Cmd + Shift + 4` (Mac)
3. Paste into Word/PowerPoint
4. Add caption explaining what it shows
5. Save as PDF

---

## 📊 Database Statistics

- **Total Tables:** 5 (STUDENTS, COMPLAINTS, COMPLAINT_CATEGORY, ADMIN_ACTIONS, COMPLAINT_AUDIT_LOG)
- **Total Procedures:** 5
- **Total Functions:** 5
- **Total Triggers:** 4
- **Total Sequences:** 3
- **Total Indexes:** 6
- **Sample Data:** 25 students, 25 complaints, 5 categories, 6 admin actions

---

## 🎯 Learning Outcomes Achieved

Through this project, I successfully demonstrated:

1. ✅ **Database Design:** Created normalized database (3NF) with proper relationships
2. ✅ **PL/SQL Programming:** Wrote procedures, functions, triggers with exception handling
3. ✅ **Data Manipulation:** Implemented complex queries with joins and aggregations
4. ✅ **Business Logic:** Automated workflows using triggers
5. ✅ **Data Integrity:** Applied constraints (PK, FK, CHECK, UNIQUE)
6. ✅ **Performance Optimization:** Created strategic indexes
7. ✅ **Security:** Implemented user authentication and privileges
8. ✅ **Audit Trail:** Maintained complete change history
9. ✅ **Reporting:** Built analytical queries for decision support
10. ✅ **Documentation:** Comprehensive technical and user documentation

---

## 🔮 Future Enhancements

Potential improvements for next version:

1. **Web Interface:** Build web application using Oracle APEX
2. **Email Notifications:** Send automatic emails when complaints are resolved
3. **Mobile App:** Android/iOS app for students
4. **File Attachments:** Allow students to upload supporting documents
5. **Priority Queue:** Implement automatic priority assignment based on keywords
6. **SLA Tracking:** Monitor and enforce service level agreements
7. **Anonymous Complaints:** Option for students to submit anonymously
8. **Multi-language Support:** Add Kinyarwanda and French interfaces
9. **Advanced Analytics:** Implement predictive analytics for trend detection
10. **Integration:** Connect with existing student information system

---

## 📞 Contact Information

**Student:** Urumuri Gasana Marie Noella  
**Email:** urumuri@auca.ac.rw  
**Phone:** 0788123456  
**Student ID:** 26326  
**Department:** Computer Science  
**Institution:** Adventist University of Central Africa (AUCA)

**Lecturer:** Eric Maniraguha  
**Email:** eric.maniraguha@auca.ac.rw

---

## 📝 License and Academic Integrity

This project was completed as part of the Database Development with PL/SQL course (INSY 8311) at AUCA. All code is original work by Urumuri Gasana Marie Noella. This project is for educational purposes only.

**Academic Integrity Statement:**  
I declare that this project is my own work. All sources used have been properly cited. I have not plagiarized or copied from other students or external sources without proper attribution.

**Signature:** Urumuri Gasana Marie Noella  
**Date:** December 7, 2025

---

## 🙏 Acknowledgments

Special thanks to:
- **Eric Maniraguha** - Course lecturer for guidance and support
- **AUCA** - For providing the learning environment and resources
- **Oracle Corporation** - For the excellent database software
- **My Classmates** - For collaborative learning and peer support

---

## 📚 References

1. Oracle Corporation. (2021). *Oracle Database 21c Documentation*. Retrieved from https://docs.oracle.com/en/database/oracle/oracle-database/21/
2. Feuerstein, S., & Pribyl, B. (2021). *Oracle PL/SQL Programming* (7th ed.). O'Reilly Media.
3. Connolly, T., & Begg, C. (2015). *Database Systems: A Practical Approach to Design, Implementation, and Management* (6th ed.). Pearson Education.
4. Elmasri, R., & Navathe, S. B. (2016). *Fundamentals of Database Systems* (7th ed.). Pearson.
5. Date, C. J. (2019). *Database Design and Relational Theory: Normal Forms and All That Jazz* (2nd ed.). Apress.

---

## ⚡ Quick Start Guide

### For Instructors Evaluating This Project:

1. **Open Oracle SQL Developer**
2. **Connect to database** using admin credentials
3. **Open file:** `Phase_V_VI_VII_Implementation/Complete_CITRS_Database.sql`
4. **Run script:** Press F5 or click "Run Script" button
5. **Wait 1-2 minutes** for completion
6. **Verify:** Run `SELECT COUNT(*) FROM STUDENTS;` (should show 25)
7. **Test procedures:** Run test queries from `06_Test_Queries.sql`
8. **Review screenshots:** Check `Screenshots/` folder for visual documentation

---

**Last Updated:** December 5, 2025  
**Version:** 1.0  
**Status:** ✅ Complete and Ready for Submission

---

*"Whatever you do, work at it with all your heart, as working for the Lord, not for human masters." — Colossians 3:23 (NIV)*
