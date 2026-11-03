## Requirements for Completion

Based on the given assumptions, apply **system analysis** to design a system that supports the operation of a **Disc Rental Service**.

This task will be completed in **three stages**. In each stage you must answer the given questions and perform the specified tasks.

---

# Assumptions

- The main goods in the rental service are **discs**.
- Customers **rent discs**.
- Customers can **rent multiple discs**.
- Discs are of **different types** (music, software, movies, etc.).
- Each customer can rent **discs of any type**.
- Discs are stored in a **disc registry**.
- Each customer has their own **rental card**.
- New disc copies are ordered from **one of many suppliers**.
- Each supplier provides **many types of discs**.
- The owner rents discs and records **who rented what** in a **central rental register**.
- The owner also records this information on the **customer’s rental card**.

At the **end of the calendar year**, the owner prepares summary reports including:

- A **list of rental service customers**
- A **list of discs available in the rental service**
- A **list of rentals** (who rented how many and which discs)
- A **list of damaged discs**
- A **list of debtors** (discs returned late or not returned at all)
- **Calculation of the average rental time** for each disc type during the year
- **Orders for new discs**

---
# Task 1 — System Analysis: Disc Rental Service

# Questions

1. **Which analysis method** can be applied to analyze this problem (a system supporting the operation of the Disc Rental Service)?

2. Based on the **initial assumptions**, can you **define the requirements** for the Disc Rental Service system?

---

## 1. System Functions

List the **tasks (functions)** that your system will perform.

Examples may include:

- Customer registration
- Disc registration
- Rental processing
- Return processing
- Supplier management
- Reporting
- Inventory tracking

---

## 2. Identification of Real-World Objects

Select **objects from the real world** that should be represented in the designed information system.

Examples:

- Customer
- Disc
- Disc Type
- Rental
- Supplier
- Order
- Rental Card
- Damage Report

Briefly **justify why each object should be included in the system**.

---

## 3. System Goal and Functional Scope

Define:

- The **main goal** of the designed system
- A **list of functions** the system should fulfill to support the operation of the Disc Rental Service

---

## 4. Data Dictionary

Create a **data dictionary** containing:

- All system **objects**
- Their **attributes**
- A **description of each attribute**
- Relationships between objects (if applicable)

Example structure:

| Object | Attribute | Description |
|------|------|------|
| Customer | CustomerID | Unique identifier of the customer |
| Customer | Name | Full name of the customer |
| Disc | DiscID | Unique disc identifier |
| Disc | DiscType | Type of disc (music, movie, software) |
| Rental | RentalDate | Date when the disc was rented |
| Rental | ReturnDate | Date when the disc was returned |

Provide a **detailed description of both objects and system functions**.

---

# Expected Outcome

Students should produce:

1. Selected **analysis method**
2. Defined **system requirements**
3. List of **system functions**
4. Identified **real-world objects with justification**
5. Defined **system goal and functional scope**
6. A complete **data dictionary**