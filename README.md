# Oracle SQL Quick Tour

## Classification of SQL

- Data Definition Language
- Data Manipulation Language
- Data Retrieval Language
- Data Control Language
- Transaction Control Language

---

`DISC` - Disconnect from database

`SHOW USER` - shows current user

---

## Data Definition Language (DDL) Statements

- CREATE
- ALTER
- DROP
- TRUNCATE
- RENAME

- ### **`CREATE`**

```bash
CREATE TABLE EMPLOYEES(
    EMP_ID NUMBER,
    FIRSTNAME VARCHAR2(50),
    LASTNAME VARCHAR2(50),
    EMAIL_ID VARCHAR(30),
    GENDER CHAR(1),
    MOBILE_NO CHAR(10),
    DATE_OF_JOINING DATE DEFAULT SYSDATE,
    DEPT_ID INT
);
```

```bash
DESC EMPLOYEES;
```

<img src = "Images/createTable.PNG">

- ### **`ALTER`**

  1. ADDING COLUMN
  2. MODIFYING COLUMN
  3. DROPPING COLUMN
  4. RENAMING COLUMN

```bash
ALTER TABLE EMPLOYEES ADD DOB DATE;
```

<img src="Images/alterTable.PNG">

#### **ADDING MULTIPLE COLUMNS**

```bash
ALTER TABLE EMPLOYEES ADD(SALARY NUMBER(10,2),COL1 NUMBER);
```

<img src="Images/addMultipleCol.PNG">

```bash
ALTER TABLE EMPLOYEES MODIFY COL1 VARCHAR2(10);
```

<img src="Images/modifyCol.PNG">

```bash
ALTER TABLE EMPLOYEES DROP COLUMN SALARY;
```

<img src="Images/dropCol.PNG">

```bash
ALTER TABLE EMPLOYEES SET UNUSED COLUMN COL1;
```

<img src="Images/setUnusedCol.PNG">

```bash
ALTER TABLE EMPLOYEES DROP UNUSED COLUMN;
```

<img src="Images/dropUnusedCol.PNG">

- ### **`DROP`**

```bash
DROP TABLE EMPLOYEES;
```

<img src="Images/dropTable.PNG">

- ### **`TRUNCATE`**

```bash
TRUNCATE TABLE EMPLOYEES;
```

<img src="Images/truncateTable.PNG">

- ### **`RENAME`**

```bash
ALTER TABLE EMPLOYEES RENAME COLUMN MOBILE_NO TO PHONE_NO;
```

<img src="Images/renameCol.PNG">

---

## Types of Constraints

- NOT NULL
- UNIQUE
- PRIMARY KEY
- CHECK
- FOREIGN KEY

- ### **`NOT NULL`**

```bash
CREATE TABLE SAMPLE1(
    COL1 NUMBER CONSTRAINT SAMPLE1_COL1_NN NOT NULL,
    COL2 NUMBER NOT NULL
);
```

```bash
ALTER TABLE EMPLOYEES MODIFY FIRSTNAME VARCHAR2(20) CONSTRAINT EMPLOYEES_FIRSTNAME_NN NOT NULL;
```

<img src="Images/notNullConstraints.PNG">

- ### **`UNIQUE`**

```bash
CREATE TABLE SAMPLE2(
    COL1 NUMBER CONSTRAINT SAMPLE2_COL1_UN UNIQUE,
    COL2 NUMBER,
    COL3 NUMBER,
    CONSTRAINT SAMPLE2_UNIQUE UNIQUE(COL2,COL3)
);
```

```bash
ALTER TABLE EMPLOYEES ADD CONSTRAINT EMPLOYEE_EMAIL_UN UNIQUE(EMAIL_ID);
ALTER TABLE EMPLOYEES ADD CONSTRAINT EMPLOYEE_MOBILE_UN UNIQUE(MOBILE_NO);
```

<img src="Images/uniqueConstraint.PNG">

- ### **`PRIMARY KEY`**

```bash
CREATE TABLE SAMPLE3(
    COL1 NUMBER CONSTRAINT SAMPLE3_PK PRIMARY KEY,
    COL2 NUMBER
);
```

```bash
ALTER TABLE EMPLOYEES ADD CONSTRAINT EMPLOYEES_PK PRIMARY KEY(EMP_ID);
```

<img src="Images/primaryKey.PNG">

- ### **`CHECK`**

```bash
CREATE TABLE SAMPLE4(
    COL1 NUMBER(10) CONSTRAINT SAMPLE4_COL1_CHECK CHECK(COL1 >= 10000)
);
```

```bash
ALTER TABLE EMPLOYEES
ADD CONSTRAINT EMP_GENDER_CHECK(GENDER IN('M','F'));
```

<img src="Images/checkConstraint.PNG">

- ### **`FOREIGN KEY`**

```bash
ALTER TABLE EMPLOYEES
ADD CONSTRAINT EMP_DEPT_REL FOREIGN KEY(DEPT_ID)
REFERENCES DEPARTMENTS(DEPT_ID) ON DELETE SET NULL;
```

<img src="Images/foreignKey.PNG">
<img src="Images/foreignKey2.PNG">

---

## Data Manipulation Language (DML) Statements

- INSERT
- UPDATE
- DELETE
- MERGE

- ### **`INSERT`**

```bash
INSERT INTO EMPLOYEES VALUES(101,'ABC','XYZ','ABC@GMAIL.COM','M','9999999999',SYSDATE,NULL);
```

<img src="Images/insertData.PNG">
<img src="Images/deptInsert.PNG">

#### **ADDING MULTIPLE DATA**

```bash
INSERT INTO DEPARTMENTS VALUES(&id,'&name');
```

`/` - To continue.

`COMMIT;` - To save all changes.

<img src="Images/multipleRowData.PNG">
<br/>
<img src="Images/deptDisplayRows.PNG">

```bash
INSERT INTO EMPLOYEES(EMP_ID,FIRSTNAME,LASTNAME,GENDER,DEPT_ID) VALUES(102,'PQR','LMN','M',30);
```

- ### **`UPDATE`**

```bash
UPDATE EMPLOYEES SET DEPT_ID = 40 WHERE EMP_ID = 101;
```

<img src="Images/updateRow.PNG">

```bash
UPDATE EMPLOYEES SET PHONE_NO = '9898989898',LASTNAME = 'JKL',
    DATE_OF_JOINING = '20-DEC-16'
    WHERE EMP_ID = 102;
```

<img src="Images/updateRow2.PNG">

- ### **`DELETE`**

```bash
DELETE FROM EMPLOYEES WHERE EMP_ID = 102;
```

<img src="Images/deleteRow.PNG">

- ### **`MERGE`**

<img src="Images/twoTables.PNG">

```bash
MERGE INTO COPY_EMP C
  USING EMPLOYEES E
  ON (E.EMP_ID = C.EMP_ID)
  WHEN MATCHED THEN
  UPDATE SET
  C.FIRSTNAME = E.FIRSTNAME,
  C.LASTNAME = E.LASTNAME,
  C.PHONE_NO = E.PHONE_NO,
  C.DEPT_ID = E.DEPT_ID
  WHEN NOT MATCHED THEN
  INSERT VALUES(E.EMP_ID, E.FIRSTNAME, E.LASTNAME, E.PHONE_NO, E.DEPT_ID);
```

<img src="Images/mergeTable.PNG">
