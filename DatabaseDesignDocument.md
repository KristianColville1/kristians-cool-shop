# Database Design Document

![DatabaseImage](docs/images/database-storage.png)

**Kristian Colville**

**W20114790**

**Databases**

**11/10/2025**

## Table of Contents

- [Revision History](#revision-history)
- [1. Purpose](#1-purpose)
  - [1.1 Document Objectives](#11-document-objectives)
  - [1.2 Scope of activities](#12-scope-of-activities)
  - [1.3 Intended Audience](#13-intended-audience)
  - [1.4 Methodology](#14-methodology)
  - [1.5 Security and Privacy Considerations](#15-security-and-privacy-considerations)
- [2. System Description](#2-system-description)
  - [2.1 Problem Domain and Contextual Background](#21-problem-domain-and-contextual-background)
  - [2.2 Stakeholders and User Requirements](#22-stakeholders-and-user-requirements)
  - [2.3 Objects of Interest (Entities) and Their Roles](#23-objects-of-interest-entities-and-their-roles)
  - [2.4 Relationships Among Objects](#24-relationships-among-objects)
  - [2.5 Functional Requirements (Transactions, Operations, Queries)](#25-functional-requirements-transactions-operations-queries)
  - [2.6 Assumptions, Constraints, and Dependencies](#26-assumptions-constraints-and-dependencies)
- [3. Conceptual Data Model](#3-conceptual-data-model)
  - [3.1 Rationale for Chosen Modelling Approach](#31-rationale-for-chosen-modelling-approach)
  - [3.2 Entity Definitions and Attributes](#32-entity-definitions-and-attributes)
  - [3.3 Relationships and Cardinalities](#33-relationships-and-cardinalities)
  - [3.4 Constraints and Business Rules (conceptual level)](#34-constraints-and-business-rules-conceptual-level)
  - [3.5 Advanced Data Modelling Features](#35-advanced-data-modelling-features)
  - [3.6 Conceptual ER Diagram (annotated)](#36-conceptual-er-diagram-annotated)
- [4. Logical Data Model](#4-logical-data-model)
  - [4.1 Mapping Process from Conceptual to Logical](#41-mapping-process-from-conceptual-to-logical)
  - [4.2 Relational Schema (Tables, Primary Keys, Foreign Keys)](#42-relational-schema-tables-primary-keys-foreign-keys)
  - [4.3 Normalization and Justification (1NF → 3NF or BCNF if applicable)](#43-normalization-and-justification-1nf--3nf-or-bcnf-if-applicable)
  - [4.4 Integrity Constraints (domain, entity, referential)](#44-integrity-constraints-domain-entity-referential)
  - [4.5 Logical ER Diagram / Relational Diagram](#45-logical-er-diagram--relational-diagram)
  - [4.6 Example SQL Definitions (CREATE TABLE statements with constraints)](#46-example-sql-definitions-create-table-statements-with-constraints)
- [5. Conclusion](#5-conclusion)
  - [5.1 Summary of Design Outcomes](#51-summary-of-design-outcomes)
  - [5.2 Limitations of the Current Design](#52-limitations-of-the-current-design)
  - [5.3 Next Steps Toward Implementation and Testing](#53-next-steps-toward-implementation-and-testing)
- [Acronyms and Abbreviations](#acronyms-and-abbreviations)
- [Appendices](#appendices)

## Revision History

| Date       | Version | Author         | Change/Note                                                                                                              |
| ---------- | ------- | -------------- | ------------------------------------------------------------------------------------------------------------------------ |
| 28-09-2025 | 0.1     | Document Owner | Initial draft, rough outline with table of contents. Prepping sections and overall information to be included.           |
| 28-09-2025 | 0.1.1   | Document Owner | Defined purpose, edited and drafted section 1.                                                                           |
| 01-10-2025 | 0.2     | Document Owner | Started Section 2, chose problem domain for assignment.                                                                  |
| 01-10-2025 | 0.2.1   | Document Owner | Extracted database user requirements from the problem domain.                                                            |
| 02-10-2025 | 0.2.2   | Document Owner | Created table for objects of interest and formatted.                                                                     |
| 02-10-2025 | 0.2.3   | Document Owner | Started working on the relationships and formatting for table.                                                           |
| 02-10-2025 | 0.2.4   | Document Owner | Prepared half of section 2 for proof reading and editing. Relationship participations to be checked over again.          |
| 02-10-2025 | 0.2.5   | Document Owner | Converted document to markdown file with libre office and set up github repo for project. I hate word.                   |
| 02-10-2025 | 0.3     | Document Owner | Started working on drafting section 3 and figuring out what to do next.                                                  |
| 05-10-2025 | 0.3.1   | Document Owner | Proofing document. Identified several formatting issues to fix.                                                          |
| 05-10-2025 | 0.4     | Document Owner | Identified redundancy between sections 2 and 3 - detailed technical specs were repeated                                  |
| 05-10-2025 | 0.4.1   | Document Owner | Moved detailed entity attributes and cardinalities from Section 2 to Section 4 (Logical Data Model)                      |
| 05-10-2025 | 0.4.2   | Document Owner | Rewrote Section 2.3 and 2.4 to be business-focused and conceptual only                                                   |
| 05-10-2025 | 0.4.3   | Document Owner | Updated Section 3.2 to provide proper conceptual entity definitions                                                      |
| 05-10-2025 | 0.4.4   | Document Owner | Created Section 4 (Logical Data Model) with all detailed technical specifications                                        |
| 05-10-2025 | 0.4.5   | Document Owner | Document now follows proper DDD conventions with clear separation between business concepts and technical implementation |
|            |         |                |                                                                                                                          |
|            |         |                |                                                                                                                          |
|            |         |                |                                                                                                                          |
|            |         |                |                                                                                                                          |

## 1. Purpose

The Database Design Document explains how the information structure of the system is turned into an actual working database. The purpose is to take the ideas and relationships defined early in the planning stages and show how they will be built in the database software we use. It bridges the gap between the blueprint of the data and the way that data will be stored, organised and accessed.

The document also considers how the database will perform in production. It will look at the system and examine how it will be used day to day and make sure that the design supports the use case. We are taking an abstract design and moving it into a practical one. The DDD ensures that the system can handle the problem domain while keeping information structured and easy to work with.

### 1.1 Document Objectives

The DDD has the following objects:

1. Describe the design
2. Basis for implementation
3. Offer a clear road map for development

### 1.2 Scope of activities

The scope of activities that resulted in the development of the DDD.

- **Research and review** – Studied sample data and existing DDDs to understand common practices, structures, and usage.
- **Document structuring** – Organised formatting and content flow, ensuring logical relationships between sections and a clear, navigable layout.
- **Problem domain exploration** – Analysed the business context and requirements to ensure the design aligns with real-world needs.
- **Tools and techniques** – Identified and applied the appropriate methods, software, and frameworks to support the design process.
- **Language specification research** – Ensured accurate use of terminology and consistency with technical and project standards.
- **Requirements gathering** – Captured functional and non-functional needs to inform the database design.
- **Project planning and time management** – Scheduled tasks and milestones to complete Version 1 of the document

### 1.3 Intended Audience

This document is intended for the following audiences:

- **Technical reviewers** – To evaluate the structure, accuracy, and completeness of the database design.
- **Developers** – To use the document as a guide during the implementation and testing phases.
- **Administrators** – To understand how the database will be maintained, optimised, and supported in practice.
- **Assessors** – To review the document as part of the evaluation process, ensuring it demonstrates understanding of database design principles and application.

### 1.4 Methodology

The development of this DDD was a step-by-step process, guided by research and careful planning. It began with a simple table of contents, which served as a roadmap and helped organize the work in a logical sequence. Along the way, similar documents and supporting materials were reviewed to understand common practices and ensure the final version met professional standards.

The document was built up in stages. Early drafts covered broad, general details typical of projects like this. These were then refined and adapted to fit the specific problem domain chosen for the module. As the work progressed, database design methods came into play—especially the creation and refinement of Entity Relationship Diagrams—to make sure the design aligned with the requirements.

A revision history was maintained throughout to track changes and improvements as the document evolved. The whole drafting process was managed against the planned schedule, with the goal of not only fulfilling the document's purpose but also showing how database design techniques can be applied in practice.

### 1.5 Security and Privacy Considerations

When designing relational databases, security and privacy should be considered alongside structure and relationships. A few key points are:

1. **Access control** – Different users should have different permissions. For example, an administrator might have full rights to create, update, and delete tables, while a standard user may only need specific access to certain views. Good use of roles and privileges prevents unnecessary exposure of data.
2. **Data protection** – Sensitive attributes (such as names, addresses, or payment details) should be identified during the design stage and, where possible, separated into their own entities. This makes it easier to apply stricter controls or encryption to those specific tables without affecting the whole schema.
3. **Good practices** – Avoid storing plain text credentials, never embed passwords in the design itself, and ensure relationships between entities are designed with the principle of least privilege in mind. (And yes, using password123 would break every best practice—and probably a few hearts along the way.)

Although this project is a learning exercise, it highlights how security and privacy need to be considered at the design level, not added later. In relational systems, how entities connect is just as important for protecting data as it is for modelling it.

## 2. System Description

This section provides a comprehensive overview of the business system that the database will support. It identifies the problem domain, key stakeholders, business entities, and functional requirements that drive the database design. The system description serves as the foundation for the conceptual and logical data models that follow, ensuring that the database design aligns with real-world business needs and operational requirements.

The analysis covers the e-commerce platform's current challenges, user requirements, and business processes that must be supported by the database system. This business-focused perspective ensures that the subsequent technical design decisions are grounded in actual operational needs rather than abstract database theory.

### 2.1 Problem Domain and Contextual Background

This project involves designing a database for Kristian's Cool Shop, an e-commerce platform specializing in unique and novelty products. The business has experienced significant growth, but the current database architecture is unable to scale effectively to meet increased demand.

**Current System Challenges:**

- Performance degradation during peak traffic periods
- Inadequate support for concurrent user operations
- Limited scalability for growing product catalog and customer base
- Inefficient data structures causing slow query response times
- Inflexible product data model unable to accommodate category-specific attributes
- Single product table forces all items to use identical attribute structure regardless of product type

**Business Requirements:**
The new database design must support a modern e-commerce platform capable of handling:

- High-volume transaction processing
- Real-time inventory management
- Multi-user concurrent access
- Comprehensive customer relationship management
- Integrated payment processing
- Customer support ticket management

**Migration Objectives:**
The database redesign focuses on creating a scalable, normalized structure that can support current operations while providing a foundation for future growth. The new design will implement proper entity relationships, data integrity constraints, and optimized data structures to address the performance and scalability issues of the current system.

### 2.2 Stakeholders and User Requirements

Several stakeholder groups interact with the system, each with distinct requirements:

- **Customers**

  - Browse and search for products.
  - Place and track orders.
  - Manage account information (e.g., contact details, payment methods).
- **Store Administrators**

  - Manage product listings (add, update, or remove items).
  - Manage category-specific product attributes (e.g., size for clothing, brand for electronics).
  - Monitor sales and generate reports.
  - Control user permissions and system settings.
- **Customer Support Agents**

  - Access customer order histories to resolve queries.
  - Track and update support tickets.
  - Communicate with customers about order issues.
- **Management**

  - Analyse sales data for business insights.
  - Monitor system usage and performance.
  - Ensure compliance with security and privacy standards.

From these roles, the following user requirements were identified:

1. The system must record customers, products, and orders with clear relationships.
2. The system must allow customers to place and track orders.
3. Administrators must be able to update product and inventory information.
4. Customer support must be able to link queries and tickets to specific orders or accounts.
5. Management must be able to generate queries and reports.
6. The system must support different product types with specialized attributes while maintaining data integrity.

### 2.3 Objects of Interest (Entities) and Their Roles

This section identifies the primary business entities that the system must manage. These entities represent the core concepts and objects that exist within Kristian's Cool Shop's business domain. The focus here is on understanding what business objects need to be tracked and managed, rather than the technical details of how they will be implemented.

| Entity                  | Business Role                                            | Key Business Concept                                 |
| ----------------------- | -------------------------------------------------------- | ---------------------------------------------------- |
| **Customer**      | People who interact with the store                       | Individuals who browse, purchase, and seek support   |
| **Product**       | Core product information shared across all product types | Common attributes for all merchandise in inventory   |
| **Electronics**   | Electronic products with specialized attributes          | Brand, model, warranty specifications for tech items |
| **Clothing**      | Apparel items with size and material attributes          | Size, color, material details for clothing items     |
| **Books**         | Publications with author and publisher information       | Author, ISBN, publisher details for book items       |
| **Category**      | Product organization system                              | Hierarchical structure for organizing products       |
| **Order**         | Customer purchase transactions                           | Records of what customers have bought                |
| **OrderItem**     | Individual products within orders                        | Line-level details of each product in a purchase     |
| **Payment**       | Financial transactions                                   | Records of how orders were paid                      |
| **SupportTicket** | Customer service interactions                            | Issues and requests from customers                   |
| **Employee**      | Staff members                                            | People who manage the system and provide support     |

### 2.4 Relationships Among Objects

This section describes the key business relationships between entities, focusing on how the business objects interact and depend on each other. The relationships are described conceptually to understand the business logic and constraints.

| Relationship                       | Business Description                            | Key Business Rule                                                             |
| ---------------------------------- | ----------------------------------------------- | ----------------------------------------------------------------------------- |
| Customer places Order              | Customers can make purchases                    | Every order must be associated with a customer                                |
| Order contains Products            | Orders can include multiple products            | Orders track which products were purchased and in what quantities             |
| Product belongs to Category        | Products can be organized into categories       | Products may be categorized to help customers find items                      |
| Product has specialized attributes | Products can have type-specific details         | Products must belong to exactly one subtype (Electronics, Clothing, or Books) |
| Category has subcategories         | Categories can be organized hierarchically      | Categories can have parent-child relationships for better organization        |
| Order has Payment                  | Orders must be paid for                         | Orders can have associated payment records                                    |
| Customer opens SupportTicket       | Customers can request help                      | Support tickets are always linked to a customer                               |
| SupportTicket relates to Order     | Support issues may be about specific orders     | Support tickets can optionally reference specific orders                      |
| Employee handles SupportTicket     | Staff members can be assigned to resolve issues | Support tickets can be assigned to employees for resolution                   |
| Employee reports to Employee       | Staff have management relationships             | Employees can have supervisors in the organizational structure                |

### 2.5 Functional Requirements (Transactions, Operations, Queries)

This section outlines the key functional requirements of Kristian's Cool Shop. These requirements describe the operations the database must support for different stakeholders, including customers, administrators, and support staff. They are expressed as typical transactions or queries that demonstrate how the database will be used in practice.

| Requirement                       | Stakeholder       | Example Transaction / Query                                                                           |
| --------------------------------- | ----------------- | ----------------------------------------------------------------------------------------------------- |
| Place a new order                 | Customer          | Insert a new record into Order, with related OrderItem rows referencing selected Products.            |
| View order history                | Customer          | `SELECT * FROM Order WHERE CustomerID = ? ORDER BY OrderDate DESC;`                                 |
| Update stock levels               | Administrator     | `UPDATE Product SET StockQty = StockQty - ? WHERE ProductID = ?;`                                   |
| Generate sales report             | Management        | `SELECT ProductID, SUM(Quantity) FROM OrderItem GROUP BY ProductID;`                                |
| Manage product listings           | Administrator     | Insert, update, or delete rows in Product and assign them to a Category.                              |
| Manage product subtypes           | Administrator     | Insert, update, or delete rows in Electronics, Clothing, or Books tables with appropriate attributes. |
| Validate product type consistency | System            | Ensure Product.ProductType matches the corresponding subtype table (Electronics, Clothing, or Books). |
| Record a payment                  | Customer / System | Insert into Payment (with subtype row in CardPayment or PayPalPayment) linked to an Order.            |
| Track unresolved support tickets  | Support Agent     | `SELECT * FROM SupportTicket WHERE Status = 'Open' AND AssignedTo IS NULL;`                         |
| Escalate a ticket                 | Support Agent     | Update SupportTicket with new EmployeeID (manager assignment).                                        |
| Analyse staff workload            | Management        | Count how many SupportTickets are assigned to each Employee.                                          |

### 2.6 Assumptions, Constraints, and Dependencies

**Assumptions**

* Customers are required to register before placing an order (guest checkouts are not considered).
* Each order must contain at least one product.
* Payments are assumed to succeed once recorded; payment gateway errors are handled outside the database.
* Support tickets are always linked to a customer account, but linking them to a specific order is optional.

**Constraints**

* Primary keys must uniquely identify every entity (e.g., CustomerID, OrderID).
* Foreign key constraints enforce valid relationships (e.g., every Order must reference a valid Customer).
* Composite attributes, such as Customer Address, will be normalised into separate fields (Street, City, Postcode, Country) for clarity.
* OrderItem is a weak entity and cannot exist without its parent Order.
* Subtypes of Payment (CardPayment, PayPalPayment) enforce disjointness — a payment record must belong to exactly one subtype.
* Products must be assigned to exactly one product type (Electronics, Clothing, or Books).
* Subtype attributes are only applicable to products of the corresponding type.

**Dependencies**

* The database design assumes integration with an external payment system.
* Customer support processes depend on staff availability; the database only manages tickets and assignments, not communication channels.
* Product categories may be hierarchical; the recursive relationship assumes front-end or business logic prevents circular dependencies.
* Reporting and analytics queries depend on accurate and consistent transaction data being recorded in real time.

## 3. Conceptual Data Model

The Conceptual Data Model represents the high-level view of Kristian's Cool Shop database, focusing on the essential entities, their attributes, and relationships without concern for implementation details. This section translates the business requirements from Section 2 into a formal data model that demonstrates the complexity and relationships needed to support the e-commerce platform's operations.

The model incorporates several advanced database concepts to handle the real-world complexity of the system, including weak entities for order line items, recursive relationships for hierarchical categories and employee management, and subtype/supertype structures for different payment methods.

### 3.1 Rationale for Chosen Modelling Approach

The Entity-Relationship (ER) modeling approach was selected for developing the conceptual data model of Kristian's Cool Shop. This choice was made after considering several alternative modeling approaches.

**Why ER Modeling is Appropriate:**

ER modeling excels at representing complex relationships between distinct business entities, making it ideal for this e-commerce system with multiple interconnected entities (customers, products, orders, payments). As noted by GeeksforGeeks, "ER diagrams represent the E-R model in a database, making them easy to convert into relations (tables)" (GeeksforGeeks, 2025), which aligns perfectly with our SQL-based implementation target.

**Alternative Approaches Considered:**

- **Object-Oriented Modeling**: Overkill for this relational database project and doesn't align with SQL implementation.
- **UML Class Diagrams**: More suited for software design rather than database design, adding unnecessary complexity.
- **Relational Model**: Starting directly with tables would skip the important conceptual phase.

**Specific Benefits for This Project:**

The ER approach directly addresses the scaling issues mentioned in the problem domain. By clearly modeling entities and relationships first, we can identify potential bottlenecks (like the many-to-many relationship between orders and products) and design appropriate solutions (the OrderItem weak entity) before implementation. This upfront modeling prevents the "clunky monolithic setup" problems that led to the current system's performance issues.

The ER model also naturally supports the advanced concepts required by the assignment - weak entities for order line items, recursive relationships for category hierarchies and employee management structures, and subtype/supertype relationships for different payment methods.

### 3.2 Entity Definitions and Attributes

This section provides conceptual definitions of each entity in the data model, focusing on their business purpose and key characteristics rather than technical implementation details. The entities are defined from a business perspective to ensure the conceptual model accurately represents the real-world system.

<details>
<summary><strong>Customer Entity</strong></summary>

**Business Definition**: Represents individuals who interact with Kristian's Cool Shop, including browsers, purchasers, and support requesters. Customers are the central actors in the e-commerce system and must be registered to place orders.

**Key Business Characteristics**:

- Each customer has a unique identity within the system
- Customers can exist independently of orders (they may browse without purchasing)
- Customer information is required for order processing and support
- Customers can have multiple orders over time

</details>

<details>
<summary><strong>Product Entity (Supertype)</strong></summary>

**Business Definition**: Represents the core product information shared across all product types in the online store. This entity serves as the supertype that contains common attributes for all merchandise, while specialized attributes are managed through subtype entities.

**Key Business Characteristics**:

- Each product has a unique business identifier (SKU)
- Products exist independently of orders (inventory items)
- Products can be organized into categories for better customer navigation
- Product information includes pricing and availability status
- Every product must belong to exactly one subtype (Electronics, Clothing, or Books)
- Common attributes are stored in the supertype, specialized attributes in subtypes

</details>

<details>
<summary><strong>Electronics Entity (Subtype)</strong></summary>

**Business Definition**: Represents electronic products with specialized attributes specific to technology items. This subtype contains attributes that are only relevant to electronic products.

**Key Business Characteristics**:

- Contains brand, model, and warranty information for electronic items
- Dependent on the Product supertype for core product information
- Attributes are only applicable to products classified as electronics
- Supports detailed product specifications for technical items

</details>

<details>
<summary><strong>Clothing Entity (Subtype)</strong></summary>

**Business Definition**: Represents apparel items with specialized attributes specific to clothing and fashion items. This subtype contains attributes that are only relevant to clothing products.

**Key Business Characteristics**:

- Contains size, color, material, and care instruction information
- Dependent on the Product supertype for core product information
- Attributes are only applicable to products classified as clothing
- Supports detailed product specifications for fashion items

</details>

<details>
<summary><strong>Books Entity (Subtype)</strong></summary>

**Business Definition**: Represents publications with specialized attributes specific to books and printed materials. This subtype contains attributes that are only relevant to book products.

**Key Business Characteristics**:

- Contains author, ISBN, publisher, and page count information
- Dependent on the Product supertype for core product information
- Attributes are only applicable to products classified as books
- Supports detailed product specifications for literary items

</details>

<details>
<summary><strong>Category Entity</strong></summary>

**Business Definition**: Represents the organizational structure used to group and classify products. Categories help customers navigate the product catalog and find items of interest.

**Key Business Characteristics**:

- Categories can be organized hierarchically (parent-child relationships)
- Categories can exist independently of products (empty categories are allowed)
- The category structure supports flexible product organization
- Categories help customers browse and filter products

</details>

<details>
<summary><strong>Order Entity</strong></summary>

**Business Definition**: Represents a customer's purchase transaction. Orders capture the business event of a customer deciding to buy products from the store.

**Key Business Characteristics**:

- Every order must be associated with a customer
- Orders represent the business transaction, not just a shopping cart
- Orders have a lifecycle from creation to completion
- Orders can contain multiple products with different quantities

</details>

<details>
<summary><strong>OrderItem Entity (Weak Entity)</strong></summary>

**Business Definition**: Represents individual products within an order, capturing the specific details of what was purchased. OrderItems are dependent on their parent order and cannot exist independently.

**Key Business Characteristics**:

- OrderItems have no meaning without their parent order
- Each OrderItem captures the quantity and pricing at the time of purchase
- OrderItems preserve historical pricing information
- OrderItems resolve the many-to-many relationship between orders and products

</details>

<details>
<summary><strong>Payment Entity</strong></summary>

**Business Definition**: Represents financial transactions associated with orders. Payments record how customers pay for their purchases and support multiple payment methods.

**Key Business Characteristics**:

- Every payment must be associated with an order
- Payments can be processed after order creation
- Different payment methods may have different characteristics
- Payment records support financial reconciliation and reporting

</details>

<details>
<summary><strong>SupportTicket Entity</strong></summary>

**Business Definition**: Represents customer service interactions and issues. SupportTickets manage the communication and resolution process between customers and support staff.

**Key Business Characteristics**:

- Every support ticket must be associated with a customer
- Support tickets can optionally reference specific orders
- Support tickets can be assigned to employees for resolution
- Support tickets have a lifecycle from creation to resolution

</details>

<details>
<summary><strong>Employee Entity</strong></summary>

**Business Definition**: Represents staff members who manage the system, handle customer support, and perform administrative functions. Employees are the internal users of the system.

**Key Business Characteristics**:

- Employees can be assigned to handle support tickets
- Employees can have management relationships (supervisor-subordinate)
- Employees have different roles and responsibilities
- Employee information supports workload management and escalation

</details>

### 3.3 Relationships and Cardinalities

### 3.4 Constraints and Business Rules (conceptual level)

### 3.5 Advanced Data Modelling Features

The conceptual data model incorporates several advanced database modeling concepts to handle the complexity of the e-commerce system:

- **Multivalued Attributes**: Attributes that can have multiple values for a single entity instance
- **Recursive Relationships**: Self-referencing relationships that model hierarchical structures (Category parent-child relationships, Employee management hierarchy)
- **Subtype/Supertype Structures**: The Product entity serves as a supertype with Electronics, Clothing, and Books as subtypes. This allows for shared common attributes (name, price, stock) while supporting specialized attributes (brand for electronics, size for clothing, author for books). The relationship enforces disjointness - each product belongs to exactly one subtype
- **Weak Entities**: OrderItem is a weak entity that depends on its parent Order for existence and identity

### 3.6 Conceptual ER Diagram (annotated)

## 4. Logical Data Model

The Logical Data Model represents the detailed technical specification of the database design, translating the conceptual business model from Section 3 into a formal relational structure that can be implemented in a database management system. This section focuses on the technical aspects of data storage, relationships, and constraints without considering the specific implementation details of any particular database platform.

The logical model serves as the bridge between the business-focused conceptual model and the physical database implementation. It defines the precise structure of tables, attributes, data types, and relationships that will support all the business requirements identified in the earlier sections while ensuring data integrity and optimal performance.

### 4.1 Mapping Process from Conceptual to Logical

The transformation from the conceptual data model to the logical data model follows established database design principles and involves several key mapping processes:

<details>
<summary><strong>Entity-to-Table Mapping</strong></summary>

Each entity identified in the conceptual model is mapped to a corresponding table in the logical model:

- **Strong entities** become independent tables with their own primary keys
- **Weak entities** become tables with composite primary keys that include foreign keys to their parent entities
- **Entity attributes** become table columns with appropriate data types and constraints
- **Subtype/supertype relationships** are implemented using separate tables with foreign key relationships to the supertype

</details>

<details>
<summary><strong>Relationship Mapping</strong></summary>

The relationships defined in the conceptual model are implemented through foreign key constraints:

- **One-to-many relationships** are implemented using foreign keys in the "many" side table
- **Many-to-many relationships** are resolved through intermediate tables (like OrderItem)
- **Recursive relationships** are implemented using self-referencing foreign keys
- **Subtype/supertype relationships** are implemented using separate tables with foreign key relationships

</details>

<details>
<summary><strong>Subtype/Supertype Implementation</strong></summary>

The Product supertype and its subtypes (Electronics, Clothing, Books) are implemented as follows:

- **Product table** contains common attributes (ProductID, Name, Description, UnitPrice, StockQty, Status, ProductType)
- **ProductType discriminator** column indicates which subtype table contains additional attributes
- **Subtype tables** (Electronics, Clothing, Books) use ProductID as both primary key and foreign key to Product
- **Disjointness constraint** ensures each product belongs to exactly one subtype
- **Referential integrity** is maintained through foreign key constraints between subtype tables and Product

</details>

<details>
<summary><strong>Attribute Transformation</strong></summary>

Conceptual attributes are transformed into technical specifications:

- **Composite attributes** (like Address) are decomposed into separate columns
- **Multivalued attributes** are handled through separate tables or normalization
- **Derived attributes** are either stored as computed columns or calculated at query time
- **Business identifiers** are preserved alongside system-generated surrogate keys

</details>

<details>
<summary><strong>Constraint Implementation</strong></summary>

Business rules and constraints from the conceptual model are enforced through:

- **Primary key constraints** ensuring entity uniqueness
- **Foreign key constraints** maintaining referential integrity
- **Check constraints** enforcing domain rules and business logic
- **Unique constraints** preventing duplicate business identifiers

</details>

This mapping process ensures that all business requirements captured in the conceptual model are properly supported by the logical database structure while maintaining data integrity and supporting efficient query operations.

### 4.2 Relational Schema (Tables, Primary Keys, Foreign Keys)

This section provides the detailed technical specifications for each entity, including all attributes, data types, primary keys, foreign keys, and constraints. This represents the logical data model that will be implemented in the database system.

#### **Customer Table**

| Attribute  | Data Type                               | Constraints                         | Description                                 |
| ---------- | --------------------------------------- | ----------------------------------- | ------------------------------------------- |
| CustomerID | INTEGER                                 | PRIMARY KEY, AUTO_INCREMENT         | Unique identifier for each customer         |
| Email      | VARCHAR(255)                            | UNIQUE, NOT NULL                    | Customer's email address for authentication |
| FullName   | VARCHAR(255)                            | NOT NULL                            | Customer's complete name                    |
| Phone      | VARCHAR(20)                             | NULL                                | Contact phone number                        |
| Street     | VARCHAR(255)                            | NULL                                | Street address                              |
| City       | VARCHAR(100)                            | NULL                                | City name                                   |
| Postcode   | VARCHAR(20)                             | NULL                                | Postal code                                 |
| Country    | VARCHAR(100)                            | NULL                                | Country name                                |
| CreatedAt  | TIMESTAMP                               | NOT NULL, DEFAULT CURRENT_TIMESTAMP | Account creation timestamp                  |
| Status     | ENUM('Active', 'Suspended', 'Inactive') | NOT NULL, DEFAULT 'Active'          | Account status                              |

#### **Product Table**

| Attribute   | Data Type                                      | Constraints                 | Description                        |
| ----------- | ---------------------------------------------- | --------------------------- | ---------------------------------- |
| ProductID   | INTEGER                                        | PRIMARY KEY, AUTO_INCREMENT | Unique identifier for each product |
| SKU         | VARCHAR(50)                                    | UNIQUE, NOT NULL            | Stock Keeping Unit for inventory   |
| Name        | VARCHAR(255)                                   | NOT NULL                    | Product name                       |
| Description | TEXT                                           | NULL                        | Detailed product description       |
| UnitPrice   | DECIMAL(10,2)                                  | NOT NULL                    | Current selling price              |
| StockQty    | INTEGER                                        | NOT NULL, DEFAULT 0         | Available inventory quantity       |
| Status      | ENUM('Active', 'Discontinued', 'Out of Stock') | NOT NULL, DEFAULT 'Active'  | Product availability status        |
| ProductType | ENUM('Electronics', 'Clothing', 'Books')       | NOT NULL                    | Discriminator for subtype tables   |

#### **Electronics Table (Subtype)**

| Attribute      | Data Type    | Constraints                                            | Description                 |
| -------------- | ------------ | ------------------------------------------------------ | --------------------------- |
| ProductID      | INTEGER      | PRIMARY KEY, FOREIGN KEY REFERENCES Product(ProductID) | Reference to parent product |
| Brand          | VARCHAR(100) | NOT NULL                                               | Manufacturer or brand name  |
| Model          | VARCHAR(100) | NULL                                                   | Product model number        |
| WarrantyPeriod | INTEGER      | NULL                                                   | Warranty period in months   |
| Specifications | TEXT         | NULL                                                   | Technical specifications    |

#### **Clothing Table (Subtype)**

| Attribute        | Data Type    | Constraints                                            | Description                       |
| ---------------- | ------------ | ------------------------------------------------------ | --------------------------------- |
| ProductID        | INTEGER      | PRIMARY KEY, FOREIGN KEY REFERENCES Product(ProductID) | Reference to parent product       |
| Size             | VARCHAR(20)  | NOT NULL                                               | Clothing size (S, M, L, XL, etc.) |
| Color            | VARCHAR(50)  | NOT NULL                                               | Primary color of the item         |
| Material         | VARCHAR(100) | NULL                                                   | Fabric or material composition    |
| CareInstructions | TEXT         | NULL                                                   | Washing and care instructions     |

#### **Books Table (Subtype)**

| Attribute | Data Type    | Constraints                                            | Description                        |
| --------- | ------------ | ------------------------------------------------------ | ---------------------------------- |
| ProductID | INTEGER      | PRIMARY KEY, FOREIGN KEY REFERENCES Product(ProductID) | Reference to parent product        |
| Author    | VARCHAR(255) | NOT NULL                                               | Book author name                   |
| ISBN      | VARCHAR(20)  | UNIQUE, NULL                                           | International Standard Book Number |
| Pages     | INTEGER      | NULL                                                   | Number of pages                    |
| Publisher | VARCHAR(255) | NULL                                                   | Publishing company                 |

#### **Category Table**

| Attribute        | Data Type    | Constraints                                       | Description                         |
| ---------------- | ------------ | ------------------------------------------------- | ----------------------------------- |
| CategoryID       | INTEGER      | PRIMARY KEY, AUTO_INCREMENT                       | Unique identifier for each category |
| Name             | VARCHAR(255) | NOT NULL                                          | Category name                       |
| ParentCategoryID | INTEGER      | NULL, FOREIGN KEY REFERENCES Category(CategoryID) | Reference to parent category        |

#### **Order Table**

| Attribute   | Data Type                                                          | Constraints                                           | Description                      |
| ----------- | ------------------------------------------------------------------ | ----------------------------------------------------- | -------------------------------- |
| OrderID     | INTEGER                                                            | PRIMARY KEY, AUTO_INCREMENT                           | Unique identifier for each order |
| OrderDate   | TIMESTAMP                                                          | NOT NULL, DEFAULT CURRENT_TIMESTAMP                   | Order creation timestamp         |
| Status      | ENUM('Pending', 'Processing', 'Shipped', 'Delivered', 'Cancelled') | NOT NULL, DEFAULT 'Pending'                           | Current order status             |
| TotalAmount | DECIMAL(10,2)                                                      | NOT NULL                                              | Calculated total value           |
| CustomerID  | INTEGER                                                            | NOT NULL, FOREIGN KEY REFERENCES Customer(CustomerID) | Reference to customer            |

#### **OrderItem Table (Weak Entity)**

| Attribute        | Data Type     | Constraints                                         | Description                         |
| ---------------- | ------------- | --------------------------------------------------- | ----------------------------------- |
| OrderID          | INTEGER       | PRIMARY KEY, FOREIGN KEY REFERENCES Order(OrderID)  | Reference to parent order           |
| LineNo           | INTEGER       | PRIMARY KEY                                         | Sequential line number within order |
| ProductID        | INTEGER       | NOT NULL, FOREIGN KEY REFERENCES Product(ProductID) | Reference to product                |
| Quantity         | INTEGER       | NOT NULL                                            | Number of units ordered             |
| UnitPriceAtOrder | DECIMAL(10,2) | NOT NULL                                            | Price per unit at time of order     |
| LineSubtotal     | DECIMAL(10,2) | NOT NULL                                            | Calculated line total               |

#### **Payment Table**

| Attribute  | Data Type                                          | Constraints                                     | Description                        |
| ---------- | -------------------------------------------------- | ----------------------------------------------- | ---------------------------------- |
| PaymentID  | INTEGER                                            | PRIMARY KEY, AUTO_INCREMENT                     | Unique identifier for each payment |
| OrderID    | INTEGER                                            | NOT NULL, FOREIGN KEY REFERENCES Order(OrderID) | Reference to order being paid      |
| Amount     | DECIMAL(10,2)                                      | NOT NULL                                        | Payment amount                     |
| PaidAt     | TIMESTAMP                                          | NULL                                            | Payment processing timestamp       |
| Status     | ENUM('Pending', 'Completed', 'Failed', 'Refunded') | NOT NULL, DEFAULT 'Pending'                     | Payment status                     |
| MethodType | ENUM('Card', 'PayPal', 'Bank Transfer')            | NOT NULL                                        | Payment method category            |

#### **SupportTicket Table**

| Attribute  | Data Type                                         | Constraints                                           | Description                       |
| ---------- | ------------------------------------------------- | ----------------------------------------------------- | --------------------------------- |
| TicketID   | INTEGER                                           | PRIMARY KEY, AUTO_INCREMENT                           | Unique identifier for each ticket |
| OpenedAt   | TIMESTAMP                                         | NOT NULL, DEFAULT CURRENT_TIMESTAMP                   | Ticket creation timestamp         |
| Status     | ENUM('Open', 'In Progress', 'Resolved', 'Closed') | NOT NULL, DEFAULT 'Open'                              | Current ticket status             |
| Priority   | ENUM('Low', 'Medium', 'High', 'Critical')         | NOT NULL, DEFAULT 'Medium'                            | Issue priority level              |
| CustomerID | INTEGER                                           | NOT NULL, FOREIGN KEY REFERENCES Customer(CustomerID) | Reference to customer             |
| OrderID    | INTEGER                                           | NULL, FOREIGN KEY REFERENCES Order(OrderID)           | Reference to related order        |
| AssignedTo | INTEGER                                           | NULL, FOREIGN KEY REFERENCES Employee(EmployeeID)     | Reference to assigned employee    |
| Subject    | VARCHAR(255)                                      | NOT NULL                                              | Brief description of issue        |

#### **Employee Table**

| Attribute  | Data Type                                         | Constraints                                       | Description                         |
| ---------- | ------------------------------------------------- | ------------------------------------------------- | ----------------------------------- |
| EmployeeID | INTEGER                                           | PRIMARY KEY, AUTO_INCREMENT                       | Unique identifier for each employee |
| FullName   | VARCHAR(255)                                      | NOT NULL                                          | Employee's complete name            |
| Email      | VARCHAR(255)                                      | UNIQUE, NOT NULL                                  | Work email address                  |
| Role       | ENUM('Administrator', 'Support Agent', 'Manager') | NOT NULL                                          | Job function                        |
| ManagerID  | INTEGER                                           | NULL, FOREIGN KEY REFERENCES Employee(EmployeeID) | Reference to direct supervisor      |

#### **Relationship Specifications with Cardinalities**

| Relationship                             | Cardinality               | Participation                                         | Attributes                               | Rationale                                                                  |
| ---------------------------------------- | ------------------------- | ----------------------------------------------------- | ---------------------------------------- | -------------------------------------------------------------------------- |
| Customer places Order                    | 1 ⟶ 0..*                 | Customer = partial, Order = total                     |                                          | Every order must belong to a customer; not every customer places an order  |
| Order contains Product (via OrderItem)   | M:N resolved by OrderItem | OrderItem = total, Order = partial, Product = partial | Quantity, UnitPriceAtOrder, LineSubtotal | OrderItem depends on Order; records product quantities at time of purchase |
| Product belongs to Category              | Many ⟶ 0..1              | Product = partial, Category = partial                 |                                          | Products may or may not be categorised; categories may be empty            |
| Category parent-of Category (recursive)  | 0..1 ⟶ 0..*              | Parent = partial, Child = partial                     |                                          | Supports multi-level catalogue; prevents cycles                            |
| Order paid by Payment                    | 1 ⟶ 1..*                 | Order = partial, Payment = total                      | Amount, PaidAt, Status                   | Orders can exist before payment; every payment must belong to an order     |
| SupportTicket opened by Customer         | 0..* ⟶ 1                 | Customer = partial, Ticket = total                    |                                          | Every ticket must belong to a customer; some customers never open tickets  |
| SupportTicket refers to Order            | 0..* ⟶ 0..1              | Both partial                                          |                                          | A ticket may or may not be linked to an order                              |
| SupportTicket assigned to Employee       | 0..* ⟶ 0..1              | Both partial                                          |                                          | Tickets may be unassigned; some employees may have none                    |
| Employee reports to Employee (recursive) | 0..1 ⟶ 0..*              | Both partial                                          |                                          | Models reporting lines; not all employees are managers                     |

### 4.3 Normalization and Justification (1NF → 3NF or BCNF if applicable)

### 4.4 Integrity Constraints (domain, entity, referential)

The logical data model enforces data integrity through various constraint types:

#### **Domain Constraints**

- **ENUM values** restrict Status fields to predefined values (Active, Inactive, etc.)
- **Data types** ensure appropriate values (INTEGER for quantities, DECIMAL for prices)
- **NOT NULL constraints** prevent missing required information
- **CHECK constraints** validate business rules (positive quantities, valid email formats)

#### **Entity Integrity Constraints**

- **Primary key constraints** ensure unique identification of all entities
- **Unique constraints** prevent duplicate business identifiers (Email, SKU, ISBN)
- **AUTO_INCREMENT** provides system-generated surrogate keys

#### **Referential Integrity Constraints**

- **Foreign key constraints** maintain valid relationships between entities
- **Cascade rules** define behavior for updates and deletes
- **Subtype constraints** ensure ProductType discriminator matches existing subtype tables

#### **Subtype/Supertype Constraints**

- **Disjointness constraint**: Each product must belong to exactly one subtype (Electronics, Clothing, or Books)
- **Completeness constraint**: Every product with a specific ProductType must have a corresponding record in the appropriate subtype table
- **Discriminator constraint**: ProductType value must match the subtype table where the product record exists
- **Referential integrity**: Subtype tables can only reference products of their specific type

### 4.5 Logical ER Diagram / Relational Diagram

### 4.6 Example SQL Definitions (CREATE TABLE statements with constraints)

## 5. Conclusion

### 5.1 Summary of Design Outcomes

### 5.2 Limitations of the Current Design

### 5.3 Next Steps Toward Implementation and Testing

## Acronyms and Abbreviations

| Acronym / Abbreviation | Meaning                     |
| ---------------------- | --------------------------- |
| DDD                    | Database Design Document    |
| ER                     | Entity-Relationship         |
| ERD                    | Entity-Relationship Diagram |
| PK                     | Primary Key                 |
| FK                     | Foreign Key                 |
| SKU                    | Stock Keeping Unit          |

## Appendices

- **A. Sample Queries and Transactions**
- **B. Glossary of Technical Terms**
- **C. Full ERD and Schema Diagrams (large-format versions)**

## Credits

### Educational Resources

**GeeksforGeeks** - Database Management System tutorials and ER Model concepts

- [Introduction of ER Model](https://www.geeksforgeeks.org/dbms/introduction-of-er-model/)
- [Difference between UML and ER Diagram](https://www.geeksforgeeks.org/dbms/difference-between-uml-and-er-diagram/)

**SETU Waterford** - Higher Diploma in Computer Science (Databases Module)

- Learning materials and assignment guidance
- Database design principles and methodologies
- ER modeling concepts and best practices
