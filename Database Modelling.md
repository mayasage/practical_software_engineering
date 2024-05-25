---
aliases:
---
# Database Modelling

It is MANDATORY to organize EVERYTHING.

## Table Of Contents

- [Database Modelling](#database-modelling)
  - [Table Of Contents](#table-of-contents)
  - [Design Methods](#design-methods)
  - [CDM (Conceptual Data Model)](#cdm-conceptual-data-model)
    - [Five Questions of CDM](#five-questions-of-cdm)
    - [Relational or Dimensional?](#relational-or-dimensional)
    - [Identify \& Define Concepts](#identify--define-concepts)
      - [Identify \& Define Concepts: For Relational](#identify--define-concepts-for-relational)
      - [Identify \& Define Concepts: For Dimensional](#identify--define-concepts-for-dimensional)
    - [Capture Relationships](#capture-relationships)
      - [Capture Relationships: For Relational](#capture-relationships-for-relational)
      - [Capture Relationships: For Dimensional](#capture-relationships-for-dimensional)
  - [Atomic Concepts](#atomic-concepts)
    - [Column Naming](#column-naming)
    - [Normalization](#normalization)
      - [First Normal Form (1NF)](#first-normal-form-1nf)
      - [Second Normal Form (2NF)](#second-normal-form-2nf)
      - [Third Normal Form (3NF)](#third-normal-form-3nf)
      - [Abstraction](#abstraction)
    - [Embed or Reference ?](#embed-or-reference-)
    - [Ways of Saving Data](#ways-of-saving-data)
    - [Partitioning](#partitioning)
    - [Recursive Relationships](#recursive-relationships)
    - [Same column in multiple Entities](#same-column-in-multiple-entities)

## Design Methods

There are only two ways to design:

- (Top-down / Forward Engineering) to make a new project.
- (Bottom-up / Reverse Engineering) to understand old projects.

Top-down flow:

1. CDM (Conceptual Data Model)
2. LDM (Logical Data Model)
3. PDM (Physical Data Model)
4. Mongodb

## CDM (Conceptual Data Model)

### Five Questions of CDM

1. Answer in 3 sentences what your application will do.
2. Write if you must understand existing system.
3. Write if you need analytics.
4. Balance Flexibility with Simplicity (**Person** is more flexible than **Employee**).
5. Make diagrams that your audience can validate.

### Relational or Dimensional?

- Dimensional is for data warehouses.
  - It's for drilling down on numbers.
  - **Gross Sales Amount** per day

- Relational is what most developers do.
  - **Gross Sales Amount** per month

I only care about Relational.

### Identify & Define Concepts

#### Identify & Define Concepts: For Relational

This is a "Concept Template."

- Who's important to business? (Employee, Patient, Player, Suspect)
- What keeps business running? (Product or Service of business.)
- When is the business in operation? (Time, Month, Date)
- Where is business conducted? (Mail Address, IP Address)
- Why is business in business? (Business Events: Order, Return, Complaint)
- How does the business keep track of Events? (Invoice, Purchase Order, Sale Order)

You need to define Entities for all these questions.

#### Identify & Define Concepts: For Dimensional

Define business questions.

Example:

Show me the number of students receiving financial aid by department and semester for the last five years. (From Financial Aid Office).

Concepts:

- Student
- Department
- Semester
- Year
- Financial Indicator

### Capture Relationships

#### Capture Relationships: For Relational

NO granular data here ❌.  
Ex: A/C Open Date, A/C Balance.  
It's not conceptual; it's logical (LDM).

Answer these **eight** Questions to determine relationships between two
entities:

- 4 Crow's notation questions:
  - 2 for participation:
    - 1 Entity A related to many Entity B?
    - 1 Entity B related to many Entity A?
  - 2 for optionality:
    - Entity A exists without Entity B?
    - Entity B exists without Entity A?
- 2 each for "should you show subtypes":
  - Any example you want to show? (**Person** can be **Teacher** or **Student**)
  - Does Entity go through a lifecycle? (**Processed Order** -> **Shipped Order**)

#### Capture Relationships: For Dimensional

Create a "grain matrix" from business questions.

It's a spreadsheet where:

- Columns:
  - are Dimensions (what do you need to find)
    - Each Dimension should be defined by a DB Query.
- Rows:
  - is Granularity (how deep you need to drill down to find the answer).

Example:

Business question:  
Show me the number of students receiving financial aid by department and semester for the last five years. (From Financial Aid Office)

Column: Student Count

Rows:

- Department
- Semester
- Year
- Financial Aid Indicator

Grain matrix:

|                         | Student Count |
|:-----------------------:|:-------------:|
|       Department        |    1, 2, 3    |
|        Semester         |    1, 2, 3    |
|          Year           |  1, 2, 3, 4   |
| Financial Aid Indicator |       1       |

## Atomic Concepts

### Column Naming

Pre-define "classwords," and append your column names with them.  
Always write full column names: id ❌ company_id ✔

| S.No. | Class Word |              Covers              |
|:-----:|:----------:|:--------------------------------:|
|   1   |    Name    |               Noun               |
|   2   |    Text    |              String              |
|   3   |   Amount   |          Monetary Value          |
|   4   |    Date    |              (¬_¬ )              |
|   5   |    Code    |  Short form of some information  |
|   6   |  Quantity  |       Non-monetary amount        |
|   7   |   Number   | NOT math number ❌ It's Phone No. |
|   8   | Identifier |              (¬_¬ )              |
|   9   | Indicator  |             Boolean              |
|  10   |    Rate    |              Ratio               |
|  11   |  Percent   |              (¬_¬ )              |
|  12   |  Complex   |     NOTA. It's PDF, MP4, MP3     |

Examples:

1. Country Code
2. Gross Sales ❌ Gross Sales Amount ✔
3. Free Checks Per Month (Quantity)
4. Credit Rating (Rate)

Pointers:

1. Divide EVERY column into one of these categories.

### Normalization

#### First Normal Form (1NF)

An Employee can't have more than One Phone Number ❌

|     Before     |           After           |
|:--------------:|:-------------------------:|
| Phone Number 1 |   Employee Phone Number   |
| Phone Number 2 |  Department Phone Number  |
| Phone Number 3 | Organization Phone Number |
|       ❌        |             ✔             |

#### Second Normal Form (2NF)

Break tables on the basis of relationships.

| employee_id | employee_name | job_code | job_name |
|:-----------:|---------------|:--------:|:--------:|
|      1      | Alice         |    1     | Waitress |
|      1      | Alice         |    2     |   Chef   |

Employee has a many-to-many relationship with Job.  
So break it down to three tables.

*employees:*

| employee_id | employee_name |
|:-----------:|:-------------:|
|      1      |     Alice     |

*jobs:*

| job_id | job_name |
|:------:|:--------:|
|   1    | Waitress |
|   2    |   Chef   |

*employee_jobs:*

| employee_id | job_id |
|:-----------:|:------:|
|      1      |   1    |
|      1      |   2    |

#### Third Normal Form (3NF)

Remove all derived attributes, duplicity, and transitive dependencies.

| employee_id | employee_name | supervisor_id | supervisor_name |
|:-----------:|:-------------:|:-------------:|:---------------:|
|      1      |     Alice     |       1       |      Malik      |

❌ NOT in 3NF.  
Because "supervisor_name" is transitively dependent on "employee_id."

Break the table down based on relationship.  
Employee has one-to-one relationship with Supervisor.

*employees:*

| employee_id | employee_name | supervisor_id |
|:-----------:|:-------------:|:-------------:|
|      1      |     Alice     |       1       |

*supervisors:*

| supervisor_id | supervisor_name |
|:-------------:|:---------------:|
|       1       |      Malik      |

- Remember that DB doesn't understand relationships.
  It only understands Foreign Keys.
- ORMs use relationships to query & organize data.

#### Abstraction

I refuse to use this 😠.  
I hate this idea.  
DB should be absolutely clear without any IFs & BUTs.

Employee becomes a Party.  
Each Party has a Role, which can be an Employee or a Tennis Player.

Problems:

- Unclear, vague, & irritating:
  - Go into the code and understand IFs & BUTs.
  - Example: ONLY Role Employee must contain "start_date."

Benefits:

- Flexibility: (Counter) Mongodb doesn't even require this, as it's inherently
  flexible.

### Embed or Reference ?

Embed when:

- Data is frequently queried together.
- Child is a weak entity (can't exist without parent).
- There is a one-to-one relationship.

Reference when:

- There is one-to-many relationship.
- There is a many-to-many relationship.

### Ways of Saving Data

- Type 0: Don't save anything
- Type 1: Overwrite with changes
- Type 2: Destroy old, add new
- Type 3: Github

### Partitioning

- Vertical: when you have too many fields
- Horizontal: when you have too many documents

### Recursive Relationships

When you're unaware of a relationship, you can use this to get out of it.
ONLY use as a last resort.

Example:

Employee has a many-to-many relationship with Employee.  
An Employee can be managed by one or more Employee.  
An Employee can manage one or more Employee.

You can imagine how obscure this can get.

### Same column in multiple Entities

The Problem:

1. Same column is stored multiple times. (Data Redundancy)
2. Update, Insert, Delete in multiple tables. (Maintenance Hell)
3. RIP Normalization

Solution:

1. Create a Supertype and move common columns into it.

This is good.  
But there's another problem.  
This type of relationship can't be stored in DB.  
For this, we have a technique called "Identity, Rolldown & Rollup."

Example:

I have 3 Entities:

- Author
- Book
- BookVersion

Relationship:

- Author has one-to-many relationship with Book.
- Author has one-to-many relationship with BookVersion.

authorId & title properties are common in Book & BookVersion.

Super-typing:

1. Move these 2 common properties to a supertype Entity **Title**.
2. Author has many Title.
3. A Title can be a Book.
4. A Title can be a BookVersion.

Again, this relationship can't be implemented in DB.

Let's use our technique.

1. Identity:

   - Title is optionally Book.
   - Title is optionally BookVersion.
   - Optional imply 1 Entity A has one or zero Entity B.

2. Rolldown:

   - Copy all properties of Title in every Subtype.
   - Delete Title.

3. Rollup:

   - Merge all the fields of all subtypes into a single Table.
   - Name that table Title.
