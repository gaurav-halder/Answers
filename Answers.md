1.
SELECT d.DEPARTMENT_ID, c.COURSEID FROM DEPARTMENT_DETAILS d LEFT JOIN COURSE_DETAILS c ON d.DEPARTMENT_ID = c.DEPARTMENT_ID ORDER BY d.DEPARTMENT_ID;

2.
a) Table Creation
-- TEACHER Table
CREATE TABLE TEACHER (
    TEACHER_ID NUMBER PRIMARY KEY,
    TEACHER_NAME VARCHAR2(50) NOT NULL,
    SUBJECT VARCHAR2(50) UNIQUE,
    SALARY NUMBER CHECK (SALARY > 0)
);

-- STUDENT Table
CREATE TABLE STUDENT (
    STUDENT_ID NUMBER PRIMARY KEY,
    STUDENT_NAME VARCHAR2(50) NOT NULL,
    COURSE VARCHAR2(50),
    MARKS NUMBER CHECK (MARKS >= 0 AND MARKS <= 100),
    TEACHER_ID NUMBER,
    CONSTRAINT FK_TEACHER FOREIGN KEY (TEACHER_ID) REFERENCES TEACHER(TEACHER_ID)
);

b) Data Insertion
-- TEACHER Data
INSERT INTO TEACHER (TEACHER_ID, TEACHER_NAME, SUBJECT, SALARY) VALUES (1, 'Mr. Smith', 'Math', 50000);
INSERT INTO TEACHER (TEACHER_ID, TEACHER_NAME, SUBJECT, SALARY) VALUES (2, 'Ms. Johnson', 'Science', 60000);

-- STUDENT Data
INSERT INTO STUDENT (STUDENT_ID, STUDENT_NAME, COURSE, MARKS, TEACHER_ID) VALUES (101, 'Alice', 'Math', 90, 1);
INSERT INTO STUDENT (STUDENT_ID, STUDENT_NAME, COURSE, MARKS, TEACHER_ID) VALUES (102, 'Bob', 'Science', 85, 2);
INSERT INTO STUDENT (STUDENT_ID, STUDENT_NAME, COURSE, MARKS, TEACHER_ID) VALUES (103, 'Charlie', 'Math', 75, 1);

c) Query to display student details and their teacher's name
SELECT
    s.STUDENT_NAME,
    s.COURSE,
    s.MARKS,
    t.TEACHER_ID,
    t.TEACHER_NAME
FROM
    STUDENT s
JOIN
    TEACHER t ON s.TEACHER_ID = t.TEACHER_ID;

3.
CREATE TABLE employees (
    employee_id NUMBER PRIMARY KEY,
    first_name VARCHAR2(50),
    last_name VARCHAR2(50),
    department_name VARCHAR2(50),
    salary NUMBER
);

PL/SQL 

CREATE OR REPLACE PROCEDURE update_sales_salary (p_percentage_raise IN NUMBER) IS
BEGIN
    UPDATE employees
    SET salary = salary * (1 + p_percentage_raise / 100)
    WHERE department_name = 'Sales';

    DBMS_OUTPUT.PUT_LINE('Salaries updated successfully for Sales department.');
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error updating salaries: ' || SQLERRM);
END;
/


2.
CREATE OR REPLACE PROCEDURE find_max_min_salary (p_department_name IN VARCHAR2) IS
    v_max_salary employees.salary%TYPE;
    v_min_salary employees.salary%TYPE;
BEGIN
    SELECT MAX(salary), MIN(salary)
    INTO v_max_salary, v_min_salary
    FROM employees
    WHERE department_name = p_department_name;

    DBMS_OUTPUT.PUT_LINE('Maximum salary in ' || p_department_name || ': ' || v_max_salary);
    DBMS_OUTPUT.PUT_LINE('Minimum salary in ' || p_department_name || ': ' || v_min_salary);

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in department: ' || p_department_name);
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
/
