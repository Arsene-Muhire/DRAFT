# Flight Booking and Management Digital Tool

## 📋 Project Overview

A comprehensive Oracle PL/SQL database system designed to automate flight booking operations, manage seat allocation in real-time, implement dynamic pricing, and handle bookings efficiently. This project serves as the capstone for the Database Development with PL/SQL (INSY 8311) course.

**Author:** MURAHIRA MUHIRE Arsene  
**Student ID:** 27656  
**Institution:** Adventist University of Central Africa (AUCA)  
**Lecturer:** Eric Maniraguha  
**Academic Year:** 2025-2026, Semester I  
**Completion Date:** December 7, 2025

---



## 🎯 Problem Statement

Many flight booking systems struggle with:
- Manual seat management
- Fixed pricing models
- Inefficient handling of cancellations and waitlists
- Lack of real-time availability tracking

Solution: An automated flight booking system that addresses these challenges through intelligent automation and data-driven decision making.
Target Users: Airlines, travel agencies, and passengers
BI Potential: Real-time analytics on booking patterns, revenue optimization, customer behavior analysis, and operational efficiency metrics.

---

## ✨ Key Features

### Core Functionality
- ✅ **Automated Seat Allocation** - Real-time seat tracking and availability management
- ✅ **Dynamic Pricing Engine** - Prices adjust based on demand and booking time
- ✅ **Smart Waitlist Management** - Automatic promotion when cancellations occur
- ✅ **Overbooking Protection** - Calculated safe limits based on historical no-show data
- ✅ **Multi-Payment Support** - Credit Card, Debit Card, PayPal, Bank Transfer, Mobile Money

### Technical Highlights
- 🔐 **Comprehensive Auditing** - All database operations logged
- 📊 **Business Intelligence** - Advanced analytics and reporting
- 🛡️ **Business Rule Enforcement** - Weekend-only data modifications
- 🔄 **Transaction Management** - ACID compliance with rollback capabilities
- 📈 **Window Functions** - Advanced analytical queries

---

## 🗂️ Database Architecture

### Entity Relationship Model

The system is designed with 5 core entities in 3rd Normal Form (3NF):

```
PASSENGERS (586-687)
├── PASSENGER_ID (PK)
├── FULL_NAME
├── EMAIL
├── PHONE
├── PASSPORT_NUMBER
├── LOYALTY_POINTS
└── REGISTRATION_DATE

FLIGHTS (1-204)
├── FLIGHT_ID (PK)
├── FLIGHT_NUMBER
├── DEPARTURE_CITY
├── ARRIVAL_CITY
├── DEPARTURE_TIME
├── ARRIVAL_TIME
└── SEATS_AVAILABLE

BOOKINGS (1-102)
├── BOOKING_ID (PK)
├── PASSENGER_ID (FK)
├── FLIGHT_ID (FK)
├── BOOKING_DATE
└── STATUS (Confirmed/Cancelled/Waitlisted/Checked-in/No-show)

PAYMENTS (1-102)
├── BOOKING_ID (FK, PK)
├── AMOUNT
└── PAYMENT_METHOD

FLIGHT_HISTORY
├── FLIGHT_ID (FK)
├── PASSENGER_ID (FK)
└── TRAVEL_DATE
```

### Data Volume
- **Passengers:** 102 records
- **Flights:** 102 records
- **Bookings:** 102 records
- **Payments:** 102 records
- **Flight History:** 102 records

---

## 🔧 Technology Stack

- **Database:** Oracle 21c
- **Language:** PL/SQL
- **Tools:** SQL Developer, Oracle SQL*Plus
- **Version Control:** GitHub
- **Database Type:** Pluggable Database (PDB)

### Database Configuration
```
Database Name: wed_27656_arsene_flightBooking_db
Admin User: arsene
Tablespaces: DATA, INDEXES, TEMPORARY
Archive Logging: ENABLED
```

---

## 📁 Repository Structure

```
flight-booking-system/
├── README.md
├── database/
│   ├── scripts/
│   │   ├── 01_create_tables.sql
│   │   ├── 02_insert_passengers.sql
│   │   ├── 03_insert_flights.sql
│   │   ├── 04_insert_bookings.sql
│   │   ├── 05_insert_payments.sql
│   │   ├── 06_insert_flight_history.sql
│   │   └── 07_constraints_indexes.sql
│   └── documentation/
│       ├── data_dictionary.md
│       ├── er_diagram.png
│       └── normalization_notes.md
├── plsql/
│   ├── packages/
│   │   ├── flight_booking_pkg_spec.sql
│   │   └── flight_booking_pkg_body.sql
│   ├── procedures/
│   │   ├── create_booking.sql
│   │   ├── update_booking_status.sql
│   │   ├── cancel_booking.sql
│   │   ├── process_payment.sql
│   │   └── bulk_update_status.sql
│   ├── functions/
│   │   ├── calculate_booking_revenue.sql
│   │   ├── validate_passenger_email.sql
│   │   ├── get_flight_capacity.sql
│   │   ├── calculate_total_revenue.sql
│   │   └── get_passenger_name.sql
│   └── triggers/
│       ├── audit_log_setup.sql
│       ├── holiday_management.sql
│       ├── restriction_triggers.sql
│       └── compound_trigger.sql
├── queries/
│   ├── data_verification.sql
│   ├── analytics_queries.sql
│   ├── window_functions.sql
│   └── audit_queries.sql
├── tests/
│   ├── test_procedures.sql
│   ├── test_functions.sql
│   ├── test_triggers.sql
│   └── test_results.md
├── business_intelligence/
│   ├── bi_requirements.md
│   ├── dashboard_mockups/
│   └── kpi_definitions.md
└── screenshots/
    ├── database_structure/
    ├── data_samples/
    ├── test_results/
    └── audit_logs/
```

---



## 🔄 Business Process Model (Phase II)
Key Business Processes:

Passenger Registration → Create passenger profile with contact details
Flight Search & Selection → Browse available flights by route/date
Seat Availability Check → Real-time verification of seat capacity
Booking Creation → Reserve seat and generate booking confirmation
Payment Processing → Handle multiple payment methods securely
Booking Management → Allow cancellations, modifications, check-ins
Waitlist Management → Auto-promote when seats become available

System Actors:

Passengers: Book flights, make payments, manage bookings
Airline Staff: Manage flights, process refunds, handle special requests
System: Automate seat allocation, pricing, waitlist promotion

MIS Relevance:

Supports operational decisions (capacity planning)
Enables strategic decisions (pricing optimization)
Provides tactical insights (demand forecasting)

Deliverable: UML/BPMN diagram showing swimlanes for each actor

🗂️ Logical Database Design (Phase III)
Entity-Relationship Model
The system is designed with 5 core entities in 3rd Normal Form (3NF):
    


## Normalization:

1NF: All attributes atomic, no repeating groups
2NF: All non-key attributes fully dependent on primary key
3NF: No transitive dependencies

BI Considerations:

Fact Tables: BOOKINGS (transactional), PAYMENTS (financial)
Dimension Tables: PASSENGERS, FLIGHTS, TIME (derived)
Audit Trail: AUDIT_LOG for complete operation history

Deliverable: ER diagram + data dictionary


## Creat Pluggable Database(PHASE 4)
```sql
-- Run as SYSDBA
CREATE PLUGGABLE DATABASE WED_27656_arsene_FlightMS_DB
ADMIN USER admin_arsene IDENTIFIED BY arsene
ROLES = (DBA)
FILE_NAME_CONVERT = (
    'C:\\0RACLE21\\ORADATA\\ORCL2021\\PDBSEED\\',
    'C:\\0RACLE21\\ORADATA\\ORCL2021\\WED_27656_arsene_FlightMS_DB\\'
);

ALTER PLUGGABLE DATABASE WED_27656_arsene_FlightMS_DB OPEN;
ALTER PLUGGABLE DATABASE WED_27656_arsene_FlightMS_DB SAVE STATE;
ALTER SESSION SET CONTAINER = WED_27656_arsene_FlightMS_DB;


```


**Screenshot:** *[Insert screenshot of PDB creation success]*

---

#### 3. Create Tablespaces
```sql
-- Create DATA tablespace
CREATE TABLESPACE flight_data
DATAFILE 'flight_data01.dbf' SIZE 500M
AUTOEXTEND ON NEXT 50M MAXSIZE 2G;

-- Create INDEX tablespace
CREATE TABLESPACE flight_indexes
DATAFILE 'flight_indexes01.dbf' SIZE 200M
AUTOEXTEND ON NEXT 20M MAXSIZE 1G;

-- Create TEMP tablespace
CREATE TEMPORARY TABLESPACE flight_temp
TEMPFILE 'flight_temp01.dbf' SIZE 200M
AUTOEXTEND ON NEXT 20M MAXSIZE 1G;
```

**Screenshot:** *[Insert screenshot of tablespace creation]*

---

#### 4. Execute Database Scripts

**Create Tables:**
```sql
-- Connect to your PDB
CONN arsene/arsene@wed_27656_arsene_flightBooking_db

-- Create PASSENGERS table
CREATE TABLE PASSENGERS (
    PASSENGER_ID NUMBER PRIMARY KEY,
    FULL_NAME VARCHAR2(100) NOT NULL,
    EMAIL VARCHAR2(100) NOT NULL UNIQUE,
    PHONE VARCHAR2(20),
    PASSPORT_NUMBER VARCHAR2(20),
    LOYALTY_POINTS NUMBER DEFAULT 0,
    REGISTRATION_DATE DATE DEFAULT SYSDATE
);

-- Create FLIGHTS table
CREATE TABLE FLIGHTS (
    FLIGHT_ID NUMBER PRIMARY KEY,
    FLIGHT_NUMBER VARCHAR2(20) NOT NULL,
    DEPARTURE_CITY VARCHAR2(50) NOT NULL,
    ARRIVAL_CITY VARCHAR2(50) NOT NULL,
    DEPARTURE_TIME DATE NOT NULL,
    ARRIVAL_TIME DATE NOT NULL,
    SEATS_AVAILABLE NUMBER
);

-- Create BOOKINGS table
CREATE TABLE BOOKINGS (
    BOOKING_ID NUMBER PRIMARY KEY,
    PASSENGER_ID NUMBER NOT NULL,
    FLIGHT_ID NUMBER NOT NULL,
    BOOKING_DATE DATE DEFAULT SYSDATE,
    STATUS VARCHAR2(20) CHECK (STATUS IN ('Confirmed', 'Cancelled', 'Waitlisted', 'Checked-in', 'No-show')),
    CONSTRAINT FK_BOOKING_PASSENGER FOREIGN KEY (PASSENGER_ID) REFERENCES PASSENGERS(PASSENGER_ID),
    CONSTRAINT FK_BOOKING_FLIGHT FOREIGN KEY (FLIGHT_ID) REFERENCES FLIGHTS(FLIGHT_ID)
);

-- Create PAYMENTS table
CREATE TABLE PAYMENTS (
    BOOKING_ID NUMBER PRIMARY KEY,
    AMOUNT NUMBER(10,2) NOT NULL CHECK (AMOUNT > 0),
    PAYMENT_METHOD VARCHAR2(50) NOT NULL,
    CONSTRAINT FK_PAYMENT_BOOKING FOREIGN KEY (BOOKING_ID) REFERENCES BOOKINGS(BOOKING_ID)
);

-- Create FLIGHT_HISTORY table
CREATE TABLE FLIGHT_HISTORY (
    FLIGHT_ID NUMBER NOT NULL,
    PASSENGER_ID NUMBER NOT NULL,
    TRAVEL_DATE DATE NOT NULL,
    CONSTRAINT FK_HISTORY_FLIGHT FOREIGN KEY (FLIGHT_ID) REFERENCES FLIGHTS(FLIGHT_ID),
    CONSTRAINT FK_HISTORY_PASSENGER FOREIGN KEY (PASSENGER_ID) REFERENCES PASSENGERS(PASSENGER_ID)
);
```

**Screenshot:** *[Insert screenshot of table creation success]*

---

**Verify Table Structure:**
```sql
-- Check all tables created
SELECT TABLE_NAME FROM USER_TABLES ORDER BY TABLE_NAME;

-- Describe key tables
DESCRIBE PASSENGERS;
DESCRIBE FLIGHTS;
DESCRIBE BOOKINGS;
```

**Screenshot:** *[Insert screenshot showing all tables and structure]*

---

#### 5. Load Data into Tables

**Check Row Counts After Loading:**
```sql
SELECT 'PASSENGERS' AS TABLE_NAME, COUNT(*) AS ROW_COUNT FROM PASSENGERS
UNION ALL
SELECT 'FLIGHTS', COUNT(*) FROM FLIGHTS
UNION ALL
SELECT 'BOOKINGS', COUNT(*) FROM BOOKINGS
UNION ALL
SELECT 'PAYMENTS', COUNT(*) FROM PAYMENTS
UNION ALL
SELECT 'FLIGHT_HISTORY', COUNT(*) FROM FLIGHT_HISTORY
ORDER BY TABLE_NAME;
```

**Expected Output:**
```
TABLE_NAME       ROW_COUNT
---------------- ---------
BOOKINGS         102
FLIGHTS          102
FLIGHT_HISTORY   102
PASSENGERS       102
PAYMENTS         102
```

**Screenshot:** *[Insert screenshot of row counts]*

---

**Sample Data Verification:**
```sql
-- View sample passengers
SELECT * FROM PASSENGERS WHERE ROWNUM <= 5;

-- View sample bookings with details
SELECT 
    B.BOOKING_ID,
    P.FULL_NAME,
    F.FLIGHT_NUMBER,
    F.DEPARTURE_CITY || ' -> ' || F.ARRIVAL_CITY AS ROUTE,
    B.STATUS,
    PAY.AMOUNT
FROM BOOKINGS B
JOIN PASSENGERS P ON B.PASSENGER_ID = P.PASSENGER_ID
JOIN FLIGHTS F ON B.FLIGHT_ID = F.FLIGHT_ID
JOIN PAYMENTS PAY ON B.BOOKING_ID = PAY.BOOKING_ID
WHERE ROWNUM <= 10
ORDER BY B.BOOKING_DATE DESC;
```

**Screenshot:** *[Insert screenshot of sample data]*

---

## 📊 Data Integrity Verification

### Check Foreign Key Relationships

**Verify all bookings have valid passengers:**
```sql
SELECT COUNT(*) AS orphaned_bookings
FROM BOOKINGS B
WHERE NOT EXISTS (
    SELECT 1 FROM PASSENGERS P 
    WHERE P.PASSENGER_ID = B.PASSENGER_ID
);
-- Expected: 0
```

**Screenshot:** *[Insert screenshot showing 0 orphaned records]*

---

**Verify all bookings have valid flights:**
```sql
SELECT COUNT(*) AS orphaned_bookings
FROM BOOKINGS B
WHERE NOT EXISTS (
    SELECT 1 FROM FLIGHTS F 
    WHERE F.FLIGHT_ID = B.FLIGHT_ID
);
-- Expected: 0
```

**Screenshot:** *[Insert screenshot showing 0 orphaned records]*

---

**Verify all payments have valid bookings:**
```sql
SELECT COUNT(*) AS orphaned_payments
FROM PAYMENTS P
WHERE NOT EXISTS (
    SELECT 1 FROM BOOKINGS B 
    WHERE B.BOOKING_ID = P.BOOKING_ID
);
-- Expected: 0
```

**Screenshot:** *[Insert screenshot showing 0 orphaned records]*

---

### Check for Duplicate Primary Keys

```sql
-- Check for duplicate booking IDs
SELECT BOOKING_ID, COUNT(*) AS duplicate_count
FROM BOOKINGS
GROUP BY BOOKING_ID
HAVING COUNT(*) > 1;
-- Expected: No rows (all IDs are unique)
```

**Screenshot:** *[Insert screenshot showing no duplicates]*

---

### Verify Constraints

**Check booking statuses are valid:**
```sql
SELECT DISTINCT STATUS, COUNT(*) AS COUNT
FROM BOOKINGS
GROUP BY STATUS
ORDER BY COUNT DESC;
```

**Expected Output:**
```
STATUS          COUNT
--------------- -----
Confirmed       45
Checked-in      18
Cancelled       17
Waitlisted      12
No-show         10
```

**Screenshot:** *[Insert screenshot of status distribution]*

---

**Check payment methods distribution:**
```sql
SELECT 
    PAYMENT_METHOD,
    COUNT(*) AS TRANSACTION_COUNT,
    TO_CHAR(SUM(AMOUNT), '$999,999.99') AS TOTAL_REVENUE,
    TO_CHAR(AVG(AMOUNT), '$999.99') AS AVG_PAYMENT
FROM PAYMENTS
GROUP BY PAYMENT_METHOD
ORDER BY TRANSACTION_COUNT DESC;
```

**Screenshot:** *[Insert screenshot of payment method analytics]*

---

**Verify no negative payment amounts:**
```sql
SELECT COUNT(*) AS invalid_amounts
FROM PAYMENTS
WHERE AMOUNT <= 0;
-- Expected: 0
```

**Screenshot:** *[Insert screenshot showing 0 invalid amounts]*

---

## 🔧 PL/SQL Package Implementation

### Install the Package

#### Create Package Specification
```sql
CREATE OR REPLACE PACKAGE FLIGHT_BOOKING_PKG AS
    -- Custom Exceptions
    E_INVALID_BOOKING EXCEPTION;
    E_PASSENGER_NOT_FOUND EXCEPTION;
    E_INVALID_PAYMENT EXCEPTION;
    
    -- Procedures
    PROCEDURE CREATE_BOOKING(
        P_PASSENGER_ID IN NUMBER,
        P_FLIGHT_ID IN NUMBER,
        P_BOOKING_ID OUT NUMBER,
        P_STATUS OUT VARCHAR2
    );
    
    PROCEDURE UPDATE_BOOKING_STATUS(
        P_BOOKING_ID IN NUMBER,
        P_NEW_STATUS IN VARCHAR2,
        P_SUCCESS OUT BOOLEAN
    );
    
    PROCEDURE CANCEL_BOOKING(
        P_BOOKING_ID IN NUMBER,
        P_REFUND_AMOUNT OUT NUMBER
    );
    
    -- Functions
    FUNCTION CALCULATE_BOOKING_REVENUE(
        P_BOOKING_ID IN NUMBER
    ) RETURN NUMBER;
    
    FUNCTION GET_FLIGHT_CAPACITY(
        P_FLIGHT_ID IN NUMBER
    ) RETURN NUMBER;
    
    FUNCTION GET_PASSENGER_NAME(
        P_PASSENGER_ID IN NUMBER
    ) RETURN VARCHAR2;
    
END FLIGHT_BOOKING_PKG;
/
```

**Screenshot:** *[Insert screenshot of package spec compilation success]*

---

#### Verify Package Created
```sql
SELECT OBJECT_NAME, OBJECT_TYPE, STATUS
FROM USER_OBJECTS
WHERE OBJECT_NAME = 'FLIGHT_BOOKING_PKG';
```

**Expected Output:**
```
OBJECT_NAME              OBJECT_TYPE      STATUS
------------------------ ---------------- -------
FLIGHT_BOOKING_PKG       PACKAGE          VALID
FLIGHT_BOOKING_PKG       PACKAGE BODY     VALID
```

**Screenshot:** *[Insert screenshot of package objects]*

---

## 🧪 Testing PL/SQL Procedures and Functions

### Test 1: Create New Booking

```sql
SET SERVEROUTPUT ON SIZE UNLIMITED;

DECLARE
    V_BOOKING_ID NUMBER;
    V_STATUS VARCHAR2(100);
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 1: Create Booking ===');
    
    FLIGHT_BOOKING_PKG.CREATE_BOOKING(
        P_PASSENGER_ID => 586,
        P_FLIGHT_ID => 1,
        P_BOOKING_ID => V_BOOKING_ID,
        P_STATUS => V_STATUS
    );
    
    DBMS_OUTPUT.PUT_LINE('Result: ' || V_STATUS);
    DBMS_OUTPUT.PUT_LINE('New Booking ID: ' || V_BOOKING_ID);
END;
/
```

**Expected Output:**
```
=== TEST 1: Create Booking ===
Booking created successfully. Booking ID: 103
Result: SUCCESS
New Booking ID: 103
```

**Screenshot:** *[Insert screenshot of test output]*

---

**Verify the booking was created:**
```sql
SELECT * FROM BOOKINGS WHERE BOOKING_ID = 103;
```

**Screenshot:** *[Insert screenshot showing new booking record]*

---

### Test 2: Update Booking Status

```sql
DECLARE
    V_SUCCESS BOOLEAN;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 2: Update Booking Status ===');
    
    FLIGHT_BOOKING_PKG.UPDATE_BOOKING_STATUS(
        P_BOOKING_ID => 1,
        P_NEW_STATUS => 'Checked-in',
        P_SUCCESS => V_SUCCESS
    );
    
    IF V_SUCCESS THEN
        DBMS_OUTPUT.PUT_LINE('Status update: SUCCESS');
    ELSE
        DBMS_OUTPUT.PUT_LINE('Status update: FAILED');
    END IF;
END;
/
```

**Screenshot:** *[Insert screenshot of update test]*

---

**Verify the status changed:**
```sql
SELECT BOOKING_ID, STATUS FROM BOOKINGS WHERE BOOKING_ID = 1;
```

**Screenshot:** *[Insert screenshot showing updated status]*

---

### Test 3: Calculate Booking Revenue

```sql
DECLARE
    V_REVENUE NUMBER;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 3: Calculate Revenue ===');
    
    V_REVENUE := FLIGHT_BOOKING_PKG.CALCULATE_BOOKING_REVENUE(1);
    DBMS_OUTPUT.PUT_LINE('Booking 1 Revenue: $' || V_REVENUE);
END;
/
```

**Expected Output:**
```
=== TEST 3: Calculate Revenue ===
Booking 1 Revenue: $450
```

**Screenshot:** *[Insert screenshot of revenue calculation]*

---

### Test 4: Get Flight Capacity

```sql
DECLARE
    V_CAPACITY NUMBER;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 4: Flight Capacity ===');
    
    V_CAPACITY := FLIGHT_BOOKING_PKG.GET_FLIGHT_CAPACITY(1);
    DBMS_OUTPUT.PUT_LINE('Available seats on Flight 1: ' || V_CAPACITY);
END;
/
```

**Screenshot:** *[Insert screenshot of capacity check]*

---

### Test 5: Get Passenger Name (Lookup Function)

```sql
DECLARE
    V_NAME VARCHAR2(200);
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 5: Get Passenger Name ===');
    
    V_NAME := FLIGHT_BOOKING_PKG.GET_PASSENGER_NAME(586);
    DBMS_OUTPUT.PUT_LINE('Passenger Name: ' || V_NAME);
END;
/
```

**Screenshot:** *[Insert screenshot of lookup function]*

---

### Test 6: Cancel Booking with Refund

```sql
DECLARE
    V_REFUND NUMBER;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 6: Cancel Booking ===');
    
    FLIGHT_BOOKING_PKG.CANCEL_BOOKING(
        P_BOOKING_ID => 2,
        P_REFUND_AMOUNT => V_REFUND
    );
    
    DBMS_OUTPUT.PUT_LINE('Refund Amount: $' || ROUND(V_REFUND, 2));
END;
/
```

**Screenshot:** *[Insert screenshot of cancellation test]*

---

**Verify cancellation in database:**
```sql
SELECT BOOKING_ID, STATUS FROM BOOKINGS WHERE BOOKING_ID = 2;
```

**Screenshot:** *[Insert screenshot showing cancelled status]*

---

## 📈 Window Functions and Analytics

### Query 1: Rank Passengers by Spending

```sql
SELECT 
    P.PASSENGER_ID,
    P.FULL_NAME AS PASSENGER_NAME,
    SUM(PAY.AMOUNT) AS TOTAL_SPENT,
    ROW_NUMBER() OVER (ORDER BY SUM(PAY.AMOUNT) DESC) AS ROW_NUM,
    RANK() OVER (ORDER BY SUM(PAY.AMOUNT) DESC) AS RANK_NUM,
    DENSE_RANK() OVER (ORDER BY SUM(PAY.AMOUNT) DESC) AS DENSE_RANK_NUM
FROM PASSENGERS P
JOIN BOOKINGS B ON P.PASSENGER_ID = B.PASSENGER_ID
JOIN PAYMENTS PAY ON B.BOOKING_ID = PAY.BOOKING_ID
GROUP BY P.PASSENGER_ID, P.FULL_NAME
ORDER BY TOTAL_SPENT DESC
FETCH FIRST 10 ROWS ONLY;
```

**Screenshot:** *[Insert screenshot of top 10 spending passengers]*

---

### Query 2: Payment Trends with LAG/LEAD

```sql
SELECT 
    BOOKING_ID,
    AMOUNT,
    PAYMENT_METHOD,
    LAG(AMOUNT, 1) OVER (ORDER BY BOOKING_ID) AS PREVIOUS_PAYMENT,
    LEAD(AMOUNT, 1) OVER (ORDER BY BOOKING_ID) AS NEXT_PAYMENT,
    AMOUNT - LAG(AMOUNT, 1) OVER (ORDER BY BOOKING_ID) AS PAYMENT_CHANGE
FROM PAYMENTS
WHERE ROWNUM <= 10
ORDER BY BOOKING_ID;
```

**Screenshot:** *[Insert screenshot of payment trends]*

---

### Query 3: Running Total of Bookings per Flight

```sql
SELECT 
    F.FLIGHT_ID,
    F.FLIGHT_NUMBER,
    F.DEPARTURE_CITY || ' -> ' || F.ARRIVAL_CITY AS ROUTE,
    COUNT(B.BOOKING_ID) AS TOTAL_BOOKINGS,
    SUM(COUNT(B.BOOKING_ID)) OVER (ORDER BY F.FLIGHT_ID) AS RUNNING_TOTAL,
    ROUND(RATIO_TO_REPORT(COUNT(B.BOOKING_ID)) OVER () * 100, 2) AS PCT_OF_TOTAL
FROM FLIGHTS F
LEFT JOIN BOOKINGS B ON F.FLIGHT_ID = B.FLIGHT_ID
GROUP BY F.FLIGHT_ID, F.FLIGHT_NUMBER, F.DEPARTURE_CITY, F.ARRIVAL_CITY
ORDER BY TOTAL_BOOKINGS DESC
FETCH FIRST 15 ROWS ONLY;
```

**Screenshot:** *[Insert screenshot of flight booking analysis]*

---

### Query 4: Revenue by Payment Method with Partitioning

```sql
SELECT 
    PAYMENT_METHOD,
    BOOKING_ID,
    AMOUNT,
    SUM(AMOUNT) OVER (PARTITION BY PAYMENT_METHOD) AS METHOD_TOTAL,
    ROUND(AVG(AMOUNT) OVER (PARTITION BY PAYMENT_METHOD), 2) AS METHOD_AVERAGE,
    ROW_NUMBER() OVER (PARTITION BY PAYMENT_METHOD ORDER BY AMOUNT DESC) AS RANK_IN_METHOD
FROM PAYMENTS
WHERE ROWNUM <= 20
ORDER BY PAYMENT_METHOD, RANK_IN_METHOD;
```

**Screenshot:** *[Insert screenshot of partitioned revenue analysis]*

---

## PHASE VIII: 🔒 Triggers and Business Rules

### Create Audit Log Table

```sql
CREATE TABLE AUDIT_LOG (
    AUDIT_ID NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    TABLE_NAME VARCHAR2(50),
    OPERATION VARCHAR2(10),
    USER_NAME VARCHAR2(50),
    OPERATION_DATE TIMESTAMP DEFAULT SYSTIMESTAMP,
    STATUS VARCHAR2(20),
    ERROR_MESSAGE VARCHAR2(500)
);
```

**Screenshot:** *[Insert screenshot of audit table creation]*

---

### Create Holiday Management Table

```sql
CREATE TABLE PUBLIC_HOLIDAYS (
    HOLIDAY_DATE DATE PRIMARY KEY,
    HOLIDAY_NAME VARCHAR2(100),
    COUNTRY VARCHAR2(50) DEFAULT 'Rwanda'
);

-- Insert upcoming holidays
INSERT INTO PUBLIC_HOLIDAYS VALUES (TO_DATE('2025-12-25', 'YYYY-MM-DD'), 'Christmas Day', 'Rwanda');
INSERT INTO PUBLIC_HOLIDAYS VALUES (TO_DATE('2025-12-26', 'YYYY-MM-DD'), 'Boxing Day', 'Rwanda');
INSERT INTO PUBLIC_HOLIDAYS VALUES (TO_DATE('2026-01-01', 'YYYY-MM-DD'), 'New Year Day', 'Rwanda');
COMMIT;
```

**Verify holidays inserted:**
```sql
SELECT * FROM PUBLIC_HOLIDAYS ORDER BY HOLIDAY_DATE;
```

**Screenshot:** *[Insert screenshot of holiday table data]*

---

### Create Restriction Check Function

```sql
CREATE OR REPLACE FUNCTION IS_OPERATION_ALLOWED
RETURN BOOLEAN
IS
    V_DAY_NAME VARCHAR2(10);
    V_IS_HOLIDAY NUMBER;
BEGIN
    -- Get current day name
    V_DAY_NAME := TO_CHAR(SYSDATE, 'DY', 'NLS_DATE_LANGUAGE=ENGLISH');
    
    -- Check if today is a weekday (Mon-Fri)
    IF V_DAY_NAME IN ('MON', 'TUE', 'WED', 'THU', 'FRI') THEN
        RETURN FALSE; -- Operations not allowed on weekdays
    END IF;
    
    -- Check if today is a public holiday
    SELECT COUNT(*)
    INTO V_IS_HOLIDAY
    FROM PUBLIC_HOLIDAYS
    WHERE TRUNC(HOLIDAY_DATE) = TRUNC(SYSDATE);
    
    IF V_IS_HOLIDAY > 0 THEN
        RETURN FALSE; -- Operations not allowed on holidays
    END IF;
    
    RETURN TRUE; -- Operations allowed (weekend, not holiday)
END;
/
```

**Screenshot:** *[Insert screenshot of function compilation]*

---

### Create Restriction Trigger for BOOKINGS

```sql
CREATE OR REPLACE TRIGGER TRG_BOOKING_RESTRICTION
BEFORE INSERT OR UPDATE OR DELETE ON BOOKINGS
FOR EACH ROW
DECLARE
    V_OPERATION VARCHAR2(10);
BEGIN
    -- Determine operation type
    IF INSERTING THEN
        V_OPERATION := 'INSERT';
    ELSIF UPDATING THEN
        V_OPERATION := 'UPDATE';
    ELSIF DELETING THEN
        V_OPERATION := 'DELETE';
    END IF;
    
    -- Check if operation is allowed
    IF NOT IS_OPERATION_ALLOWED() THEN
        -- Log the denied attempt
        INSERT INTO AUDIT_LOG (TABLE_NAME, OPERATION, USER_NAME, STATUS, ERROR_MESSAGE)
        VALUES ('BOOKINGS', V_OPERATION, USER, 'DENIED', 
                'Operation not allowed on weekdays or public holidays');
        COMMIT;
        
        -- Raise error to block the operation
        RAISE_APPLICATION_ERROR(-20100, 
            'OPERATION DENIED: Cannot perform ' || V_OPERATION || 
            ' on BOOKINGS table on weekdays (Mon-Fri) or public holidays. ' ||
            'Please try again on weekends.');
    ELSE
        -- Log the allowed operation
        INSERT INTO AUDIT_LOG (TABLE_NAME, OPERATION, USER_NAME, STATUS)
        VALUES ('BOOKINGS', V_OPERATION, USER, 'ALLOWED');
        COMMIT;
    END IF;
END;
/
```

**Screenshot:** *[Insert screenshot of trigger creation]*

---

### Testing the Restriction Trigger

#### Test 1: Attempt INSERT on Weekday (Should FAIL)

```sql
-- Simulate weekday by altering session date (if needed)
-- Or run this test on an actual Monday-Friday

INSERT INTO BOOKINGS (BOOKING_ID, PASSENGER_ID, FLIGHT_ID, BOOKING_DATE, STATUS)
VALUES (999, 586, 1, SYSDATE, 'Confirmed');
```

**Expected Error:**
```
ORA-20100: OPERATION DENIED: Cannot perform INSERT on BOOKINGS table on weekdays (Mon-Fri) or public holidays. Please try again on weekends.
```

**Screenshot:** *[Insert screenshot of denied weekday operation]*

---

**Check audit log for denied attempt:**
```sql
SELECT 
    AUDIT_ID,
    TABLE_NAME,
    OPERATION,
    USER_NAME,
    TO_CHAR(OPERATION_DATE, 'YYYY-MM-DD HH24:MI:SS') AS OPERATION_TIME,
    STATUS,
    ERROR_MESSAGE
FROM AUDIT_LOG
WHERE STATUS = 'DENIED'
ORDER BY OPERATION_DATE DESC
FETCH FIRST 5 ROWS ONLY;
```

**Screenshot:** *[Insert screenshot of audit log showing DENIED status]*

---

#### Test 2: Attempt INSERT on Weekend (Should SUCCEED)

```sql
-- Run this test on Saturday or Sunday

INSERT INTO BOOKINGS (BOOKING_ID, PASSENGER_ID, FLIGHT_ID, BOOKING_DATE, STATUS)
VALUES (999, 586, 1, SYSDATE, 'Confirmed');

COMMIT;
```

**Expected Output:**
```
1 row inserted.
```

**Screenshot:** *[Insert screenshot of successful weekend operation]*

---

**Check audit log for allowed operation:**
```sql
SELECT 
    AUDIT_ID,
    TABLE_NAME,
    OPERATION,
    USER_NAME,
    STATUS
FROM AUDIT_LOG
WHERE STATUS = 'ALLOWED'
ORDER BY OPERATION_DATE DESC
FETCH FIRST 5 ROWS ONLY;
```

**Screenshot:** *[Insert screenshot of audit log showing ALLOWED status]*

---

#### Test 3: Attempt INSERT on Public Holiday (Should FAIL)

```sql
-- Set session date to a public holiday for testing
ALTER SESSION SET NLS_DATE_FORMAT = 'YYYY-MM-DD HH24:MI:SS';

-- Attempt operation on Christmas Day
INSERT INTO BOOKINGS (BOOKING_ID, PASSENGER_ID, FLIGHT_ID, BOOKING_DATE, STATUS)
VALUES (998, 587, 2, TO_DATE('2025-12-25', 'YYYY-MM-DD'), 'Confirmed');
```

**Expected Error:**
```
ORA-20100: OPERATION DENIED: Cannot perform INSERT on BOOKINGS table on weekdays (Mon-Fri) or public holidays. Please try again on weekends.
```

**Screenshot:** *[Insert screenshot of denied holiday operation]*

---

### Comprehensive Audit Report

```sql
SELECT 
    TO_CHAR(OPERATION_DATE, 'YYYY-MM-DD') AS OPERATION_DAY,
    STATUS,
    COUNT(*) AS ATTEMPT_COUNT,
    LISTAGG(OPERATION, ', ') WITHIN GROUP (ORDER BY OPERATION) AS OPERATIONS
FROM AUDIT_LOG
GROUP BY TO_CHAR(OPERATION_DATE, 'YYYY-MM-DD'), STATUS
ORDER BY OPERATION_DAY DESC, STATUS



TEST 3: Cancel Booking Procedure
sql-- TEST 3: Cancel Booking
DECLARE
    V_REFUND NUMBER;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 3: Cancel Booking ===');
    
    FLIGHT_BOOKING_PKG.CANCEL_BOOKING(
        P_BOOKING_ID => 2,
        P_REFUND_AMOUNT => V_REFUND
    );
    
    DBMS_OUTPUT.PUT_LINE('Refund Amount: $' || ROUND(V_REFUND, 2));
    DBMS_OUTPUT.PUT_LINE('');
END;
/
Expected Output:
=== TEST 3: Cancel Booking ===
Booking cancelled. Refund amount: $405
Refund Amount: $405.00
Screenshot: [Insert screenshot of CANCEL_BOOKING test output]

Verify Cancellation
sql-- Check booking status after cancellation
SELECT B.BOOKING_ID, B.STATUS, PAY.AMOUNT AS ORIGINAL_AMOUNT
FROM BOOKINGS B
JOIN PAYMENTS PAY ON B.BOOKING_ID = PAY.BOOKING_ID
WHERE B.BOOKING_ID = 2;
Screenshot: [Insert screenshot showing booking status as 'Cancelled']

TEST 4: Process Payment Procedure

sql-- TEST 4: Process Payment
DECLARE
    V_PAYMENT_ID NUMBER;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 4: Process Payment ===');
    
    FLIGHT_BOOKING_PKG.PROCESS_PAYMENT(
        P_BOOKING_ID => 103,
        P_AMOUNT => 550.00,
        P_PAYMENT_METHOD => 'Credit Card',
        P_PAYMENT_ID => V_PAYMENT_ID
    );
    
    DBMS_OUTPUT.PUT_LINE('Payment ID: ' || V_PAYMENT_ID);
    DBMS_OUTPUT.PUT_LINE('');
END;
/


Expected Output:
=== TEST 4: Process Payment ===
Payment processed: $550 via Credit Card
Payment ID: 103
Screenshot: [Insert screenshot of PROCESS_PAYMENT test output]

Verify Payment Record
sql-- Check the new payment record
SELECT BOOKING_ID, AMOUNT, PAYMENT_METHOD
FROM PAYMENTS
WHERE BOOKING_ID = 103;
Screenshot: [Insert screenshot showing payment record for booking 103]


TEST 5: Bulk Update Status Procedure
sql-- TEST 5: Bulk Update Status
DECLARE
    V_COUNT NUMBER;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 5: Bulk Update Status ===');
    
    FLIGHT_BOOKING_PKG.BULK_UPDATE_STATUS(
        P_OLD_STATUS => 'Waitlisted',
        P_NEW_STATUS => 'Confirmed',
        P_UPDATED_COUNT => V_COUNT
    );
    
    DBMS_OUTPUT.PUT_LINE('Total Records Updated: ' || V_COUNT);
    DBMS_OUTPUT.PUT_LINE('');
END;
/
Expected Output:


=== TEST 5: Bulk Update Status ===
Updated 12 bookings from Waitlisted to Confirmed
Total Records Updated: 12
Screenshot: [Insert screenshot of BULK_UPDATE_STATUS test output]

Verify Bulk Update
sql-- Check status distribution after bulk update
SELECT STATUS, COUNT(*) AS COUNT
FROM BOOKINGS
GROUP BY STATUS
ORDER BY COUNT DESC;
Screenshot: [Insert screenshot showing updated status distribution]


TEST 6: Calculate Booking Revenue Function


sql-- TEST 6: Calculate Revenue Function
DECLARE
    V_REVENUE NUMBER;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 6: Calculate Booking Revenue ===');
    
    V_REVENUE := FLIGHT_BOOKING_PKG.CALCULATE_BOOKING_REVENUE(1);
    
    DBMS_OUTPUT.PUT_LINE('Booking ID 1 Revenue: $' || ROUND(V_REVENUE, 2));
    DBMS_OUTPUT.PUT_LINE('');
END;
/

```

