
# EXPERIMENT 9: PL/SQL – PROCEDURES AND FUNCTIONS

## AIM

To understand and implement procedures and functions in PL/SQL for performing various operations such as calculations, decision-making, and looping.

## THEORY

PL/SQL (Procedural Language/SQL) extends SQL by adding procedural constructs like variables, conditions, loops, procedures, and functions.

Procedures and functions are subprograms that help modularize code and improve code reusability.

### 1. PROCEDURE

A PL/SQL procedure is a subprogram that performs a specific action. It does not return a value directly but can return values using OUT parameters.

**Syntax:**

```sql
CREATE OR REPLACE PROCEDURE procedure_name(parameters)
IS
BEGIN
    -- Statements
END;
/
```

**Calling a Procedure:**

```sql
EXEC procedure_name(arguments);
```

### 2. FUNCTION

A PL/SQL function is a subprogram that performs an operation and returns a single value using the RETURN keyword.

**Syntax:**

```sql
CREATE OR REPLACE FUNCTION function_name(parameters)
RETURN datatype
IS
BEGIN
    -- Statements
    RETURN value;
END;
/
```

**Calling a Function:**

```sql
SELECT function_name(arguments) FROM DUAL;
```

### Key Differences Between Procedures and Functions

| Procedure | Function |
|---|---|
| Performs a specific action | Performs an operation and returns a value |
| Does not return a value directly | Must return a value |
| Can use OUT parameters | Uses RETURN statement |
| Called using EXEC or from PL/SQL | Can be called from PL/SQL or SQL queries, subject to SQL restrictions |

---

## PROGRAM 1: PL/SQL PROCEDURE TO FIND THE SQUARE OF A NUMBER

### Algorithm / Steps

1. Create a procedure named `find_square`.
2. Declare an input parameter to accept a number.
3. Calculate the square of the input number.
4. Display the result using `DBMS_OUTPUT.PUT_LINE`.
5. Call the procedure with a number as input.

### Program

```sql
SET SERVEROUTPUT ON;

CREATE OR REPLACE PROCEDURE find_square(
    n IN NUMBER
)
IS
    result NUMBER;
BEGIN
    result := n * n;

    DBMS_OUTPUT.PUT_LINE(
        'Square of ' || n || ' is ' || result
    );
END;
/
```

### Calling the Procedure

```sql
EXEC find_square(6);
```

### Output

<img width="395" height="237" alt="image" src="https://github.com/user-attachments/assets/a7325193-6aa1-4638-bd6d-07c4c12228c0" />
<img width="352" height="172" alt="image" src="https://github.com/user-attachments/assets/31c4fb68-993b-424b-8bc3-576125145c0c" />

---

## PROGRAM 2: PL/SQL FUNCTION TO RETURN THE FACTORIAL OF A NUMBER

### Algorithm / Steps

1. Create a function named `get_factorial`.
2. Declare an input parameter to accept a number.
3. Initialize the factorial variable to 1.
4. Use a FOR loop from 1 to the given number.
5. Multiply the factorial variable by the loop counter.
6. Return the calculated factorial using the RETURN statement.
7. Call the function using a SELECT statement.

### Program

```sql
CREATE OR REPLACE FUNCTION get_factorial(
    n IN NUMBER
)
RETURN NUMBER
IS
    fact NUMBER := 1;
BEGIN
    FOR i IN 1..n LOOP
        fact := fact * i;
    END LOOP;

    RETURN fact;
END;
/
```

### Calling the Function

```sql
SELECT get_factorial(5) AS factorial
FROM DUAL;
```

### Output

<img width="712" height="230" alt="image" src="https://github.com/user-attachments/assets/09641f26-b035-41ed-a5b5-962a51c0ccd4" />
<img width="323" height="85" alt="image" src="https://github.com/user-attachments/assets/a14ec43c-12ad-420a-8d47-3bc365a524ff" />

---

## PROGRAM 3: PL/SQL PROCEDURE TO CHECK WHETHER A NUMBER IS EVEN OR ODD

### Algorithm / Steps

1. Create a procedure named `check_even_odd`.
2. Declare an input parameter to accept a number.
3. Use the MOD function to check whether the number is divisible by 2.
4. If the remainder is 0, display EVEN.
5. Otherwise, display ODD.
6. Call the procedure with a number as input.

### Program

```sql
SET SERVEROUTPUT ON;

CREATE OR REPLACE PROCEDURE check_even_odd(
    n IN NUMBER
)
IS
BEGIN
    IF MOD(n, 2) = 0 THEN
        DBMS_OUTPUT.PUT_LINE(n || ' is Even');
    ELSE
        DBMS_OUTPUT.PUT_LINE(n || ' is Odd');
    END IF;
END;
/
```

### Calling the Procedure

```sql
EXEC check_even_odd(12);
```

### Output

<img width="412" height="225" alt="image" src="https://github.com/user-attachments/assets/a1a38efd-b832-413d-bc8c-a4f6f843c7fe" />
<img width="342" height="167" alt="image" src="https://github.com/user-attachments/assets/a77c8e6f-4feb-4b44-8874-4521b1c4bb21" />

---

## PROGRAM 4: PL/SQL FUNCTION TO RETURN THE REVERSE OF A NUMBER

### Algorithm / Steps

1. Create a function named `reverse_number`.
2. Declare an input parameter to accept a number.
3. Initialize the reverse variable to 0.
4. Use a WHILE loop until the number becomes 0.
5. Extract the last digit using MOD(n,10).
6. Add the extracted digit to the reverse number.
7. Remove the last digit using TRUNC(n/10).
8. Return the reversed number.
9. Call the function and display the result.

### Program

```sql
CREATE OR REPLACE FUNCTION reverse_number(
    n IN NUMBER
)
RETURN NUMBER
IS
    num NUMBER := n;
    digit NUMBER;
    rev NUMBER := 0;
BEGIN
    WHILE num > 0 LOOP
        digit := MOD(num, 10);

        rev := rev * 10 + digit;

        num := TRUNC(num / 10);
    END LOOP;

    RETURN rev;
END;
/
```

### Calling the Function

```sql
SELECT reverse_number(1234) AS reversed_number
FROM DUAL;
```

### Output

<img width="401" height="217" alt="image" src="https://github.com/user-attachments/assets/edce854b-0488-4378-9f82-6e931d776bf0" />
<img width="308" height="92" alt="image" src="https://github.com/user-attachments/assets/20d1fe6c-9cdf-45dd-8f77-149fbd7bcb50" />

---

## PROGRAM 5: PL/SQL PROCEDURE TO DISPLAY THE MULTIPLICATION TABLE OF A NUMBER

### Algorithm / Steps

1. Create a procedure named `print_table`.
2. Declare an input parameter to accept a number.
3. Use a FOR loop from 1 to 10.
4. Multiply the input number by the loop counter.
5. Display the multiplication results using `DBMS_OUTPUT.PUT_LINE`.
6. Call the procedure with a number as input.

### Program

```sql
SET SERVEROUTPUT ON;

CREATE OR REPLACE PROCEDURE print_table(
    n IN NUMBER
)
IS
BEGIN
    FOR i IN 1..10 LOOP
        DBMS_OUTPUT.PUT_LINE(
            n || ' x ' || i || ' = ' || (n * i)
        );
    END LOOP;
END;
/
```

### Calling the Procedure

```sql
EXEC print_table(5);
```

### Output

<img width="485" height="231" alt="image" src="https://github.com/user-attachments/assets/7cf178b2-a7f9-4b20-80b6-495736e628b5" />
<img width="380" height="265" alt="image" src="https://github.com/user-attachments/assets/f9bcc1ca-b75d-47fb-aa63-907adbda742d" />

---

## IMPORTANT PL/SQL CONCEPTS

| Concept | Description |
|---|---|
| Procedure | Subprogram that performs an operation |
| Function | Subprogram that returns a value |
| CREATE OR REPLACE | Creates a new subprogram or replaces an existing one |
| IN | Passes a value into a procedure or function |
| OUT | Returns a value through a parameter |
| IN OUT | Passes a value in and returns its modified value |
| IS | Begins the declaration section of a subprogram |
| RETURN | Returns a value from a function |
| MOD() | Returns the remainder |
| TRUNC() | Removes the fractional part |
| DUAL | Oracle's special one-row table |
| DBMS_OUTPUT.PUT_LINE | Displays output |

---

## RESULT

Thus, the PL/SQL programs using procedures and functions for calculations, decision-making, and looping were successfully written, compiled, and executed.
