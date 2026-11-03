# Disc Rental Service System — Stage I: System Analysis

---

## Question 1. Which analysis method can be applied?

Two classic methods can be used to analyze this problem:

| Method | Main idea | Main tools |
|---|---|---|
| **Structured analysis** | The system is seen as a set of processes that transform data | Data Flow Diagrams (DFD), Entity–Relationship Diagrams (ERD), data dictionary, process specifications |
| **Object-oriented analysis (OOA)** | The system is seen as a set of cooperating objects that hold data and behavior | UML: use case, class, activity and sequence diagrams |

**Selected method: Object-Oriented Analysis (OOA), supported by UML.**

**Justification:**

1. The problem description is built around clearly identifiable real-world things — *customer, disc, disc type, rental card, supplier, order* — which map directly to objects (classes).
2. Each object has its own data (e.g. a disc has a title and status) and its own behavior (a disc can be rented, returned, marked as damaged), which OOA keeps together.
3. Relationships stated in the assumptions (a customer rents many discs, a supplier provides many disc types) are naturally expressed as associations with multiplicities.
4. The system is easy to extend later (e.g. online reservations, new disc types) by adding new classes without rebuilding the whole model.
5. UML is a widely used standard and leads directly to implementation in modern object-oriented languages and relational databases.

Structured analysis would also work (the yearly reports are a clear data-processing task), but OOA better reflects the entity-centred nature of the business.

---

## Question 2. Requirements of the Disc Rental Service system

### 2.1 Functional requirements

The system must:

| ID | Requirement | Source assumption |
|---|---|---|
| FR1 | Register customers and maintain their personal data | Customers rent discs |
| FR2 | Issue and maintain a **rental card** for every customer | Each customer has their own rental card |
| FR3 | Maintain a **disc registry** (catalog of all disc copies) | Discs are stored in a disc registry |
| FR4 | Classify discs by **type** (music, movie, software, game, …) | Discs are of different types |
| FR5 | Record rentals of **one or many discs of any type** by a customer | Customers can rent multiple discs of any type |
| FR6 | Record each rental in the **central rental register** and on the customer's rental card | Owner records who rented what |
| FR7 | Record returns, including the return date and disc condition | Needed for debtors / average time |
| FR8 | Record **damaged discs** (damage report) | List of damaged discs |
| FR9 | Detect **overdue** and **not returned** discs and calculate penalties | List of debtors |
| FR10 | Maintain a list of **suppliers** and the disc types each one provides | Many suppliers, each provides many types |
| FR11 | Create and track **orders for new disc copies** from a chosen supplier | New copies are ordered from one of many suppliers |
| FR12 | Add delivered discs to the registry | Supplier delivers ordered discs |
| FR13 | Generate the **year-end reports** (see FR13a–g) | End of calendar year reports |
| FR13a | – list of customers | |
| FR13b | – list of discs available in the rental service | |
| FR13c | – list of rentals (who rented how many and which discs) | |
| FR13d | – list of damaged discs | |
| FR13e | – list of debtors (late or not returned) | |
| FR13f | – average rental time for each disc type during the year | |
| FR13g | – list of orders for new discs | |
| FR14 | Search and filter customers, discs and rentals | Daily operation |

### 2.2 Non-functional requirements

| ID | Requirement |
|---|---|
| NFR1 | Data is stored permanently in a database and backed up regularly |
| NFR2 | Access is restricted to authorised users (owner / staff) with login and password |
| NFR3 | Customer personal data is protected according to data-protection law (e.g. GDPR) |
| NFR4 | Searching a disc or customer and recording a rental takes only a few seconds |
| NFR5 | The interface is simple enough for the owner to use without technical training |
| NFR6 | Reports can be printed and exported (PDF / spreadsheet) |
| NFR7 | Data integrity: a disc cannot be rented twice at the same time; a deleted customer with open rentals is not allowed |
| NFR8 | The system can be extended with new disc types and functions |

---

## 1. System Functions

### F1. Customer management
- Register a new customer and issue a rental card
- Update customer data (address, phone, e-mail)
- Deactivate a customer (only when no discs are outstanding)
- Search customers by name, card number or phone

### F2. Disc registry (inventory) management
- Register a new disc copy (title, type, supplier, price, location)
- Update disc information
- Change disc status: *available / rented / damaged / withdrawn*
- Check disc availability
- Search discs by title, type or status

### F3. Disc type management
- Add, edit and remove disc types (music, movie, software, game …)

### F4. Rental processing
- Identify the customer by rental card
- Select one or more available discs
- Set the rental date and due date, calculate the fee
- Save the rental in the central rental register and on the rental card
- Change the status of the discs to *rented*

### F5. Return processing
- Record the return date of each disc
- Check the disc condition; if damaged, create a damage report
- Calculate the penalty for late return or damage
- Change the disc status back to *available* (or *damaged*)

### F6. Overdue and debtor monitoring
- Find rentals whose due date has passed and which are not returned
- Find rentals returned after the due date
- Calculate the customer's outstanding debt

### F7. Damage handling
- Register a damage report (disc, date, description, customer responsible)
- Withdraw damaged discs from rental and mark them for replacement

### F8. Supplier management
- Add, edit and remove suppliers
- Record which disc types each supplier offers

### F9. Order management
- Create an order for new disc copies from one supplier
- Track the order status (*created / sent / delivered / cancelled*)
- Receive a delivery and add the new copies to the disc registry

### F10. Reporting and statistics
- Customer list
- List of available discs
- Rental list (per customer: number and titles of rented discs)
- Damaged disc list
- Debtor list
- Average rental time per disc type in a given year
- List of orders for new discs
- Reports for any chosen period (month, year, custom)

### F11. System administration
- User accounts and passwords
- Data backup and restore

---

## 2. Identification of Real-World Objects

| Object | What it represents | Why it must be in the system |
|---|---|---|
| **Customer** | A person who rents discs | Rentals, debts and the customer list are all based on customers; the owner must know who rented what. |
| **Rental Card** | The customer's personal card listing their rentals | Required by the assumptions — the owner writes every rental on it; it also identifies the customer at the counter. |
| **Disc** | A single physical copy in the disc registry | It is the main good of the business; its status (available, rented, damaged) and history must be known. |
| **Disc Type** | Category of a disc (music, movie, software …) | Discs differ by type; the yearly average rental time must be calculated *per type*, and suppliers offer specific types. |
| **Rental** | One rental transaction of a customer (the entry in the central rental register) | Records *who* rented, *when*, and the due date; a customer can rent several discs at once. |
| **Rental Item** | One disc within a rental, with its own return date | Needed because one rental can include many discs which may be returned on different days; the basis for debtor and average-time reports. |
| **Damage Report** | Record of a disc found damaged | Required for the list of damaged discs and for charging the responsible customer. |
| **Supplier** | A company that sells new disc copies | New copies are ordered from one of many suppliers; contact data is needed to place orders. |
| **Supplier Offer** | The disc types a given supplier provides | Each supplier provides many types and each type can come from many suppliers (many-to-many); the owner chooses a supplier by type. |
| **Order** | An order for new disc copies sent to one supplier | Required for the year-end list of orders and for tracking deliveries. |
| **Order Item** | One ordered title/type and quantity within an order | One order can contain several titles or types of discs. |
| **Owner (User)** | The person operating the system | Performs all operations; needed for login and to know who recorded a transaction. |

Objects such as the *central rental register* and the *disc registry* are not separate classes — they are the complete collections of **Rental** and **Disc** records kept by the system.

---

## 3. System Goal and Functional Scope

### 3.1 Main goal

The goal of the system is to **support the owner of the Disc Rental Service in the day-to-day registration of customers, discs, rentals, returns, damages, suppliers and orders, and to automatically produce the year-end reports**, replacing paper registers and rental cards with one consistent, reliable database.

Specific objectives:
- faster and error-free rental and return processing,
- up-to-date knowledge of which discs are available, rented or damaged,
- quick identification of debtors,
- informed purchasing decisions (which disc types are popular, what to order),
- automatic preparation of yearly statistics and lists.

### 3.2 Functional scope

**Included in the system:**

| Area | Functions |
|---|---|
| Customers | registration, rental cards, data updates, search |
| Discs | disc registry, disc types, status and availability |
| Rentals | rentals of one or many discs, returns, fees, penalties |
| Control | overdue rentals, debtors, damage reports |
| Purchasing | suppliers, supplier offers, orders, deliveries |
| Reporting | the seven year-end reports and period reports |
| Administration | users, security, backup |

**Outside the scope (possible future extensions):**
- online shop / online reservations,
- card payments and accounting / invoicing,
- automatic e-mail or SMS reminders,
- barcode or RFID scanning hardware integration.

---

## 4. Data Dictionary

### 4.1 Objects and attributes

**Customer**

| Object | Attribute | Type | Description |
|---|---|---|---|
| Customer | CustomerID (PK) | Integer | Unique identifier of the customer |
| Customer | FirstName | Text(50) | Customer's first name |
| Customer | LastName | Text(50) | Customer's last name |
| Customer | Address | Text(150) | Home address |
| Customer | Phone | Text(20) | Contact phone number |
| Customer | Email | Text(100) | E-mail address |
| Customer | IDDocumentNo | Text(20) | ID card number used to confirm identity |
| Customer | RegistrationDate | Date | Date the customer joined the rental service |
| Customer | IsActive | Boolean | Whether the customer may still rent discs |

**Rental Card**

| Object | Attribute | Type | Description |
|---|---|---|---|
| RentalCard | CardNo (PK) | Text(12) | Unique number printed on the card |
| RentalCard | CustomerID (FK) | Integer | Owner of the card |
| RentalCard | IssueDate | Date | Date the card was issued |
| RentalCard | ExpiryDate | Date | Date until which the card is valid |
| RentalCard | Status | Enum | Active / blocked / expired |

**Disc Type**

| Object | Attribute | Type | Description |
|---|---|---|---|
| DiscType | TypeID (PK) | Integer | Unique identifier of the disc type |
| DiscType | TypeName | Text(30) | Name of the type: music, movie, software, game … |
| DiscType | Description | Text(200) | Description of the type |
| DiscType | DailyRate | Decimal | Standard rental price per day for this type |
| DiscType | MaxRentalDays | Integer | Number of days after which a rental becomes overdue |

**Disc**

| Object | Attribute | Type | Description |
|---|---|---|---|
| Disc | DiscID (PK) | Integer | Unique identifier of the disc copy (inventory number) |
| Disc | Title | Text(100) | Title of the disc |
| Disc | TypeID (FK) | Integer | Type of the disc |
| Disc | SupplierID (FK) | Integer | Supplier from which the copy was bought |
| Disc | Author | Text(100) | Artist, director or producer |
| Disc | ReleaseYear | Integer | Year of publication |
| Disc | PurchaseDate | Date | Date the copy was added to the registry |
| Disc | PurchasePrice | Decimal | Price paid to the supplier |
| Disc | Location | Text(20) | Shelf / place in the shop |
| Disc | Status | Enum | Available / rented / damaged / withdrawn |

**Rental** (entry in the central rental register)

| Object | Attribute | Type | Description |
|---|---|---|---|
| Rental | RentalID (PK) | Integer | Unique identifier of the rental |
| Rental | CardNo (FK) | Text(12) | Rental card of the customer who rents |
| Rental | RentalDate | Date | Date the discs were rented |
| Rental | DueDate | Date | Date by which the discs must be returned |
| Rental | TotalFee | Decimal | Total rental fee for all discs in the rental |
| Rental | Status | Enum | Open / closed / overdue |
| Rental | UserID (FK) | Integer | Person who recorded the rental |

**Rental Item**

| Object | Attribute | Type | Description |
|---|---|---|---|
| RentalItem | RentalID (PK, FK) | Integer | Rental this item belongs to |
| RentalItem | DiscID (PK, FK) | Integer | Rented disc copy |
| RentalItem | ReturnDate | Date | Actual return date (empty = not returned) |
| RentalItem | Fee | Decimal | Fee for this disc |
| RentalItem | LateDays | Integer | Days returned after the due date (derived) |
| RentalItem | Penalty | Decimal | Penalty for late return or damage |
| RentalItem | ConditionOnReturn | Enum | Good / damaged |

**Damage Report**

| Object | Attribute | Type | Description |
|---|---|---|---|
| DamageReport | DamageID (PK) | Integer | Unique identifier of the report |
| DamageReport | DiscID (FK) | Integer | Damaged disc copy |
| DamageReport | RentalID (FK) | Integer | Rental during which damage occurred (optional) |
| DamageReport | ReportDate | Date | Date the damage was found |
| DamageReport | Description | Text(200) | Description of the damage |
| DamageReport | Charge | Decimal | Amount charged to the customer |
| DamageReport | Action | Enum | Repaired / withdrawn / replaced |

**Supplier**

| Object | Attribute | Type | Description |
|---|---|---|---|
| Supplier | SupplierID (PK) | Integer | Unique identifier of the supplier |
| Supplier | CompanyName | Text(100) | Name of the supplier company |
| Supplier | TaxNo | Text(20) | Tax identification number |
| Supplier | Address | Text(150) | Supplier address |
| Supplier | Phone | Text(20) | Contact phone |
| Supplier | Email | Text(100) | Contact e-mail |
| Supplier | ContactPerson | Text(100) | Person to contact |

**Supplier Offer**

| Object | Attribute | Type | Description |
|---|---|---|---|
| SupplierOffer | SupplierID (PK, FK) | Integer | Supplier |
| SupplierOffer | TypeID (PK, FK) | Integer | Disc type the supplier provides |
| SupplierOffer | UnitPrice | Decimal | Typical price per copy of this type |
| SupplierOffer | DeliveryDays | Integer | Typical delivery time |

**Order**

| Object | Attribute | Type | Description |
|---|---|---|---|
| Order | OrderID (PK) | Integer | Unique identifier of the order |
| Order | SupplierID (FK) | Integer | Supplier the order is placed with |
| Order | OrderDate | Date | Date the order was placed |
| Order | ExpectedDelivery | Date | Expected delivery date |
| Order | DeliveryDate | Date | Actual delivery date |
| Order | Status | Enum | Created / sent / delivered / cancelled |
| Order | TotalValue | Decimal | Total value of the order (derived) |

**Order Item**

| Object | Attribute | Type | Description |
|---|---|---|---|
| OrderItem | OrderID (PK, FK) | Integer | Order this line belongs to |
| OrderItem | LineNo (PK) | Integer | Line number in the order |
| OrderItem | TypeID (FK) | Integer | Type of the ordered discs |
| OrderItem | Title | Text(100) | Ordered title |
| OrderItem | Quantity | Integer | Number of copies ordered |
| OrderItem | UnitPrice | Decimal | Price per copy |

**User (Owner / Staff)**

| Object | Attribute | Type | Description |
|---|---|---|---|
| User | UserID (PK) | Integer | Unique identifier of the user |
| User | Login | Text(30) | Login name |
| User | PasswordHash | Text(255) | Encrypted password |
| User | Role | Enum | Owner / staff |

*PK = primary key, FK = foreign key, derived = calculated by the system.*

### 4.2 Relationships between objects

| Object A | Relationship | Object B | Multiplicity | Description |
|---|---|---|---|---|
| Customer | has | RentalCard | 1 : 1 | Each customer has exactly one active rental card |
| RentalCard | is used for | Rental | 1 : N | One card holds many rentals; each rental is made with one card |
| Rental | includes | RentalItem | 1 : N | One rental contains one or more discs |
| Disc | appears in | RentalItem | 1 : N | One disc copy is rented many times over its life |
| DiscType | classifies | Disc | 1 : N | Each disc has one type; a type has many discs |
| Disc | has | DamageReport | 1 : N | A disc may be damaged (zero or more reports) |
| Rental | causes | DamageReport | 1 : N (optional) | Damage may be linked to the rental during which it happened |
| Supplier | provides | DiscType | M : N (via SupplierOffer) | A supplier offers many types; a type is offered by many suppliers |
| Supplier | receives | Order | 1 : N | Each order goes to exactly one supplier |
| Order | contains | OrderItem | 1 : N | One order has one or more lines |
| DiscType | is ordered in | OrderItem | 1 : N | Each order line concerns one disc type |
| Supplier | delivered | Disc | 1 : N | Each disc copy comes from one supplier |
| User | records | Rental | 1 : N | Each rental is recorded by one user |

### 4.3 How the year-end reports are obtained from the data

| Report | Data used |
|---|---|
| List of customers | Customer + RentalCard |
| List of available discs | Disc where Status = *available* (grouped by DiscType) |
| List of rentals (who, how many, which discs) | Customer → RentalCard → Rental → RentalItem → Disc, for the year, with COUNT of items per customer |
| List of damaged discs | DamageReport + Disc for the year (or Disc where Status = *damaged*) |
| List of debtors | RentalItem where ReturnDate > DueDate **or** ReturnDate is empty and DueDate < today, with the customer and penalty |
| Average rental time per disc type | AVG(ReturnDate − RentalDate) of returned RentalItems in the year, grouped by Disc.TypeID |
| Orders for new discs | Order + OrderItem + Supplier for the year |

---

## Summary of Expected Outcome

| # | Required element | Where |
|---|---|---|
| 1 | Selected analysis method | Question 1 — Object-Oriented Analysis with UML |
| 2 | Defined system requirements | Question 2 — FR1–FR14, NFR1–NFR8 |
| 3 | List of system functions | Section 1 — F1–F11 |
| 4 | Real-world objects with justification | Section 2 — 12 objects |
| 5 | System goal and functional scope | Section 3 |
| 6 | Complete data dictionary | Section 4 — attributes, relationships, report sources |
