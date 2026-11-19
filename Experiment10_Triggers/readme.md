
# EXPERIMENT 10: PL/SQL – TRIGGERS

## AIM

To write and execute PL/SQL trigger programs for automating actions in response to specific table events like INSERT, UPDATE, or DELETE.

## THEORY

A trigger is a stored PL/SQL block that is automatically executed or fired when a specified event occurs on a table or view.

Triggers are used for enforcing business rules, auditing changes, validating data, and performing automatic updates.

### Types of Triggers

1. **BEFORE Trigger:** Executes before an INSERT, UPDATE or DELETE operation.
2. **AFTER Trigger:** Executes after an INSERT, UPDATE or DELETE operation.
3. **Row-level Trigger:** Executes for every row affected by the triggering statement.
4. **Statement-level Trigger:** Executes only once for the entire SQL statement.

### Basic Syntax of a Trigger

```sql
CREATE OR REPLACE TRIGGER trigger_name
BEFORE OR AFTER INSERT OR UPDATE OR DELETE
ON table_name
FOR EACH ROW
BEGIN
    -- Trigger statements
END;
/
```

### Important Trigger Concepts

- **CREATE OR REPLACE TRIGGER:** Creates a new trigger or replaces an existing trigger.
- **BEFORE:** Executes before the triggering operation.
- **AFTER:** Executes after the triggering operation.
- **FOR EACH ROW:** Makes the trigger execute once for every affected row.
- **:NEW:** Refers to the new values of a row.
- **:OLD:** Refers to the old values of a row.
- **RAISE_APPLICATION_ERROR:** Generates a custom error message.

---

## PROGRAM 1: TRIGGER TO LOG EVERY INSERTION INTO A TABLE

### Algorithm / Steps

1. Create an employees table to store employee details.
2. Create an employee_log table to store inserted employee records.
3. Create an AFTER INSERT trigger on the employees table.
4. Insert the new employee details into employee_log using :NEW.
5. Insert a new employee record into the employees table.
6. Display the employee_log table to verify the inserted record.

### Table Creation and Sample Data

```sql
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    salary NUMBER
);

CREATE TABLE employee_log (
    emp_id NUMBER,
    emp_name VARCHAR2(50),
    salary NUMBER,
    log_date DATE
);
```

### Program

```sql
CREATE OR REPLACE TRIGGER trg_employee_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employee_log
    VALUES (
        :NEW.emp_id,
        :NEW.emp_name,
        :NEW.salary,
        SYSDATE
    );
END;
/
```

### Executing the Trigger

```sql
INSERT INTO employees
VALUES (101, 'Ravi', 25000);

COMMIT;
```

### Displaying the Output

```sql
SELECT * FROM employee_log;
```

### Output

<img width="707" height="72" alt="image" src="https://github.com/user-attachments/assets/16001169-aa79-4eb3-80ba-0e3c5eec7ad8" />
<img width="570" height="195" alt="image" src="https://github.com/user-attachments/assets/946e22b4-b2cb-4113-99cc-cc1eee6b3d14" />

---

## PROGRAM 2: TRIGGER TO PREVENT DELETION OF RECORDS

### Algorithm / Steps

1. Create a sensitive_data table.
2. Insert sample records into the table.
3. Create a BEFORE DELETE trigger on sensitive_data.
4. Use RAISE_APPLICATION_ERROR to prevent deletion.
5. Attempt to delete a record from the table.
6. Display the custom error message.

### Table Creation and Sample Data

```sql
CREATE TABLE sensitive_data (
    id NUMBER PRIMARY KEY,
    data VARCHAR2(100)
);

INSERT INTO sensitive_data VALUES (1, 'Confidential Data');
INSERT INTO sensitive_data VALUES (2, 'Important Records');

COMMIT;
```

### Program

```sql
CREATE OR REPLACE TRIGGER trg_prevent_delete
BEFORE DELETE ON sensitive_data
FOR EACH ROW
BEGIN
    RAISE_APPLICATION_ERROR(
        -20001,
        'Deletion not allowed on this table.'
    );
END;
/
```

### Executing the Trigger

```sql
DELETE FROM sensitive_data
WHERE id = 1;
```

### Output

<img width="563" height="176" alt="image" src="https://github.com/user-attachments/assets/6c9dcc18-70bb-4650-9aa0-965de0963f98" />

The deletion is blocked and the original record remains in the table.

---

## PROGRAM 3: TRIGGER TO AUTOMATICALLY UPDATE LAST_MODIFIED TIMESTAMP

### Algorithm / Steps

1. Create a products table with product_id, product_name, price and last_modified columns.
2. Insert sample product records.
3. Create a BEFORE UPDATE trigger on the products table.
4. Set last_modified to the current timestamp using SYSTIMESTAMP.
5. Update a product record.
6. Display the products table to verify the updated timestamp.

### Table Creation and Sample Data

```sql
CREATE TABLE products (
    product_id NUMBER PRIMARY KEY,
    product_name VARCHAR2(50),
    price NUMBER(10,2),
    last_modified TIMESTAMP
);

INSERT INTO products
VALUES (1, 'Laptop', 50000, SYSTIMESTAMP);

INSERT INTO products
VALUES (2, 'Mobile', 20000, SYSTIMESTAMP);

COMMIT;
```

### Program

```sql
CREATE OR REPLACE TRIGGER trg_update_timestamp
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    :NEW.last_modified := SYSTIMESTAMP;
END;
/
```

### Executing the Trigger

```sql
UPDATE products
SET price = 55000
WHERE product_id = 1;

COMMIT;
```

### Displaying the Output

```sql
SELECT * FROM products;
```

### Output

<img width="862" height="120" alt="image" src="https://github.com/user-attachments/assets/7aeeaf74-3125-4092-8cb7-7508825353d8" />

---

## PROGRAM 4: TRIGGER TO COUNT THE NUMBER OF UPDATES MADE TO A TABLE

### Algorithm / Steps

1. Create a customer_orders table.
2. Insert sample order records.
3. Create an audit_log table with a counter column.
4. Insert an initial counter value of zero into audit_log.
5. Create an AFTER UPDATE trigger on customer_orders.
6. Increment the counter in audit_log whenever an order record is updated.
7. Update a customer order.
8. Display the audit_log table to verify the update count.

### Table Creation and Sample Data

```sql
CREATE TABLE customer_orders (
    order_id NUMBER PRIMARY KEY,
    customer_name VARCHAR2(50),
    amount NUMBER(10,2)
);

CREATE TABLE audit_log (
    update_count NUMBER
);

INSERT INTO customer_orders
VALUES (1, 'Ravi', 5000);

INSERT INTO customer_orders
VALUES (2, 'Priya', 8000);

INSERT INTO audit_log
VALUES (0);

COMMIT;
```

### Program

```sql
CREATE OR REPLACE TRIGGER trg_update_count
AFTER UPDATE ON customer_orders
FOR EACH ROW
BEGIN
    UPDATE audit_log
    SET update_count = update_count + 1;
END;
/
```

### Executing the Trigger

```sql
UPDATE customer_orders
SET amount = 6000
WHERE order_id = 1;

COMMIT;
```

### Displaying the Output

```sql
SELECT * FROM audit_log;
```

### Output

<img width="335" height="82" alt="image" src="https://github.com/user-attachments/assets/61531b4a-aa94-4b9a-8aca-159ca803fd12" />

If another order record is updated, the counter increases to 2.

---

## PROGRAM 5: TRIGGER TO CHECK A CONDITION BEFORE INSERTION

### Algorithm / Steps

1. Create an employees table with emp_id, emp_name and salary.
2. Create a BEFORE INSERT trigger on the employees table.
3. Check whether the inserted salary is greater than or equal to 3000.
4. If the salary is below 3000, raise an application error.
5. If the salary satisfies the condition, allow the insertion.
6. Test the trigger with valid and invalid salary values.

### Table Creation

```sql
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    salary NUMBER
);
```

### Program

```sql
CREATE OR REPLACE TRIGGER trg_check_salary
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    IF :NEW.salary < 3000 THEN
        RAISE_APPLICATION_ERROR(
            -20002,
            'Salary below minimum threshold.'
        );
    END IF;
END;
/
```

### Executing the Trigger

**Case 1: Valid Salary**

```sql
INSERT INTO employees
VALUES (101, 'Ravi', 5000);

COMMIT;
```

Output:

```text
1 row inserted.
```

**Case 2: Invalid Salary**

```sql
INSERT INTO employees
VALUES (102, 'Arun', 2000);
```

Output:

<img width="735" height="196" alt="image" src="https://github.com/user-attachments/assets/6fddab7f-2b42-4783-9821-a7b625e5d74c" />

The invalid record is not inserted into the employees table.

---

## IMPORTANT PL/SQL TRIGGER CONCEPTS

| Concept | Description |
|---|---|
| Trigger | Automatically executed PL/SQL block |
| BEFORE | Executes before a table operation |
| AFTER | Executes after a table operation |
| INSERT | Adds a new record |
| UPDATE | Modifies an existing record |
| DELETE | Removes a record |
| FOR EACH ROW | Executes for every affected row |
| :NEW | Refers to the new row values |
| :OLD | Refers to the old row values |
| SYSDATE | Returns the current date and time |
| SYSTIMESTAMP | Returns the current timestamp |
| RAISE_APPLICATION_ERROR | Generates a custom error |
| CREATE OR REPLACE | Creates or replaces a trigger |

---

## DIFFERENCE BETWEEN BEFORE AND AFTER TRIGGERS

| BEFORE Trigger | AFTER Trigger |
|---|---|
| Executes before the operation | Executes after the operation |
| Used for validation | Used for auditing |
| Can modify :NEW values in a row-level trigger | Cannot modify :NEW values |
| Can prevent an operation | Can perform additional actions after an operation |

---

## DIFFERENCE BETWEEN ROW-LEVEL AND STATEMENT-LEVEL TRIGGERS

| Row-Level Trigger | Statement-Level Trigger |
|---|---|
| Executes for every affected row | Executes once per SQL statement |
| Uses FOR EACH ROW | Does not use FOR EACH ROW |
| Can access :NEW and :OLD | Cannot access :NEW and :OLD |
| Used for individual row operations | Used for overall statement operations |

---

## RESULT

Thus, the PL/SQL trigger programs for logging insertions, preventing deletions, updating timestamps, counting updates and validating salary conditions were successfully written and executed.
