# Flight Booking and Management Digital Tool


**Author:** MURAHIRA MUHIRE Arsene  
**Student ID:** 27656  
**Institution:** Adventist University of Central Africa (AUCA)  # Flight Booking and Management Digital Tool

## 📋 Project Overview

A comprehensive Oracle PL/SQL database system designed to automate flight booking operations, manage seat allocation in real-time, implement dynamic pricing, and handle bookings efficiently. This project serves as the capstone for the Database Development with PL/SQL (INSY 8311) course.

**Author:** MURAHIRA MUHIRE Arsene  
**Student ID:** 27656  
**Institution:** Adventist University of Central Africa (AUCA)  
**Lecturer:** Eric Maniraguha  
**Academic Year:** 2025-2026, Semester I  
**Completion Date:** December 7, 2025




## PHASE 1:🎯 Problem Statement

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




## Phase II: 🔄 Business Process Model 

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


    

## Phase III: 🗂️ Logical Database Design 

Entity-Relationship Model
The system is designed with 5 core entities in 3rd Normal Form (3NF):

### Entity Relationship Model

The system is designed with 5 core entities in 3rd Normal Form (3NF):


### Data Volume
- **Passengers:** 102 records
- **Flights:** 102 records
- **Bookings:** 102 records
- **Payments:** 102 records
- **Flight History:** 102 records



### Database Configuration
Database Name: wed_27656_arsene_flightBooking_db
Admin User: arsene
Tablespaces: DATA, INDEXES, TEMPORARY




## Normalization:

1NF: All attributes atomic, no repeating groups
2NF: All non-key attributes fully dependent on primary key
3NF: No transitive dependencies



## PHASE 4 Creat Pluggable Database
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

## phase  5: Table Implementation & Data Insertion

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

## 🔧 PHASE 6:  Database Interaction & Transaction 

```SQL
-- PART 2: PACKAGE SPECIFICATION (Public Interface)
-- =====================================================

CREATE OR REPLACE PACKAGE flight_booking_pkg AS
    -- Custom Exceptions
    e_invalid_booking EXCEPTION;
    e_insufficient_seats EXCEPTION;
    e_invalid_payment EXCEPTION;
    e_passenger_not_found EXCEPTION;
    
    -- Procedure Declarations
    PROCEDURE create_booking(
        p_passenger_id IN NUMBER,
        p_flight_id IN NUMBER,
        p_booking_date IN DATE DEFAULT SYSDATE,
        p_booking_id OUT NUMBER,
        p_status OUT VARCHAR2
    );
    
    PROCEDURE update_booking_status(
        p_booking_id IN NUMBER,
        p_new_status IN VARCHAR2,
        p_success OUT BOOLEAN
    );
    
    PROCEDURE cancel_booking(
        p_booking_id IN NUMBER,
        p_refund_amount OUT NUMBER
    );
    
    PROCEDURE process_payment(
        p_booking_id IN NUMBER,
        p_amount IN NUMBER,
        p_payment_method IN VARCHAR2,
        p_payment_id OUT NUMBER
    );
    
    PROCEDURE bulk_update_status(
        p_old_status IN VARCHAR2,
        p_new_status IN VARCHAR2,
        p_updated_count OUT NUMBER
    );
    
    -- Function Declarations
    FUNCTION calculate_booking_revenue(
        p_booking_id IN NUMBER
    ) RETURN NUMBER;
    
    FUNCTION validate_passenger_email(
        p_email IN VARCHAR2
    ) RETURN BOOLEAN;
    
    FUNCTION get_flight_capacity(
        p_flight_id IN NUMBER
    ) RETURN NUMBER;
    
    FUNCTION calculate_total_revenue(
        p_start_date IN DATE DEFAULT NULL,
        p_end_date IN DATE DEFAULT NULL
    ) RETURN NUMBER;
    
    FUNCTION get_passenger_name(
        p_passenger_id IN NUMBER
    ) RETURN VARCHAR2;
    
    -- Cursor-based Procedure
    PROCEDURE generate_booking_report(
        p_status IN VARCHAR2 DEFAULT NULL
    );
    
END flight_booking_pkg;
/

-- =====================================================
-- PART 3: PACKAGE BODY (Implementation)
-- =====================================================

CREATE OR REPLACE PACKAGE BODY flight_booking_pkg AS

    -- Private procedure for error logging
    PROCEDURE log_error(
        p_error_code IN NUMBER,
        p_error_msg IN VARCHAR2,
        p_proc_name IN VARCHAR2
    ) IS
        PRAGMA AUTONOMOUS_TRANSACTION;
    BEGIN
        INSERT INTO error_log (error_code, error_message, procedure_name)
        VALUES (p_error_code, p_error_msg, p_proc_name);
        COMMIT;
    END log_error;

    -- =====================================================
    -- PROCEDURE 1: Create Booking
    -- Purpose: Insert new booking with validation
    -- Parameters: IN (passenger_id, flight_id), OUT (booking_id, status)
    -- =====================================================
    PROCEDURE create_booking(
        p_passenger_id IN NUMBER,
        p_flight_id IN NUMBER,
        p_booking_date IN DATE DEFAULT SYSDATE,
        p_booking_id OUT NUMBER,
        p_status OUT VARCHAR2
    ) IS
        v_passenger_exists NUMBER;
        v_flight_exists NUMBER;
        v_next_booking_id NUMBER;
    BEGIN
        -- Validate passenger exists
        SELECT COUNT(*) INTO v_passenger_exists
        FROM passengers
        WHERE passenger_id = p_passenger_id;
        
        IF v_passenger_exists = 0 THEN
            RAISE e_passenger_not_found;
        END IF;
        
        -- Validate flight exists
        SELECT COUNT(*) INTO v_flight_exists
        FROM flights
        WHERE flight_id = p_flight_id;
        
        IF v_flight_exists = 0 THEN
            RAISE_APPLICATION_ERROR(-20001, 'Flight does not exist');
        END IF;
        
        -- Get next booking ID
        SELECT NVL(MAX(booking_id), 0) + 1 INTO v_next_booking_id
        FROM bookings;
        
        -- Insert booking
        INSERT INTO bookings (booking_id, passenger_id, flight_id, booking_date, status)
        VALUES (v_next_booking_id, p_passenger_id, p_flight_id, p_booking_date, 'Confirmed');
        
        p_booking_id := v_next_booking_id;
        p_status := 'SUCCESS';
        
        COMMIT;
        
        DBMS_OUTPUT.PUT_LINE('Booking created successfully. Booking ID: ' || p_booking_id);
        
    EXCEPTION
        WHEN e_passenger_not_found THEN
            p_status := 'FAILED - Passenger not found';
            log_error(SQLCODE, 'Passenger ID ' || p_passenger_id || ' not found', 'create_booking');
            ROLLBACK;
        WHEN OTHERS THEN
            p_status := 'FAILED - ' || SQLERRM;
            log_error(SQLCODE, SQLERRM, 'create_booking');
            ROLLBACK;
    END create_booking;

    -- =====================================================
    -- PROCEDURE 2: Update Booking Status
    -- Purpose: Change booking status with validation
    -- Parameters: IN (booking_id, new_status), OUT (success)
    -- =====================================================
    PROCEDURE update_booking_status(
        p_booking_id IN NUMBER,
        p_new_status IN VARCHAR2,
        p_success OUT BOOLEAN
    ) IS
        v_current_status VARCHAR2(20);
        v_booking_exists NUMBER;
    BEGIN
        -- Check if booking exists
        SELECT COUNT(*) INTO v_booking_exists
        FROM bookings
        WHERE booking_id = p_booking_id;
        
        IF v_booking_exists = 0 THEN
            RAISE e_invalid_booking;
        END IF;
        
        -- Get current status
        SELECT status INTO v_current_status
        FROM bookings
        WHERE booking_id = p_booking_id;
        
        -- Update status
        UPDATE bookings
        SET status = p_new_status
        WHERE booking_id = p_booking_id;
        
        p_success := TRUE;
        COMMIT;
        
        DBMS_OUTPUT.PUT_LINE('Booking ' || p_booking_id || ' status updated from ' || 
                            v_current_status || ' to ' || p_new_status);
        
    EXCEPTION
        WHEN e_invalid_booking THEN
            p_success := FALSE;
            log_error(-20002, 'Booking ID ' || p_booking_id || ' does not exist', 'update_booking_status');
            ROLLBACK;
        WHEN OTHERS THEN
            p_success := FALSE;
            log_error(SQLCODE, SQLERRM, 'update_booking_status');
            ROLLBACK;
    END update_booking_status;

    -- =====================================================
    -- PROCEDURE 3: Cancel Booking
    -- Purpose: Cancel booking and calculate refund
    -- Parameters: IN (booking_id), OUT (refund_amount)
    -- =====================================================
    PROCEDURE cancel_booking(
        p_booking_id IN NUMBER,
        p_refund_amount OUT NUMBER
    ) IS
        v_payment_amount NUMBER;
    BEGIN
        -- Get payment amount
        SELECT amount INTO v_payment_amount
        FROM payments
        WHERE booking_id = p_booking_id;
        
        -- Calculate refund (90% refund policy)
        p_refund_amount := v_payment_amount * 0.90;
        
        -- Update booking status
        UPDATE bookings
        SET status = 'Cancelled'
        WHERE booking_id = p_booking_id;
        
        COMMIT;
        
        DBMS_OUTPUT.PUT_LINE('Booking cancelled. Refund amount: $' || 
                            ROUND(p_refund_amount, 2));
        
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            p_refund_amount := 0;
            log_error(-20003, 'No payment found for booking ' || p_booking_id, 'cancel_booking');
            ROLLBACK;
        WHEN OTHERS THEN
            p_refund_amount := 0;
            log_error(SQLCODE, SQLERRM, 'cancel_booking');
            ROLLBACK;
    END cancel_booking;

    -- =====================================================
    -- PROCEDURE 4: Process Payment
    -- Purpose: Record payment for booking
    -- Parameters: IN (booking_id, amount, method), OUT (payment_id)
    -- =====================================================
    PROCEDURE process_payment(
        p_booking_id IN NUMBER,
        p_amount IN NUMBER,
        p_payment_method IN VARCHAR2,
        p_payment_id OUT NUMBER
    ) IS
        v_booking_exists NUMBER;
    BEGIN
        -- Validate amount
        IF p_amount <= 0 THEN
            RAISE e_invalid_payment;
        END IF;
        
        -- Check booking exists
        SELECT COUNT(*) INTO v_booking_exists
        FROM bookings
        WHERE booking_id = p_booking_id;
        
        IF v_booking_exists = 0 THEN
            RAISE e_invalid_booking;
        END IF;
        
        -- Insert payment (assuming PAYMENT_ID is auto-generated or managed)
        INSERT INTO payments (booking_id, amount, payment_method)
        VALUES (p_booking_id, p_amount, p_payment_method);
        
        p_payment_id := p_booking_id; -- Simplified for this example
        
        COMMIT;
        
        DBMS_OUTPUT.PUT_LINE('Payment processed: $' || p_amount || 
                            ' via ' || p_payment_method);
        
    EXCEPTION
        WHEN e_invalid_payment THEN
            p_payment_id := NULL;
            log_error(-20004, 'Invalid payment amount: ' || p_amount, 'process_payment');
            ROLLBACK;
        WHEN OTHERS THEN
            p_payment_id := NULL;
            log_error(SQLCODE, SQLERRM, 'process_payment');
            ROLLBACK;
    END process_payment;

    -- =====================================================
    -- PROCEDURE 5: Bulk Update Status (Bulk Operations)
    -- Purpose: Update multiple bookings at once
    -- Parameters: IN (old_status, new_status), OUT (updated_count)
    -- =====================================================
    PROCEDURE bulk_update_status(
        p_old_status IN VARCHAR2,
        p_new_status IN VARCHAR2,
        p_updated_count OUT NUMBER
    ) IS
        TYPE booking_id_tab IS TABLE OF bookings.booking_id%TYPE;
        v_booking_ids booking_id_tab;
    BEGIN
        -- Bulk collect booking IDs
        SELECT booking_id
        BULK COLLECT INTO v_booking_ids
        FROM bookings
        WHERE status = p_old_status;
        
        -- Bulk update
        FORALL i IN 1..v_booking_ids.COUNT
            UPDATE bookings
            SET status = p_new_status
            WHERE booking_id = v_booking_ids(i);
        
        p_updated_count := SQL%ROWCOUNT;
        
        COMMIT;
        
        DBMS_OUTPUT.PUT_LINE('Updated ' || p_updated_count || 
                            ' bookings from ' || p_old_status || 
                            ' to ' || p_new_status);
        
    EXCEPTION
        WHEN OTHERS THEN
            p_updated_count := 0;
            log_error(SQLCODE, SQLERRM, 'bulk_update_status');
            ROLLBACK;
    END bulk_update_status;

    -- =====================================================
    -- FUNCTION 1: Calculate Booking Revenue
    -- Purpose: Get total payment for a booking
    -- Returns: Payment amount
    -- =====================================================
    FUNCTION calculate_booking_revenue(
        p_booking_id IN NUMBER
    ) RETURN NUMBER IS
        v_revenue NUMBER := 0;
    BEGIN
        SELECT NVL(SUM(amount), 0)
        INTO v_revenue
        FROM payments
        WHERE booking_id = p_booking_id;
        
        RETURN v_revenue;
        
    EXCEPTION
        WHEN OTHERS THEN
            log_error(SQLCODE, SQLERRM, 'calculate_booking_revenue');
            RETURN 0;
    END calculate_booking_revenue;

    -- =====================================================
    -- FUNCTION 2: Validate Passenger Email
    -- Purpose: Check if email format is valid
    -- Returns: TRUE/FALSE
    -- =====================================================
    FUNCTION validate_passenger_email(
        p_email IN VARCHAR2
    ) RETURN BOOLEAN IS
    BEGIN
        -- Basic email validation (contains @ and .)
        IF p_email LIKE '%_@__%.__%' THEN
            RETURN TRUE;
        ELSE
            RETURN FALSE;
        END IF;
        
    EXCEPTION
        WHEN OTHERS THEN
            log_error(SQLCODE, SQLERRM, 'validate_passenger_email');
            RETURN FALSE;
    END validate_passenger_email;

    -- =====================================================
    -- FUNCTION 3: Get Flight Capacity
    -- Purpose: Calculate available seats on flight
    -- Returns: Number of available seats
    -- =====================================================
    FUNCTION get_flight_capacity(
        p_flight_id IN NUMBER
    ) RETURN NUMBER IS
        v_booked_seats NUMBER := 0;
        v_total_capacity NUMBER := 200; -- Assume 200 seats per flight
    BEGIN
        SELECT COUNT(*)
        INTO v_booked_seats
        FROM bookings
        WHERE flight_id = p_flight_id
        AND status IN ('Confirmed', 'Checked-in');
        
        RETURN v_total_capacity - v_booked_seats;
        
    EXCEPTION
        WHEN OTHERS THEN
            log_error(SQLCODE, SQLERRM, 'get_flight_capacity');
            RETURN 0;
    END get_flight_capacity;

    -- =====================================================
    -- FUNCTION 4: Calculate Total Revenue
    -- Purpose: Calculate revenue for date range
    -- Returns: Total revenue
    -- =====================================================
    FUNCTION calculate_total_revenue(
        p_start_date IN DATE DEFAULT NULL,
        p_end_date IN DATE DEFAULT NULL
    ) RETURN NUMBER IS
        v_total_revenue NUMBER := 0;
    BEGIN
        IF p_start_date IS NULL AND p_end_date IS NULL THEN
            SELECT NVL(SUM(amount), 0)
            INTO v_total_revenue
            FROM payments;
        ELSE
            SELECT NVL(SUM(p.amount), 0)
            INTO v_total_revenue
            FROM payments p
            JOIN bookings b ON p.booking_id = b.booking_id
            WHERE b.booking_date BETWEEN 
                  NVL(p_start_date, b.booking_date) AND 
                  NVL(p_end_date, b.booking_date);
        END IF;
        
        RETURN v_total_revenue;
        
    EXCEPTION
        WHEN OTHERS THEN
            log_error(SQLCODE, SQLERRM, 'calculate_total_revenue');
            RETURN 0;
    END calculate_total_revenue;

    -- =====================================================
    -- FUNCTION 5: Get Passenger Name (Lookup)
    -- Purpose: Retrieve passenger full name
    -- Returns: Full name
    -- =====================================================
    FUNCTION get_passenger_name(
        p_passenger_id IN NUMBER
    ) RETURN VARCHAR2 IS
        v_full_name VARCHAR2(200);
    BEGIN
        SELECT first_name || ' ' || last_name
        INTO v_full_name
        FROM passengers
        WHERE passenger_id = p_passenger_id;
        
        RETURN v_full_name;
        
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            RETURN 'Unknown Passenger';
        WHEN OTHERS THEN
            log_error(SQLCODE, SQLERRM, 'get_passenger_name');
            RETURN 'Error';
    END get_passenger_name;

    -- =====================================================
    -- PROCEDURE 6: Generate Booking Report (Cursor)
    -- Purpose: Display booking details using explicit cursor
    -- =====================================================
    PROCEDURE generate_booking_report(
        p_status IN VARCHAR2 DEFAULT NULL
    ) IS
        -- Explicit cursor
        CURSOR c_bookings IS
            SELECT b.booking_id, b.booking_date, b.status,
                   p.first_name || ' ' || p.last_name AS passenger_name,
                   f.flight_number, f.origin, f.destination,
                   pay.amount
            FROM bookings b
            JOIN passengers p ON b.passenger_id = p.passenger_id
            JOIN flights f ON b.flight_id = f.flight_id
            LEFT JOIN payments pay ON b.booking_id = pay.booking_id
            WHERE b.status = NVL(p_status, b.status)
            ORDER BY b.booking_date DESC;
        
        v_booking c_bookings%ROWTYPE;
        v_count NUMBER := 0;
    BEGIN
        DBMS_OUTPUT.PUT_LINE('===== BOOKING REPORT =====');
        DBMS_OUTPUT.PUT_LINE('Status Filter: ' || NVL(p_status, 'ALL'));
        DBMS_OUTPUT.PUT_LINE('--------------------------');
        
        -- Open cursor
        OPEN c_bookings;
        
        -- Fetch rows
        LOOP
            FETCH c_bookings INTO v_booking;
            EXIT WHEN c_bookings%NOTFOUND;
            
            v_count := v_count + 1;
            
            DBMS_OUTPUT.PUT_LINE('Booking ID: ' || v_booking.booking_id);
            DBMS_OUTPUT.PUT_LINE('Passenger: ' || v_booking.passenger_name);
            DBMS_OUTPUT.PUT_LINE('Flight: ' || v_booking.flight_number || 
                                ' (' || v_booking.origin || ' → ' || 
                                v_booking.destination || ')');
            DBMS_OUTPUT.PUT_LINE('Amount: $' || NVL(v_booking.amount, 0));
            DBMS_OUTPUT.PUT_LINE('Status: ' || v_booking.status);
            DBMS_OUTPUT.PUT_LINE('--------------------------');
        END LOOP;
        
        -- Close cursor
        CLOSE c_bookings;
        
        DBMS_OUTPUT.PUT_LINE('Total records: ' || v_count);
        
    EXCEPTION
        WHEN OTHERS THEN
            IF c_bookings%ISOPEN THEN
                CLOSE c_bookings;
            END IF;
            log_error(SQLCODE, SQLERRM, 'generate_booking_report');
    END generate_booking_report;

END flight_booking_pkg;
/

-- =====================================================
-- PART 4: WINDOW FUNCTIONS QUERIES
-- =====================================================

-- Window Function 1: Rank passengers by spending
CREATE OR REPLACE VIEW v_passenger_spending_rank AS
SELECT 
    p.passenger_id,
    p.first_name || ' ' || p.last_name AS passenger_name,
    SUM(pay.amount) AS total_spent,
    ROW_NUMBER() OVER (ORDER BY SUM(pay.amount) DESC) AS row_num,
    RANK() OVER (ORDER BY SUM(pay.amount) DESC) AS rank,
    DENSE_RANK() OVER (ORDER BY SUM(pay.amount) DESC) AS dense_rank
FROM passengers p
JOIN bookings b ON p.passenger_id = b.passenger_id
JOIN payments pay ON b.booking_id = pay.booking_id
GROUP BY p.passenger_id, p.first_name, p.last_name;

-- Window Function 2: Payment trends with LAG/LEAD
CREATE OR REPLACE VIEW v_payment_trends AS
SELECT 
    booking_id,
    amount,
    payment_method,
    LAG(amount, 1) OVER (ORDER BY booking_id) AS previous_payment,
    LEAD(amount, 1) OVER (ORDER BY booking_id) AS next_payment,
    amount - LAG(amount, 1) OVER (ORDER BY booking_id) AS payment_change
FROM payments;

-- Window Function 3: Bookings per flight with running total
CREATE OR REPLACE VIEW v_flight_booking_analysis AS
SELECT 
    f.flight_id,
    f.flight_number,
    f.origin || ' → ' || f.destination AS route,
    COUNT(b.booking_id) AS total_bookings,
    SUM(COUNT(b.booking_id)) OVER (ORDER BY f.flight_id) AS running_total,
    ROUND(
        RATIO_TO_REPORT(COUNT(b.booking_id)) OVER () * 100, 2
    ) AS percentage_of_total
FROM flights f
LEFT JOIN bookings b ON f.flight_id = b.flight_id
GROUP BY f.flight_id, f.flight_number, f.origin, f.destination;

-- Window Function 4: Revenue by payment method with partitioning
CREATE OR REPLACE VIEW v_payment_method_analytics AS
SELECT 
    payment_method,
    booking_id,
    amount,
    SUM(amount) OVER (PARTITION BY payment_method) AS method_total,
    AVG(amount) OVER (PARTITION BY payment_method) AS method_average,
    ROW_NUMBER() OVER (PARTITION BY payment_method ORDER BY amount DESC) AS rank_in_method
FROM payments;

-- =====================================================
-- PART 5: TESTING SCRIPT
-- =====================================================

-- Enable output
SET SERVEROUTPUT ON;

-- TEST 1: Create Booking
DECLARE
    v_booking_id NUMBER;
    v_status VARCHAR2(100);
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 1: Create Booking ===');
    flight_booking_pkg.create_booking(
        p_passenger_id => 586,
        p_flight_id => 1,
        p_booking_id => v_booking_id,
        p_status => v_status
    );
    DBMS_OUTPUT.PUT_LINE('Result: ' || v_status);
    DBMS_OUTPUT.PUT_LINE('');
END;
/

-- TEST 2: Update Booking Status
DECLARE
    v_success BOOLEAN;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 2: Update Booking Status ===');
    flight_booking_pkg.update_booking_status(
        p_booking_id => 1,
        p_new_status => 'Checked-in',
        p_success => v_success
    );
    IF v_success THEN
        DBMS_OUTPUT.PUT_LINE('Status update: SUCCESS');
    ELSE
        DBMS_OUTPUT.PUT_LINE('Status update: FAILED');
    END IF;
    DBMS_OUTPUT.PUT_LINE('');
END;
/

-- TEST 3: Calculate Revenue Function
DECLARE
    v_revenue NUMBER;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 3: Calculate Revenue ===');
    v_revenue := flight_booking_pkg.calculate_booking_revenue(1);
    DBMS_OUTPUT.PUT_LINE('Booking 1 Revenue: $' || v_revenue);
    DBMS_OUTPUT.PUT_LINE('');
END;
/

-- TEST 4: Validate Email Function
DECLARE
    v_valid BOOLEAN;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 4: Email Validation ===');
    v_valid := flight_booking_pkg.validate_passenger_email('test@example.com');
    IF v_valid THEN
        DBMS_OUTPUT.PUT_LINE('Email valid: TRUE');
    ELSE
        DBMS_OUTPUT.PUT_LINE('Email valid: FALSE');
    END IF;
    DBMS_OUTPUT.PUT_LINE('');
END;
/

-- TEST 5: Get Flight Capacity
DECLARE
    v_capacity NUMBER;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 5: Flight Capacity ===');
    v_capacity := flight_booking_pkg.get_flight_capacity(1);
    DBMS_OUTPUT.PUT_LINE('Available seats on Flight 1: ' || v_capacity);
    DBMS_OUTPUT.PUT_LINE('');
END;
/

-- TEST 6: Generate Report (Cursor)
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 6: Booking Report ===');
    flight_booking_pkg.generate_booking_report('Confirmed');
END;
/
-- PART 2: PACKAGE SPECIFICATION (Public Interface)
-- =====================================================

CREATE OR REPLACE PACKAGE flight_booking_pkg AS
    -- Custom Exceptions
    e_invalid_booking EXCEPTION;
    e_insufficient_seats EXCEPTION;
    e_invalid_payment EXCEPTION;
    e_passenger_not_found EXCEPTION;
    
    -- Procedure Declarations
    PROCEDURE create_booking(
        p_passenger_id IN NUMBER,
        p_flight_id IN NUMBER,
        p_booking_date IN DATE DEFAULT SYSDATE,
        p_booking_id OUT NUMBER,
        p_status OUT VARCHAR2
    );
    
    PROCEDURE update_booking_status(
        p_booking_id IN NUMBER,
        p_new_status IN VARCHAR2,
        p_success OUT BOOLEAN
    );
    
    PROCEDURE cancel_booking(
        p_booking_id IN NUMBER,
        p_refund_amount OUT NUMBER
    );
    
    PROCEDURE process_payment(
        p_booking_id IN NUMBER,
        p_amount IN NUMBER,
        p_payment_method IN VARCHAR2,
        p_payment_id OUT NUMBER
    );
    
    PROCEDURE bulk_update_status(
        p_old_status IN VARCHAR2,
        p_new_status IN VARCHAR2,
        p_updated_count OUT NUMBER
    );
    
    -- Function Declarations
    FUNCTION calculate_booking_revenue(
        p_booking_id IN NUMBER
    ) RETURN NUMBER;
    
    FUNCTION validate_passenger_email(
        p_email IN VARCHAR2
    ) RETURN BOOLEAN;
    
    FUNCTION get_flight_capacity(
        p_flight_id IN NUMBER
    ) RETURN NUMBER;
    
    FUNCTION calculate_total_revenue(
        p_start_date IN DATE DEFAULT NULL,
        p_end_date IN DATE DEFAULT NULL
    ) RETURN NUMBER;
    
    FUNCTION get_passenger_name(
        p_passenger_id IN NUMBER
    ) RETURN VARCHAR2;
    
    -- Cursor-based Procedure
    PROCEDURE generate_booking_report(
        p_status IN VARCHAR2 DEFAULT NULL
    );
    
END flight_booking_pkg;
/

-- =====================================================
-- PART 3: PACKAGE BODY (Implementation)
-- =====================================================

CREATE OR REPLACE PACKAGE BODY flight_booking_pkg AS

    -- Private procedure for error logging
    PROCEDURE log_error(
        p_error_code IN NUMBER,
        p_error_msg IN VARCHAR2,
        p_proc_name IN VARCHAR2
    ) IS
        PRAGMA AUTONOMOUS_TRANSACTION;
    BEGIN
        INSERT INTO error_log (error_code, error_message, procedure_name)
        VALUES (p_error_code, p_error_msg, p_proc_name);
        COMMIT;
    END log_error;

    -- =====================================================
    -- PROCEDURE 1: Create Booking
    -- Purpose: Insert new booking with validation
    -- Parameters: IN (passenger_id, flight_id), OUT (booking_id, status)
    -- =====================================================
    PROCEDURE create_booking(
        p_passenger_id IN NUMBER,
        p_flight_id IN NUMBER,
        p_booking_date IN DATE DEFAULT SYSDATE,
        p_booking_id OUT NUMBER,
        p_status OUT VARCHAR2
    ) IS
        v_passenger_exists NUMBER;
        v_flight_exists NUMBER;
        v_next_booking_id NUMBER;
    BEGIN
        -- Validate passenger exists
        SELECT COUNT(*) INTO v_passenger_exists
        FROM passengers
        WHERE passenger_id = p_passenger_id;
        
        IF v_passenger_exists = 0 THEN
            RAISE e_passenger_not_found;
        END IF;
        
        -- Validate flight exists
        SELECT COUNT(*) INTO v_flight_exists
        FROM flights
        WHERE flight_id = p_flight_id;
        
        IF v_flight_exists = 0 THEN
            RAISE_APPLICATION_ERROR(-20001, 'Flight does not exist');
        END IF;
        
        -- Get next booking ID
        SELECT NVL(MAX(booking_id), 0) + 1 INTO v_next_booking_id
        FROM bookings;
        
        -- Insert booking
        INSERT INTO bookings (booking_id, passenger_id, flight_id, booking_date, status)
        VALUES (v_next_booking_id, p_passenger_id, p_flight_id, p_booking_date, 'Confirmed');
        
        p_booking_id := v_next_booking_id;
        p_status := 'SUCCESS';
        
        COMMIT;
        
        DBMS_OUTPUT.PUT_LINE('Booking created successfully. Booking ID: ' || p_booking_id);
        
    EXCEPTION
        WHEN e_passenger_not_found THEN
            p_status := 'FAILED - Passenger not found';
            log_error(SQLCODE, 'Passenger ID ' || p_passenger_id || ' not found', 'create_booking');
            ROLLBACK;
        WHEN OTHERS THEN
            p_status := 'FAILED - ' || SQLERRM;
            log_error(SQLCODE, SQLERRM, 'create_booking');
            ROLLBACK;
    END create_booking;

    -- =====================================================
    -- PROCEDURE 2: Update Booking Status
    -- Purpose: Change booking status with validation
    -- Parameters: IN (booking_id, new_status), OUT (success)
    -- =====================================================
    PROCEDURE update_booking_status(
        p_booking_id IN NUMBER,
        p_new_status IN VARCHAR2,
        p_success OUT BOOLEAN
    ) IS
        v_current_status VARCHAR2(20);
        v_booking_exists NUMBER;
    BEGIN
        -- Check if booking exists
        SELECT COUNT(*) INTO v_booking_exists
        FROM bookings
        WHERE booking_id = p_booking_id;
        
        IF v_booking_exists = 0 THEN
            RAISE e_invalid_booking;
        END IF;
        
        -- Get current status
        SELECT status INTO v_current_status
        FROM bookings
        WHERE booking_id = p_booking_id;
        
        -- Update status
        UPDATE bookings
        SET status = p_new_status
        WHERE booking_id = p_booking_id;
        
        p_success := TRUE;
        COMMIT;
        
        DBMS_OUTPUT.PUT_LINE('Booking ' || p_booking_id || ' status updated from ' || 
                            v_current_status || ' to ' || p_new_status);
        
    EXCEPTION
        WHEN e_invalid_booking THEN
            p_success := FALSE;
            log_error(-20002, 'Booking ID ' || p_booking_id || ' does not exist', 'update_booking_status');
            ROLLBACK;
        WHEN OTHERS THEN
            p_success := FALSE;
            log_error(SQLCODE, SQLERRM, 'update_booking_status');
            ROLLBACK;
    END update_booking_status;

    -- =====================================================
    -- PROCEDURE 3: Cancel Booking
    -- Purpose: Cancel booking and calculate refund
    -- Parameters: IN (booking_id), OUT (refund_amount)
    -- =====================================================
    PROCEDURE cancel_booking(
        p_booking_id IN NUMBER,
        p_refund_amount OUT NUMBER
    ) IS
        v_payment_amount NUMBER;
    BEGIN
        -- Get payment amount
        SELECT amount INTO v_payment_amount
        FROM payments
        WHERE booking_id = p_booking_id;
        
        -- Calculate refund (90% refund policy)
        p_refund_amount := v_payment_amount * 0.90;
        
        -- Update booking status
        UPDATE bookings
        SET status = 'Cancelled'
        WHERE booking_id = p_booking_id;
        
        COMMIT;
        
        DBMS_OUTPUT.PUT_LINE('Booking cancelled. Refund amount: $' || 
                            ROUND(p_refund_amount, 2));
        
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            p_refund_amount := 0;
            log_error(-20003, 'No payment found for booking ' || p_booking_id, 'cancel_booking');
            ROLLBACK;
        WHEN OTHERS THEN
            p_refund_amount := 0;
            log_error(SQLCODE, SQLERRM, 'cancel_booking');
            ROLLBACK;
    END cancel_booking;

    -- =====================================================
    -- PROCEDURE 4: Process Payment
    -- Purpose: Record payment for booking
    -- Parameters: IN (booking_id, amount, method), OUT (payment_id)
    -- =====================================================
    PROCEDURE process_payment(
        p_booking_id IN NUMBER,
        p_amount IN NUMBER,
        p_payment_method IN VARCHAR2,
        p_payment_id OUT NUMBER
    ) IS
        v_booking_exists NUMBER;
    BEGIN
        -- Validate amount
        IF p_amount <= 0 THEN
            RAISE e_invalid_payment;
        END IF;
        
        -- Check booking exists
        SELECT COUNT(*) INTO v_booking_exists
        FROM bookings
        WHERE booking_id = p_booking_id;
        
        IF v_booking_exists = 0 THEN
            RAISE e_invalid_booking;
        END IF;
        
        -- Insert payment (assuming PAYMENT_ID is auto-generated or managed)
        INSERT INTO payments (booking_id, amount, payment_method)
        VALUES (p_booking_id, p_amount, p_payment_method);
        
        p_payment_id := p_booking_id; -- Simplified for this example
        
        COMMIT;
        
        DBMS_OUTPUT.PUT_LINE('Payment processed: $' || p_amount || 
                            ' via ' || p_payment_method);
        
    EXCEPTION
        WHEN e_invalid_payment THEN
            p_payment_id := NULL;
            log_error(-20004, 'Invalid payment amount: ' || p_amount, 'process_payment');
            ROLLBACK;
        WHEN OTHERS THEN
            p_payment_id := NULL;
            log_error(SQLCODE, SQLERRM, 'process_payment');
            ROLLBACK;
    END process_payment;

    -- =====================================================
    -- PROCEDURE 5: Bulk Update Status (Bulk Operations)
    -- Purpose: Update multiple bookings at once
    -- Parameters: IN (old_status, new_status), OUT (updated_count)
    -- =====================================================
    PROCEDURE bulk_update_status(
        p_old_status IN VARCHAR2,
        p_new_status IN VARCHAR2,
        p_updated_count OUT NUMBER
    ) IS
        TYPE booking_id_tab IS TABLE OF bookings.booking_id%TYPE;
        v_booking_ids booking_id_tab;
    BEGIN
        -- Bulk collect booking IDs
        SELECT booking_id
        BULK COLLECT INTO v_booking_ids
        FROM bookings
        WHERE status = p_old_status;
        
        -- Bulk update
        FORALL i IN 1..v_booking_ids.COUNT
            UPDATE bookings
            SET status = p_new_status
            WHERE booking_id = v_booking_ids(i);
        
        p_updated_count := SQL%ROWCOUNT;
        
        COMMIT;
        
        DBMS_OUTPUT.PUT_LINE('Updated ' || p_updated_count || 
                            ' bookings from ' || p_old_status || 
                            ' to ' || p_new_status);
        
    EXCEPTION
        WHEN OTHERS THEN
            p_updated_count := 0;
            log_error(SQLCODE, SQLERRM, 'bulk_update_status');
            ROLLBACK;
    END bulk_update_status;

    -- =====================================================
    -- FUNCTION 1: Calculate Booking Revenue
    -- Purpose: Get total payment for a booking
    -- Returns: Payment amount
    -- =====================================================
    FUNCTION calculate_booking_revenue(
        p_booking_id IN NUMBER
    ) RETURN NUMBER IS
        v_revenue NUMBER := 0;
    BEGIN
        SELECT NVL(SUM(amount), 0)
        INTO v_revenue
        FROM payments
        WHERE booking_id = p_booking_id;
        
        RETURN v_revenue;
        
    EXCEPTION
        WHEN OTHERS THEN
            log_error(SQLCODE, SQLERRM, 'calculate_booking_revenue');
            RETURN 0;
    END calculate_booking_revenue;

    -- =====================================================
    -- FUNCTION 2: Validate Passenger Email
    -- Purpose: Check if email format is valid
    -- Returns: TRUE/FALSE
    -- =====================================================
    FUNCTION validate_passenger_email(
        p_email IN VARCHAR2
    ) RETURN BOOLEAN IS
    BEGIN
        -- Basic email validation (contains @ and .)
        IF p_email LIKE '%_@__%.__%' THEN
            RETURN TRUE;
        ELSE
            RETURN FALSE;
        END IF;
        
    EXCEPTION
        WHEN OTHERS THEN
            log_error(SQLCODE, SQLERRM, 'validate_passenger_email');
            RETURN FALSE;
    END validate_passenger_email;

    -- =====================================================
    -- FUNCTION 3: Get Flight Capacity
    -- Purpose: Calculate available seats on flight
    -- Returns: Number of available seats
    -- =====================================================
    FUNCTION get_flight_capacity(
        p_flight_id IN NUMBER
    ) RETURN NUMBER IS
        v_booked_seats NUMBER := 0;
        v_total_capacity NUMBER := 200; -- Assume 200 seats per flight
    BEGIN
        SELECT COUNT(*)
        INTO v_booked_seats
        FROM bookings
        WHERE flight_id = p_flight_id
        AND status IN ('Confirmed', 'Checked-in');
        
        RETURN v_total_capacity - v_booked_seats;
        
    EXCEPTION
        WHEN OTHERS THEN
            log_error(SQLCODE, SQLERRM, 'get_flight_capacity');
            RETURN 0;
    END get_flight_capacity;

    -- =====================================================
    -- FUNCTION 4: Calculate Total Revenue
    -- Purpose: Calculate revenue for date range
    -- Returns: Total revenue
    -- =====================================================
    FUNCTION calculate_total_revenue(
        p_start_date IN DATE DEFAULT NULL,
        p_end_date IN DATE DEFAULT NULL
    ) RETURN NUMBER IS
        v_total_revenue NUMBER := 0;
    BEGIN
        IF p_start_date IS NULL AND p_end_date IS NULL THEN
            SELECT NVL(SUM(amount), 0)
            INTO v_total_revenue
            FROM payments;
        ELSE
            SELECT NVL(SUM(p.amount), 0)
            INTO v_total_revenue
            FROM payments p
            JOIN bookings b ON p.booking_id = b.booking_id
            WHERE b.booking_date BETWEEN 
                  NVL(p_start_date, b.booking_date) AND 
                  NVL(p_end_date, b.booking_date);
        END IF;
        
        RETURN v_total_revenue;
        
    EXCEPTION
        WHEN OTHERS THEN
            log_error(SQLCODE, SQLERRM, 'calculate_total_revenue');
            RETURN 0;
    END calculate_total_revenue;

    -- =====================================================
    -- FUNCTION 5: Get Passenger Name (Lookup)
    -- Purpose: Retrieve passenger full name
    -- Returns: Full name
    -- =====================================================
    FUNCTION get_passenger_name(
        p_passenger_id IN NUMBER
    ) RETURN VARCHAR2 IS
        v_full_name VARCHAR2(200);
    BEGIN
        SELECT first_name || ' ' || last_name
        INTO v_full_name
        FROM passengers
        WHERE passenger_id = p_passenger_id;
        
        RETURN v_full_name;
        
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            RETURN 'Unknown Passenger';
        WHEN OTHERS THEN
            log_error(SQLCODE, SQLERRM, 'get_passenger_name');
            RETURN 'Error';
    END get_passenger_name;

    -- =====================================================
    -- PROCEDURE 6: Generate Booking Report (Cursor)
    -- Purpose: Display booking details using explicit cursor
    -- =====================================================
    PROCEDURE generate_booking_report(
        p_status IN VARCHAR2 DEFAULT NULL
    ) IS
        -- Explicit cursor
        CURSOR c_bookings IS
            SELECT b.booking_id, b.booking_date, b.status,
                   p.first_name || ' ' || p.last_name AS passenger_name,
                   f.flight_number, f.origin, f.destination,
                   pay.amount
            FROM bookings b
            JOIN passengers p ON b.passenger_id = p.passenger_id
            JOIN flights f ON b.flight_id = f.flight_id
            LEFT JOIN payments pay ON b.booking_id = pay.booking_id
            WHERE b.status = NVL(p_status, b.status)
            ORDER BY b.booking_date DESC;
        
        v_booking c_bookings%ROWTYPE;
        v_count NUMBER := 0;
    BEGIN
        DBMS_OUTPUT.PUT_LINE('===== BOOKING REPORT =====');
        DBMS_OUTPUT.PUT_LINE('Status Filter: ' || NVL(p_status, 'ALL'));
        DBMS_OUTPUT.PUT_LINE('--------------------------');
        
        -- Open cursor
        OPEN c_bookings;
        
        -- Fetch rows
        LOOP
            FETCH c_bookings INTO v_booking;
            EXIT WHEN c_bookings%NOTFOUND;
            
            v_count := v_count + 1;
            
            DBMS_OUTPUT.PUT_LINE('Booking ID: ' || v_booking.booking_id);
            DBMS_OUTPUT.PUT_LINE('Passenger: ' || v_booking.passenger_name);
            DBMS_OUTPUT.PUT_LINE('Flight: ' || v_booking.flight_number || 
                                ' (' || v_booking.origin || ' → ' || 
                                v_booking.destination || ')');
            DBMS_OUTPUT.PUT_LINE('Amount: $' || NVL(v_booking.amount, 0));
            DBMS_OUTPUT.PUT_LINE('Status: ' || v_booking.status);
            DBMS_OUTPUT.PUT_LINE('--------------------------');
        END LOOP;
        
        -- Close cursor
        CLOSE c_bookings;
        
        DBMS_OUTPUT.PUT_LINE('Total records: ' || v_count);
        
    EXCEPTION
        WHEN OTHERS THEN
            IF c_bookings%ISOPEN THEN
                CLOSE c_bookings;
            END IF;
            log_error(SQLCODE, SQLERRM, 'generate_booking_report');
    END generate_booking_report;

END flight_booking_pkg;
/

-- =====================================================
-- PART 4: WINDOW FUNCTIONS QUERIES
-- =====================================================

-- Window Function 1: Rank passengers by spending
CREATE OR REPLACE VIEW v_passenger_spending_rank AS
SELECT 
    p.passenger_id,
    p.first_name || ' ' || p.last_name AS passenger_name,
    SUM(pay.amount) AS total_spent,
    ROW_NUMBER() OVER (ORDER BY SUM(pay.amount) DESC) AS row_num,
    RANK() OVER (ORDER BY SUM(pay.amount) DESC) AS rank,
    DENSE_RANK() OVER (ORDER BY SUM(pay.amount) DESC) AS dense_rank
FROM passengers p
JOIN bookings b ON p.passenger_id = b.passenger_id
JOIN payments pay ON b.booking_id = pay.booking_id
GROUP BY p.passenger_id, p.first_name, p.last_name;

-- Window Function 2: Payment trends with LAG/LEAD
CREATE OR REPLACE VIEW v_payment_trends AS
SELECT 
    booking_id,
    amount,
    payment_method,
    LAG(amount, 1) OVER (ORDER BY booking_id) AS previous_payment,
    LEAD(amount, 1) OVER (ORDER BY booking_id) AS next_payment,
    amount - LAG(amount, 1) OVER (ORDER BY booking_id) AS payment_change
FROM payments;

-- Window Function 3: Bookings per flight with running total
CREATE OR REPLACE VIEW v_flight_booking_analysis AS
SELECT 
    f.flight_id,
    f.flight_number,
    f.origin || ' → ' || f.destination AS route,
    COUNT(b.booking_id) AS total_bookings,
    SUM(COUNT(b.booking_id)) OVER (ORDER BY f.flight_id) AS running_total,
    ROUND(
        RATIO_TO_REPORT(COUNT(b.booking_id)) OVER () * 100, 2
    ) AS percentage_of_total
FROM flights f
LEFT JOIN bookings b ON f.flight_id = b.flight_id
GROUP BY f.flight_id, f.flight_number, f.origin, f.destination;

-- Window Function 4: Revenue by payment method with partitioning
CREATE OR REPLACE VIEW v_payment_method_analytics AS
SELECT 
    payment_method,
    booking_id,
    amount,
    SUM(amount) OVER (PARTITION BY payment_method) AS method_total,
    AVG(amount) OVER (PARTITION BY payment_method) AS method_average,
    ROW_NUMBER() OVER (PARTITION BY payment_method ORDER BY amount DESC) AS rank_in_method
FROM payments;

-- =====================================================
-- PART 5: TESTING SCRIPT
-- =====================================================

-- Enable output
SET SERVEROUTPUT ON;

-- TEST 1: Create Booking
DECLARE
    v_booking_id NUMBER;
    v_status VARCHAR2(100);
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 1: Create Booking ===');
    flight_booking_pkg.create_booking(
        p_passenger_id => 586,
        p_flight_id => 1,
        p_booking_id => v_booking_id,
        p_status => v_status
    );
    DBMS_OUTPUT.PUT_LINE('Result: ' || v_status);
    DBMS_OUTPUT.PUT_LINE('');
END;
/

-- TEST 2: Update Booking Status
DECLARE
    v_success BOOLEAN;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 2: Update Booking Status ===');
    flight_booking_pkg.update_booking_status(
        p_booking_id => 1,
        p_new_status => 'Checked-in',
        p_success => v_success
    );
    IF v_success THEN
        DBMS_OUTPUT.PUT_LINE('Status update: SUCCESS');
    ELSE
        DBMS_OUTPUT.PUT_LINE('Status update: FAILED');
    END IF;
    DBMS_OUTPUT.PUT_LINE('');
END;
/

-- TEST 3: Calculate Revenue Function
DECLARE
    v_revenue NUMBER;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 3: Calculate Revenue ===');
    v_revenue := flight_booking_pkg.calculate_booking_revenue(1);
    DBMS_OUTPUT.PUT_LINE('Booking 1 Revenue: $' || v_revenue);
    DBMS_OUTPUT.PUT_LINE('');
END;
/

-- TEST 4: Validate Email Function
DECLARE
    v_valid BOOLEAN;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 4: Email Validation ===');
    v_valid := flight_booking_pkg.validate_passenger_email('test@example.com');
    IF v_valid THEN
        DBMS_OUTPUT.PUT_LINE('Email valid: TRUE');
    ELSE
        DBMS_OUTPUT.PUT_LINE('Email valid: FALSE');
    END IF;
    DBMS_OUTPUT.PUT_LINE('');
END;
/

-- TEST 5: Get Flight Capacity
DECLARE
    v_capacity NUMBER;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 5: Flight Capacity ===');
    v_capacity := flight_booking_pkg.get_flight_capacity(1);
    DBMS_OUTPUT.PUT_LINE('Available seats on Flight 1: ' || v_capacity);
    DBMS_OUTPUT.PUT_LINE('');
END;
/

-- TEST 6: Generate Report (Cursor)
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 6: Booking Report ===');
    flight_booking_pkg.generate_booking_report('Confirmed');
END;
/

-- TEST 7: Calculate Total Revenue
DECLARE
    v_total NUMBER;
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 7: Total Revenue ===');
    v_total := flight_booking_pkg.calculate_total_revenue();
    DBMS_OUTPUT.PUT_LINE('Total Revenue: $' || ROUND(v_total, 2));
    DBMS_OUTPUT.PUT_LINE('');
END;
/

-- TEST 8: Get Passenger Name
DECLARE
    v_name VARCHAR2(200);
BEGIN
    DBMS_OUTPUT.PUT_LINE('=== TEST 8: Get Passenger Name ===');
    v_name := flight_booking_pkg.get_passenger_name(586);
    DBMS_OUTPUT.PUT_LINE('Passenger Name: ' || v_name);
    DBMS_OUTPUT.PUT_LINE('');
END;
/NN
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




**Lecturer:** Eric Maniraguha  
**Academic Year:** 2025-2026, Semester I  
**Completion Date:** December 7, 2025

---


]
