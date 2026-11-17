
# EXPERIMENT 8: PL/SQL CURSOR PROGRAMS

## AIM

To write and execute PL/SQL programs using cursors and exception handling to manage runtime errors effectively and display appropriate messages.

## THEORY

In PL/SQL, cursors are used to process query result sets row by row.

There are two types of cursors:

1. **Implicit Cursor:** Automatically created by PL/SQL for SQL statements such as INSERT, UPDATE, DELETE and single-row SELECT INTO statements.
2. **Explicit Cursor:** Declared and controlled by the programmer to process multiple rows returned by a query.

### Types of Explicit Cursors

1. **Simple Cursor:** A basic cursor used to retrieve multiple rows from a table.
2. **Parameterized Cursor:** Accepts parameters to filter records dynamically.
3. **Cursor FOR Loop:** Automatically opens, fetches and closes the cursor.
4. **%ROWTYPE Cursor:** Fetches an entire row into a record variable.
5. **Cursor with FOR UPDATE:** Locks selected rows so they can be updated while processing them.

### Basic Syntax of a PL/SQL Block

```sql
DECLARE
    -- Declaration section
BEGIN
    -- Executable statements
EXCEPTION
    -- Exception handling
END;
/
```

### Exception Handling

Exception handling is used to handle runtime errors during program execution.

Types of exceptions:

- **Predefined Exceptions:** Automatically defined by Oracle, such as NO_DATA_FOUND, TOO_MANY_ROWS and ZERO_DIVIDE.
- **User-defined Exceptions:** Declared by the programmer using the EXCEPTION keyword.
- **WHEN OTHERS:** Handles any exception not handled by previous exception handlers.

---

## PROGRAM 1: SIMPLE CURSOR WITH EXCEPTION HANDLING

### Algorithm / Steps

1. Create an employees table with emp_id, emp_name and designation.
2. Insert sample employee records.
3. Declare a simple cursor to retrieve employee names and designations.
4. Open the cursor.
5. Fetch records one by one.
6. Display employee details.
7. Close the cursor.
8. Handle exceptions using NO_DATA_FOUND and WHEN OTHERS.

### Table Creation and Sample Data

```sql
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50)
);

INSERT INTO employees VALUES (101, 'Ravi', 'Manager');
INSERT INTO employees VALUES (102, 'Priya', 'Developer');
INSERT INTO employees VALUES (103, 'Arun', 'Analyst');

COMMIT;
```

### Program

```sql
SET SERVEROUTPUT ON;

DECLARE
    CURSOR c_emp IS
        SELECT emp_name, designation
        FROM employees;

    v_name employees.emp_name%TYPE;
    v_designation employees.designation%TYPE;
    v_count NUMBER := 0;

    e_no_data EXCEPTION;

BEGIN
    OPEN c_emp;

    LOOP
        FETCH c_emp INTO v_name, v_designation;
        EXIT WHEN c_emp%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(
            'Name: ' || v_name ||
            ', Designation: ' || v_designation
        );

        v_count := v_count + 1;
    END LOOP;

    CLOSE c_emp;

    IF v_count = 0 THEN
        RAISE e_no_data;
    END IF;

EXCEPTION
    WHEN e_no_data THEN
        IF c_emp%ISOPEN THEN
            CLOSE c_emp;
        END IF;

        DBMS_OUTPUT.PUT_LINE('No employee records found');

    WHEN OTHERS THEN
        IF c_emp%ISOPEN THEN
            CLOSE c_emp;
        END IF;

        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
/
```

### Output

<img width="342" height="287" alt="image" src="https://github.com/user-attachments/assets/05ac5355-bc80-4224-b745-f6f18c0d6eb1" />

---

## PROGRAM 2: PARAMETERIZED CURSOR WITH EXCEPTION HANDLING

### Algorithm / Steps

1. Add a salary column to the employees table.
2. Insert salary values for employees.
3. Declare a parameterized cursor that accepts minimum and maximum salary.
4. Open the cursor with the required salary range.
5. Fetch employees whose salary falls within the range.
6. Display employee details.
7. Handle exceptions for no records and unexpected errors.

### Table Modification and Sample Data

```sql
ALTER TABLE employees
ADD salary NUMBER(10,2);

UPDATE employees SET salary = 45000
WHERE emp_id = 101;

UPDATE employees SET salary = 55000
WHERE emp_id = 102;

UPDATE employees SET salary = 40000
WHERE emp_id = 103;

COMMIT;
```

### Program

```sql
SET SERVEROUTPUT ON;

DECLARE
    CURSOR c_emp(p_min NUMBER, p_max NUMBER) IS
        SELECT emp_id, emp_name, salary
        FROM employees
        WHERE salary BETWEEN p_min AND p_max;

    v_id employees.emp_id%TYPE;
    v_name employees.emp_name%TYPE;
    v_salary employees.salary%TYPE;

    v_count NUMBER := 0;
    e_no_data EXCEPTION;

BEGIN
    OPEN c_emp(40000, 50000);

    LOOP
        FETCH c_emp INTO v_id, v_name, v_salary;
        EXIT WHEN c_emp%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(
            'ID: ' || v_id ||
            ', Name: ' || v_name ||
            ', Salary: ' || v_salary
        );

        v_count := v_count + 1;
    END LOOP;

    CLOSE c_emp;

    IF v_count = 0 THEN
        RAISE e_no_data;
    END IF;

EXCEPTION
    WHEN e_no_data THEN
        IF c_emp%ISOPEN THEN
            CLOSE c_emp;
        END IF;

        DBMS_OUTPUT.PUT_LINE(
            'No employees found in the given salary range'
        );

    WHEN OTHERS THEN
        IF c_emp%ISOPEN THEN
            CLOSE c_emp;
        END IF;

        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
/
```

### Output

<img width="427" height="275" alt="image" src="https://github.com/user-attachments/assets/533244c1-a351-42f6-9abe-06768fe5e8a4" />


---

## PROGRAM 3: CURSOR FOR LOOP WITH EXCEPTION HANDLING

### Algorithm / Steps

1. Add a dept_no column to the employees table.
2. Insert department numbers for employees.
3. Declare a cursor to retrieve employee names and department numbers.
4. Use a cursor FOR loop to automatically fetch records.
5. Display employee names and department numbers.
6. Handle exceptions using a user-defined exception and WHEN OTHERS.

### Table Modification and Sample Data

```sql
ALTER TABLE employees
ADD dept_no NUMBER;

UPDATE employees SET dept_no = 10
WHERE emp_id = 101;

UPDATE employees SET dept_no = 20
WHERE emp_id = 102;

UPDATE employees SET dept_no = 10
WHERE emp_id = 103;

COMMIT;
```

### Program

```sql
SET SERVEROUTPUT ON;

DECLARE
    CURSOR c_emp IS
        SELECT emp_name, dept_no
        FROM employees;

    v_count NUMBER := 0;
    e_no_data EXCEPTION;

BEGIN
    FOR r IN c_emp LOOP

        DBMS_OUTPUT.PUT_LINE(
            'Employee: ' || r.emp_name ||
            ', Department: ' || r.dept_no
        );

        v_count := v_count + 1;

    END LOOP;

    IF v_count = 0 THEN
        RAISE e_no_data;
    END IF;

EXCEPTION
    WHEN e_no_data THEN
        DBMS_OUTPUT.PUT_LINE('No employee records found');

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
/
```

### Output

<img width="402" height="281" alt="image" src="https://github.com/user-attachments/assets/65d622e8-c1b5-45b2-9eaf-6d5df1feb7a8" />


---

## PROGRAM 4: CURSOR WITH %ROWTYPE AND EXCEPTION HANDLING

### Algorithm / Steps

1. Create an employees table containing emp_id, emp_name, designation and salary.
2. Insert sample employee records.
3. Declare a cursor to retrieve all employee records.
4. Declare a record variable using employees%ROWTYPE.
5. Open the cursor and fetch complete employee records.
6. Display employee details.
7. Close the cursor.
8. Handle exceptions using a user-defined exception and WHEN OTHERS.

### Table Creation and Sample Data

This program uses the existing employees table created in Program 1 and modified in Program 2.

### Program

```sql
SET SERVEROUTPUT ON;

DECLARE
    CURSOR c_emp IS
        SELECT emp_id, emp_name, designation, salary
        FROM employees;

    v_emp employees%ROWTYPE;
    v_count NUMBER := 0;

    e_no_data EXCEPTION;

BEGIN
    OPEN c_emp;

    LOOP
        FETCH c_emp INTO
            v_emp.emp_id,
            v_emp.emp_name,
            v_emp.designation,
            v_emp.salary;

        EXIT WHEN c_emp%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(
            'ID: ' || v_emp.emp_id ||
            ', Name: ' || v_emp.emp_name ||
            ', Designation: ' || v_emp.designation ||
            ', Salary: ' || v_emp.salary
        );

        v_count := v_count + 1;

    END LOOP;

    CLOSE c_emp;

    IF v_count = 0 THEN
        RAISE e_no_data;
    END IF;

EXCEPTION
    WHEN e_no_data THEN
        IF c_emp%ISOPEN THEN
            CLOSE c_emp;
        END IF;

        DBMS_OUTPUT.PUT_LINE('No employee records found');

    WHEN OTHERS THEN
        IF c_emp%ISOPEN THEN
            CLOSE c_emp;
        END IF;

        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
/
```

### Output

<img width="515" height="290" alt="image" src="https://github.com/user-attachments/assets/65143aa6-aaaf-4b15-a9be-e72cbcde7174" />

---

## PROGRAM 5: CURSOR WITH FOR UPDATE CLAUSE AND EXCEPTION HANDLING

### Algorithm / Steps

1. Add dept_no and salary columns to the employees table if they do not already exist.
2. Insert employee records with different department numbers.
3. Declare a cursor with the FOR UPDATE clause.
4. Open the cursor and fetch employee records from a specific department.
5. Use WHERE CURRENT OF to update the salary of the currently fetched employee.
6. Display the updated employee details.
7. Commit the changes.
8. Handle exceptions using a user-defined exception and WHEN OTHERS.

### Table Modification and Sample Data

This program uses the existing employees table from the previous programs.

### Program

```sql
SET SERVEROUTPUT ON;

DECLARE
    CURSOR c_emp IS
        SELECT emp_id, emp_name, salary
        FROM employees
        WHERE dept_no = 10
        FOR UPDATE OF salary;

    v_id employees.emp_id%TYPE;
    v_name employees.emp_name%TYPE;
    v_salary employees.salary%TYPE;

    v_count NUMBER := 0;
    e_no_data EXCEPTION;

BEGIN
    OPEN c_emp;

    LOOP
        FETCH c_emp INTO v_id, v_name, v_salary;
        EXIT WHEN c_emp%NOTFOUND;

        UPDATE employees
        SET salary = salary + 5000
        WHERE CURRENT OF c_emp;

        DBMS_OUTPUT.PUT_LINE(
            'Employee: ' || v_name ||
            ', Old Salary: ' || v_salary ||
            ', New Salary: ' || (v_salary + 5000)
        );

        v_count := v_count + 1;

    END LOOP;

    CLOSE c_emp;

    IF v_count = 0 THEN
        RAISE e_no_data;
    END IF;

    COMMIT;

EXCEPTION
    WHEN e_no_data THEN
        IF c_emp%ISOPEN THEN
            CLOSE c_emp;
        END IF;

        DBMS_OUTPUT.PUT_LINE(
            'No employees found in the specified department'
        );

    WHEN OTHERS THEN
        IF c_emp%ISOPEN THEN
            CLOSE c_emp;
        END IF;

        ROLLBACK;

        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
/
```

### Output

<img width="435" height="276" alt="image" src="https://github.com/user-attachments/assets/1880e0fa-9a04-43d8-9fba-89782d7b7056" />

---

## IMPORTANT PL/SQL CURSOR CONCEPTS

| Concept | Description |
|---|---|
| Cursor | Points to a query result set |
| Implicit Cursor | Automatically created by Oracle |
| Explicit Cursor | Declared and controlled by the programmer |
| OPEN | Opens an explicit cursor |
| FETCH | Retrieves the next row |
| CLOSE | Closes the cursor |
| %FOUND | Returns TRUE if a row was fetched |
| %NOTFOUND | Returns TRUE if no row was fetched |
| %ISOPEN | Checks whether a cursor is open |
| %ROWCOUNT | Returns the number of rows fetched |
| %ROWTYPE | Declares a record with the structure of a table row |
| FOR UPDATE | Locks selected rows for updating |
| WHERE CURRENT OF | Updates the currently fetched row |
| NO_DATA_FOUND | Raised when a SELECT INTO returns no rows |
| TOO_MANY_ROWS | Raised when a SELECT INTO returns multiple rows |
| WHEN OTHERS | Handles other unhandled exceptions |

---

## IMPORTANT DIFFERENCES

### 1. Simple Cursor vs Cursor FOR Loop

| Simple Cursor | Cursor FOR Loop |
|---|---|
| Programmer opens the cursor | Automatically opens cursor |
| Programmer fetches records | Automatically fetches records |
| Programmer closes cursor | Automatically closes cursor |
| Requires more statements | Requires fewer statements |

### 2. Implicit Cursor vs Explicit Cursor

| Implicit Cursor | Explicit Cursor |
|---|---|
| Created automatically | Declared by programmer |
| Used for SQL statements | Used for multi-row queries |
| Requires no OPEN or CLOSE | Usually requires OPEN and CLOSE |
| Example: SQL%ROWCOUNT | Example: c_emp%NOTFOUND |

### 3. Cursor Attributes

- `%FOUND`: TRUE if the previous FETCH retrieved a row.
- `%NOTFOUND`: TRUE if the previous FETCH did not retrieve a row.
- `%ISOPEN`: TRUE if the cursor is currently open.
- `%ROWCOUNT`: Number of rows fetched so far.

---

## RESULT

Thus, the PL/SQL cursor programs using simple cursors, parameterized cursors, cursor FOR loops, %ROWTYPE and FOR UPDATE clauses were successfully written and executed with appropriate exception handling.

