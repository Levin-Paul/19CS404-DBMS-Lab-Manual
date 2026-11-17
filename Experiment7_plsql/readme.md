
# EXPERIMENT 7: PL/SQL – VARIABLES, CONTROL STRUCTURES AND LOOPS

## AIM
To write and execute simple PL/SQL programs using variables, loops, and conditional statements.

## THEORY

PL/SQL stands for Procedural Language extension to Structured Query Language (SQL). It combines SQL with procedural programming features such as variables, conditional statements, loops, and exception handling.

### Basic Syntax of PL/SQL

```sql
DECLARE
    -- Declaration section
BEGIN
    -- Executable statements
EXCEPTION
    -- Exception handling section
END;
/
```

### Basic Components of a PL/SQL Block

1. **DECLARE:** Used to declare variables and constants. This section is optional.
2. **BEGIN:** Contains the executable statements. This section is mandatory.
3. **EXCEPTION:** Handles runtime errors and exceptions. This section is optional.
4. **END:** Marks the end of the PL/SQL block.

---

## PROGRAM 1: GREATEST OF TWO NUMBERS

### Algorithm / Steps
1. Declare two numeric variables `a` and `b`.
2. Initialize the variables with values.
3. Use an IF-ELSE statement to compare the two numbers.
4. Display the greater number using `DBMS_OUTPUT.PUT_LINE`.

### Program

```sql
SET SERVEROUTPUT ON;

DECLARE
    a NUMBER := 50;
    b NUMBER := 80;
BEGIN
    IF a > b THEN
        DBMS_OUTPUT.PUT_LINE('Greater number is: ' || a);
    ELSE
        DBMS_OUTPUT.PUT_LINE('Greater number is: ' || b);
    END IF;
END;
/
```

### Output

<img width="455" height="250" alt="image" src="https://github.com/user-attachments/assets/ce8c2b4f-f0d4-492a-af3d-f81f731c7d6c" />

---

## PROGRAM 2: SUM OF FIRST N NATURAL NUMBERS

### Algorithm / Steps
1. Declare variables `n`, `i`, and `total`.
2. Initialize `n` to 10, `i` to 1, and `total` to 0.
3. Use a WHILE loop to iterate from 1 to n.
4. Add the current value of `i` to `total`.
5. Increment `i` by 1 in each iteration.
6. Display the final sum.

### Program

```sql
SET SERVEROUTPUT ON;

DECLARE
    n NUMBER := 10;
    i NUMBER := 1;
    total NUMBER := 0;
BEGIN
    WHILE i <= n LOOP
        total := total + i;
        i := i + 1;
    END LOOP;

    DBMS_OUTPUT.PUT_LINE(
        'Sum of first ' || n ||
        ' natural numbers is: ' || total
    );
END;
/
```

### Output

<img width="431" height="260" alt="image" src="https://github.com/user-attachments/assets/a7c31f0d-611c-4bf9-afe5-c3515abce33e" />


---

## PROGRAM 3: GENERATE FIBONACCI SERIES

### Algorithm / Steps
1. Declare variables `n`, `a`, `b`, and `c`.
2. Initialize `n` to 7, `a` to 0, and `b` to 1.
3. Use a FOR loop to generate the Fibonacci series.
4. Display the current value of `a`.
5. Calculate the next number using `c = a + b`.
6. Update `a` and `b` for the next iteration.
7. Repeat until n terms are generated.

### Program

```sql
SET SERVEROUTPUT ON;

DECLARE
    n NUMBER := 7;
    a NUMBER := 0;
    b NUMBER := 1;
    c NUMBER;
BEGIN
    DBMS_OUTPUT.PUT('Fibonacci sequence: ');

    FOR i IN 1..n LOOP
        DBMS_OUTPUT.PUT(a || ' ');

        c := a + b;
        a := b;
        b := c;
    END LOOP;

    DBMS_OUTPUT.NEW_LINE;
END;
/
```

### Output

<img width="413" height="268" alt="image" src="https://github.com/user-attachments/assets/ac084336-76c9-4711-bcb6-da0da9bc9c54" />


---

## PROGRAM 4: REVERSE A NUMBER

### Algorithm / Steps
1. Declare variables `n`, `digit`, and `rev`.
2. Initialize `n` to 1535 and `rev` to 0.
3. Use a WHILE loop until n becomes 0.
4. Extract the last digit using `MOD(n,10)`.
5. Append the extracted digit to the reversed number using `rev = rev * 10 + digit`.
6. Remove the last digit from n using `TRUNC(n/10)`.
7. Display the reversed number.

### Program

```sql
SET SERVEROUTPUT ON;

DECLARE
    n NUMBER := 1535;
    digit NUMBER;
    rev NUMBER := 0;
BEGIN
    WHILE n > 0 LOOP
        digit := MOD(n, 10);
        rev := rev * 10 + digit;
        n := TRUNC(n / 10);
    END LOOP;

    DBMS_OUTPUT.PUT_LINE('Reversed number is: ' || rev);
END;
/
```

### Output

<img width="505" height="263" alt="image" src="https://github.com/user-attachments/assets/88bfdc67-ee45-4d50-860b-ac36c76f6e48" />

---

## PROGRAM 5: LARGEST OF THREE NUMBERS

### Algorithm / Steps
1. Declare three numeric variables `a`, `b`, and `c`.
2. Initialize them with values 10, 9, and 15.
3. Use IF-ELSIF-ELSE conditional statements.
4. Compare the three numbers to identify the largest.
5. Display the largest number.

### Program

```sql
SET SERVEROUTPUT ON;

DECLARE
    a NUMBER := 10;
    b NUMBER := 9;
    c NUMBER := 15;
BEGIN
    IF a > b AND a > c THEN
        DBMS_OUTPUT.PUT_LINE('Largest number is: ' || a);

    ELSIF b > a AND b > c THEN
        DBMS_OUTPUT.PUT_LINE('Largest number is: ' || b);

    ELSE
        DBMS_OUTPUT.PUT_LINE('Largest number is: ' || c);
    END IF;
END;
/
```

### Output

<img width="377" height="250" alt="image" src="https://github.com/user-attachments/assets/814060e1-71e0-4a94-a3b1-26a462cae12c" />
---

## IMPORTANT PL/SQL CONCEPTS

| Concept | Description |
|---|---|
| PL/SQL | Procedural extension of SQL |
| DECLARE | Variable declaration section |
| BEGIN | Execution section |
| END | Terminates the PL/SQL block |
| NUMBER | Numeric datatype |
| VARCHAR2 | Variable-length character datatype |
| := | Assignment operator |
| = | Equality comparison operator |
| IF | Conditional statement |
| ELSIF | Additional conditional branch |
| ELSE | Executes when previous conditions are false |
| WHILE LOOP | Repeats while a condition is true |
| FOR LOOP | Repeats for a specified range |
| MOD() | Returns the remainder |
| TRUNC() | Removes the fractional part |
| DBMS_OUTPUT.PUT_LINE | Displays output on a new line |
| DBMS_OUTPUT.PUT | Displays output without a new line |

---

## RESULT

Thus, the PL/SQL programs using variables, conditional statements (IF-ELSE and IF-ELSIF-ELSE), and loops (WHILE and FOR) were written and executed successfully.

