# Database Design Document

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

| Date       | Version | Author            | Change/Note                                                                                                     |
| ---------- | ------- | ----------------- | --------------------------------------------------------------------------------------------------------------- |
| 28-09-2025 | 0.1     | Kristian Colville | Initial draft, rough outline with table of contents. Prepping sections and overall information to be included.  |
| 28-09-2025 | 0.1.1   | Kristian Colville | Defined purpose, edited and drafted section 1.                                                                  |
| 01-10-2025 | 0.2     | Kristian Colville | Started Section 2, chose problem domain for assignment.                                                         |
| 01-10-2025 | 0.2.1   | Kristian Colville | Extracted database user requirements from the problem domain.                                                   |
| 02-10-2025 | 0.2.2   | Kristian Colville | Created table for objects of interest and formatted.                                                            |
| 02-10-2025 | 0.2.3   | Kristian Colville | Started working on the relationships and formatting for table.                                                  |
| 02-10-2025 | 0.2.4   | Kristian Colville | Prepared half of section 2 for proof reading and editing. Relationship participations to be checked over again. |
| 02-10-2025 | 0.2.5   | Kristian Colville | Converted document to markdown file with libre office and set up github repo for project. I hate word.          |

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

### 2.1 Problem Domain and Contextual Background

For this project, I've chosen to design a database for Kristian's Cool Shop – an online store that sells all sorts of random but "cool" stuff. The shop has been growing quickly, which is great, but the current system is built on a clunky monolithic setup that just can't handle the traffic anymore. Pages are slowing down, customer support is constantly overwhelmed, and the whole thing is showing cracks when people hit the site during busy hours.

Strangely enough, most of these "busy hours" seem to happen late at night, probably when people are scrolling on their phones and impulse-buying things (because of course that's when the urge to buy comes).

To fix the mess, the company has decided to migrate the platform and completely rethink how the database is structured. They've even brought in a new database administrator, nicknamed Mr. Freeze for his ability to keep his cool while everything around him falls apart. His main point is that the database design is at the heart of the scaling issues, so reworking the entities, relationships, and transactions is a big part of the solution.

This project is about building that new design. The focus will be on mapping out the key entities (like customers, products, and orders), the relationships between them, and the operations the system needs to support so that Kristian's Cool Shop can actually keep up with demand without collapsing every time someone decides they need a novelty mug at 2am.

### 2.2 Stakeholders and User Requirements

Several stakeholder groups interact with the system, each with distinct requirements:

- **Customers**

  - Browse and search for products.
  - Place and track orders.
  - Manage account information (e.g., contact details, payment methods).
- **Store Administrators**

  - Manage product listings (add, update, or remove items).
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

### 2.3 Objects of Interest (Entities) and Their Roles

This section lists the primary entities that make up the database design. Each entity represents a real-world object or concept in the system, such as customers, products, or orders. Their attributes, including primary and foreign keys, will later be used to construct the ER diagram and logical schema. The table below provides a concise description of these entities and their roles within the system.

<table>
<thead>
<tr>
<th>Entity</th>
<th>Role</th>
<th>Key Attributes</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Customer</strong></td>
<td>People who browse, buy and contact support.</td>
<td>• CustomerID (PK)<br>• Email (unique)<br>• FullName<br>• Phone<br>• Address (composite: Street, City, Postcode, Country)<br>• CreatedAt<br>• Status</td>
</tr>
<tr>
<td><strong>Product</strong></td>
<td>Items for sale.</td>
<td>• ProductID (PK)<br>• SKU (unique)<br>• Name<br>• Description<br>• UnitPrice<br>• StockQty<br>• Status</td>
</tr>
<tr>
<td><strong>Category</strong></td>
<td>Organises products, supports nested categories.</td>
<td>• CategoryID (PK)<br>• Name<br>• ParentCategoryID (recursive, nullable)</td>
</tr>
<tr>
<td><strong>Order</strong></td>
<td>A customer purchase event</td>
<td>• OrderID (PK)<br>• OrderDate<br>• Status<br>• TotalAmount<br>• CustomerID (FK)</td>
</tr>
<tr>
<td><strong>OrderItem</strong></td>
<td>Links an Order to the Products purchased, with line-level details.</td>
<td>• OrderID (FK, part of PK)<br>• LineNo (part of PK)<br>• ProductID (FK)<br><br>Descriptive attributes: Quantity, UnitPriceAtOrder, LineSubtotal.<br><br>Why weak? It has no meaning without its owning Order; identified by (OrderID, LineNo).</td>
</tr>
<tr>
<td><strong>Payment</strong></td>
<td>Records how an order was paid.</td>
<td>• PaymentID (PK)<br>• OrderID (FK)<br>• Amount<br>• PaidAt<br>• Status<br>• MethodType</td>
</tr>
<tr>
<td><strong>SupportTicket</strong></td>
<td>Customer issues linked to orders/products.</td>
<td>• TicketID (PK)<br>• OpenedAt<br>• Status<br>• Priority<br>• CustomerID (FK)<br>• OrderID (FK, nullable)<br>• AssignedTo (FK → Employee, nullable)<br>• Subject</td>
</tr>
<tr>
<td><strong>Employee</strong></td>
<td>Admin/support staff</td>
<td>• EmployeeID (PK)<br>• FullName<br>• Email (unique)<br>• Role<br>• ManagerID (recursive, nullable).</td>
</tr>
</tbody>
</table>

### 2.4 Relationships Among Objects

| Relationship                                   | Cardinality               | Participation                                         | Attributes                 | Rationale                                                                   |
| ---------------------------------------------- | ------------------------- | ----------------------------------------------------- | -------------------------- | --------------------------------------------------------------------------- |
| Customer places Order                          | 1 ⟶ 0..*                 | Customer = partial, Order = total                     |                            | Every order must belong to a customer; not every customer places an order.  |
| Order contains Product (via OrderItem – weak) | M:N resolved by OrderItem | OrderItem = total, Order = partial, Product = partial | Quantity, UnitPriceAtOrder | OrderItem depends on Order; records product quantities at time of purchase. |
| Product belongs to Category                    | Many ⟶ 0..1              | Product = partial, Category = partial                 |                            | Products may or may not be categorised; categories may be empty.            |
| Category parent-of Category (recursive)        | 0..1 ⟶ 0..*              | Parent = partial, Child = partial                     |                            | Supports multi-level catalogue; prevents cycles.                            |
| Order paid by Payment                          | 1 ⟶ 1..*                 | Order = partial, Payment = total                      | Amount, PaidAt             | Orders can exist before payment; every payment must belong to an order.     |
| SupportTicket opened by Customer               | 0..* ⟶ 1                 | Customer = partial, Ticket = total                    |                            | Every ticket must belong to a customer; some customers never open tickets.  |
| SupportTicket refers to Order                  | 0..* ⟶ 0..1              | Both partial                                          |                            | A ticket may or may not be linked to an order.                              |
| SupportTicket assigned to Employee             | 0..* ⟶ 0..1              | Both partial                                          | AssignedAt                 | Tickets may be unassigned; some employees may have none.                    |
| Employee reports to Employee (recursive)       | 0..1 ⟶ 0..*              | Both partial                                          |                            | Models reporting lines; not all employees are managers.                     |

### 2.5 Functional Requirements (Transactions, Operations, Queries)

### 2.6 Assumptions, Constraints, and Dependencies

## 3. Conceptual Data Model

### 3.1 Rationale for Chosen Modelling Approach

### 3.2 Entity Definitions and Attributes

### 3.3 Relationships and Cardinalities

### 3.4 Constraints and Business Rules (conceptual level)

### 3.5 Advanced Data Modelling Features

- **Multivalued Attributes**
- **Recursive Relationships**
- **Subtype/Supertype Structures**
- **Weak Entities**

### 3.6 Conceptual ER Diagram (annotated)

## 4. Logical Data Model

### 4.1 Mapping Process from Conceptual to Logical

### 4.2 Relational Schema (Tables, Primary Keys, Foreign Keys)

### 4.3 Normalization and Justification (1NF → 3NF or BCNF if applicable)

### 4.4 Integrity Constraints (domain, entity, referential)

### 4.5 Logical ER Diagram / Relational Diagram

### 4.6 Example SQL Definitions (CREATE TABLE statements with constraints)

## 5. Conclusion

### 5.1 Summary of Design Outcomes

### 5.2 Limitations of the Current Design

### 5.3 Next Steps Toward Implementation and Testing

## Acronyms and Abbreviations

| Acronym / Abbreviation | Meaning                  |
| ---------------------- | ------------------------ |
| DDD                    | Database Design Document |

## Appendices

- **A. Sample Queries and Transactions**
- **B. Glossary of Technical Terms**
- **C. Full ERD and Schema Diagrams (large-format versions)**
