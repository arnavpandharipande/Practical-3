CREATE DATABASE college_demo; USE college_demo; show tables; desc course; desc department; desc enrollment; desc student; desc faculty; CREATE TABLE department ( dept_id INT PRIMARY KEY, dept_name VARCHAR(50) UNIQUE NOT NULL );

CREATE TABLE student ( roll_no INT PRIMARY KEY, name VARCHAR(50) NOT NULL, email VARCHAR(50) UNIQUE, aadhar_no VARCHAR(12) UNIQUE, dept_id INT, FOREIGN KEY (dept_id) REFERENCES department(dept_id) );

CREATE TABLE course ( course_id INT PRIMARY KEY, course_name VARCHAR(50) NOT NULL, dept_id INT, FOREIGN KEY (dept_id) REFERENCES department(dept_id) );

CREATE TABLE enrollment ( roll_no INT, course_id INT, semester INT CHECK (semester BETWEEN 1 AND 8), grade CHAR(2), PRIMARY KEY (roll_no, course_id, semester), FOREIGN KEY (roll_no) REFERENCES student(roll_no), FOREIGN KEY (course_id) REFERENCES course(course_id) );

USE college_demo;

INSERT INTO department (dept_id, dept_name) VALUES (1, 'Computer Science'), (2, 'Mechanical'), (3, 'Electronics'); SELECT * FROM Department;

INSERT INTO student (roll_no, name, email, aadhar_no, dept_id) VALUES (101, 'Saumitra Tambekar', 'saumitra@example.com', '123456789012', 1), (102, 'Arnav', 'arnav@example.com', '234567890123', 2), (103, 'Riyanshu', 'riyanshu@example.com', '345678901234', 1); SELECT * FROM student;

INSERT INTO course (course_id, course_name, dept_id) VALUES (201, 'Database Systems', 1), (202, 'Thermodynamics', 2), (203, 'Digital Circuits', 3); SELECT * FROM course;

INSERT INTO enrollment (roll_no, course_id, semester, grade) VALUES (101, 201, 3, 'A'), (101, 203, 4, 'B'), (102, 202, 3, 'A'), (103, 201, 3, 'B'); SELECT * FROM enrollment; CREATE TABLE faculty ( faculty_id INT PRIMARY KEY, faculty_name VARCHAR(50) NOT NULL, email VARCHAR(50) UNIQUE, phone_no VARCHAR(15) UNIQUE, dept_id INT, FOREIGN KEY (dept_id) REFERENCES department(dept_id) );

INSERT INTO faculty VALUES (201, 'Dr. Sharma', 'sharma@gmail.com', '9876543210', 1), (202, 'Prof. Mehta', 'mehta@gmail.com', '9876543211', 2), (203, 'Dr. Rao', 'rao@gmail.com', '9876543212', 3); SELECT * FROM faculty;

This SQL script builds a complete college database system step by step. First, it ensures that any existing database named college_demo is dropped, then creates a fresh database with that name and switches to it using USE college_demo. The script defines four tables: department: stores department IDs and names, with dept_id as the primary key. student: stores student details such as roll number, name, email, Aadhaar number, and department ID. The dept_id here is a foreign key linking each student to their department. course: stores course details with course_id as the primary key and links each course to a department via dept_id. enrollment: records which student is enrolled in which course, along with the semester and grade. It uses a composite primary key (roll_no, course_id, semester) and foreign keys referencing both the student and course tables. After defining the structure, the script inserts sample data: three departments (Computer Science, Mechanical, Electronics), three students with unique roll numbers and Aadhaar numbers, and three courses linked to departments. Finally, it records enrollments showing which student took which course, in which semester, and their grade. The SELECT * queries after each insertion display the contents of the tables, allowing you to verify that the data has been correctly inserted. In summary, this code sets up a mini relational database for a college, with proper relationships between departments, students, courses, and enrollments, and demonstrates how to populate and query it. (102, 202, 3, 'A'), (103, 201, 3, 'B'); SELECT * FROM enrollment;



Yes. Based on your **college_demo** database, the normalization can be identified as follows.

## Normalization of Your Database

Your tables are designed to achieve **up to 3NF (Third Normal Form)**.

### 1. Department

| dept_id | dept_name        |
| ------- | ---------------- |
| 1       | Computer Science |
| 2       | Mechanical       |
| 3       | Electronics      |

**Primary Key:** `dept_id`

* **1NF:** Yes — each field contains a single value.
* **2NF:** Yes — there is no partial dependency because the primary key has only one attribute.
* **3NF:** Yes — `dept_name` depends only on `dept_id`.

**Result: Department is in 3NF.**

---

### 2. Student

| roll_no | name              | email                                               | aadhar_no    | dept_id |
| ------- | ----------------- | --------------------------------------------------- | ------------ | ------- |
| 101     | Saumitra Tambekar | [saumitra@example.com](mailto:saumitra@example.com) | 123456789012 | 1       |
| 102     | Arnav             | [arnav@example.com](mailto:arnav@example.com)       | 234567890123 | 2       |
| 103     | Riyanshu          | [riyanshu@example.com](mailto:riyanshu@example.com) | 345678901234 | 1       |

**Primary Key:** `roll_no`

Functional dependency:

`roll_no → name, email, aadhar_no, dept_id`

* **1NF:** Yes — atomic values.
* **2NF:** Yes — primary key is single attribute, so partial dependency is impossible.
* **3NF:** Yes — non-key attributes depend directly on `roll_no`.

`dept_id` is a foreign key referring to `department`.

**Result: Student is in 3NF.**

---

### 3. Course

| course_id | course_name      | dept_id |
| --------- | ---------------- | ------- |
| 201       | Database Systems | 1       |
| 202       | Thermodynamics   | 2       |
| 203       | Digital Circuits | 3       |

**Primary Key:** `course_id`

Functional dependency:

`course_id → course_name, dept_id`

* **1NF:** Yes.
* **2NF:** Yes — single-attribute primary key.
* **3NF:** Yes — all non-key attributes depend directly on `course_id`.

**Result: Course is in 3NF.**

---

### 4. Enrollment

| roll_no | course_id | semester | grade |
| ------- | --------- | -------- | ----- |
| 101     | 201       | 3        | A     |
| 101     | 203       | 4        | B     |
| 102     | 202       | 3        | A     |
| 103     | 201       | 3        | B     |

**Composite Primary Key:**

`(roll_no, course_id, semester)`

Functional dependency:

`(roll_no, course_id, semester) → grade`

* **1NF:** Yes — all values are atomic.
* **2NF:** Yes — `grade` depends on the complete composite key, not just part of it.
* **3NF:** Yes — there is no transitive dependency among non-key attributes.

**Result: Enrollment is in 3NF.**

---

### 5. Faculty

| faculty_id | faculty_name | email                                       | phone_no   | dept_id |
| ---------- | ------------ | ------------------------------------------- | ---------- | ------- |
| 201        | Dr. Sharma   | [sharma@gmail.com](mailto:sharma@gmail.com) | 9876543210 | 1       |
| 202        | Prof. Mehta  | [mehta@gmail.com](mailto:mehta@gmail.com)   | 9876543211 | 2       |
| 203        | Dr. Rao      | [rao@gmail.com](mailto:rao@gmail.com)       | 9876543212 | 3       |

**Primary Key:** `faculty_id`

Functional dependency:

`faculty_id → faculty_name, email, phone_no, dept_id`

* **1NF:** Yes.
* **2NF:** Yes — single primary key.
* **3NF:** Yes — non-key attributes depend directly on `faculty_id`.

**Result: Faculty is in 3NF.**

---

## Final Answer

| Table      | 1NF | 2NF | 3NF |
| ---------- | --- | --- | --- |
| Department | ✅   | ✅   | ✅   |
| Student    | ✅   | ✅   | ✅   |
| Course     | ✅   | ✅   | ✅   |
| Enrollment | ✅   | ✅   | ✅   |
| Faculty    | ✅   | ✅   | ✅   |

### Overall Normalization

**The given college database is normalized up to Third Normal Form (3NF).**

The main reason is that:

* There are **no repeating groups** → 1NF.
* There are **no partial dependencies** → 2NF.
* There are **no unnecessary transitive dependencies** → 3NF.
* Department information is separated into its own table instead of repeatedly storing `dept_name` in Student, Course, and Faculty.
* Enrollment handles the **many-to-many relationship between Student and Course**.

**Exam-style answer:**

> The given College Management Database is in **3NF**. All relations satisfy 1NF and 2NF, and there are no transitive dependencies among non-key attributes. Primary keys uniquely identify records, while foreign keys maintain relationships between the tables.

