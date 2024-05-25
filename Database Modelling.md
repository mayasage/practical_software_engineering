# Database Modelling

It is MANDATORY to organize EVERYTHING.

- [Database Modelling](#database-modelling)
  - [Design Methods](#design-methods)
  - [Top-down Approach](#top-down-approach)
    - [CDM (Conceptual Data Model)](#cdm-conceptual-data-model)
      - [Five Questions of CDM](#five-questions-of-cdm)
      - [Relational or Dimensional?](#relational-or-dimensional)
      - [Identify \& Define Concepts](#identify--define-concepts)
        - [Identify \& Define Concepts: For Relational](#identify--define-concepts-for-relational)
        - [Identify \& Define Concepts: For Dimensional](#identify--define-concepts-for-dimensional)
      - [Capture Relationships](#capture-relationships)
        - [Capture Relationships: For Relational](#capture-relationships-for-relational)
        - [Capture Relationships: For Dimensional](#capture-relationships-for-dimensional)
      - [Create diagrams that validators will look at](#create-diagrams-that-validators-will-look-at)
    - [LDM (Logical Data Model)](#ldm-logical-data-model)
    - [PDM (Physical Data Model)](#pdm-physical-data-model)
  - [Atomic Concepts](#atomic-concepts)
    - [Column Naming](#column-naming)
    - [Normalization](#normalization)
      - [First Normal Form (1NF)](#first-normal-form-1nf)
      - [Second Normal Form (2NF)](#second-normal-form-2nf)
      - [Third Normal Form (3NF)](#third-normal-form-3nf)
      - [Abstraction](#abstraction)

## Design Methods

There are only two ways to design:

- Top-down / Forward Engineering:
  - When you're building something new.
  - Flow: CDM -> LDM -> PDM -> Mongodb

- Bottom-up / Reverse Engineering:
  - When you have a shitty pre-built project.
  - Flow: Mongodb -> PDM -> LDM -> CDM

## Top-down Approach

### CDM (Conceptual Data Model)

#### Five Questions of CDM

1. What's the App going to do?
    1. Answer in three sentences.

2. "as is" or "to be"?
    1. Understand & model existing system?
    2. Or, build a new system?
    3. Or, is it both?

3. Do you need analytics?
    1. Analytics means playing with numbers in short.
    2. If yes, you need dimensional modeling (answer business questions).
    3. If no, you need relational modeling (enforce business rules).

4. Who's the audience?
    1. Present model in a way the audience can understand & validate.

5. Flexibility or Simplicity?
    1. You need a balance.
    2. **Event** instead of **Order**?
    3. **Person** instead of **Employee**?
    4. This is going in that Abstraction category I hate.

#### Relational or Dimensional?

Imagine this as branching paths.  
You've found an exit if you don't "Go."

1. Is the system Operational or Reporting?
    1. Operational imply "automating a business process."
    2. Reporting imply "using operational data to evaluate performance".
    3. **Go** Relational for Operational because Relational enforce business
       rules.

2. You need Reporting. But do you need to see any Text (name, address, etc...)?
    1. IF Yes, **Go** Relational.

3. You need Reporting.
   You don't need to see any Text, ONLY numbers.
   Then are those numbers read-only or can they be played with?
    1. IF read-only, **Go** Relational.

4. **Go** Dimensional.
    1. **Gross Sales Amount** at month level is Relational.
    2. But drilling it down to the Date level is Dimensional.

Conclusion:

Build Dimensional model ONLY when you need to analyze numbers.  
For everything else, we go Relational.

#### Identify & Define Concepts

##### Identify & Define Concepts: For Relational

This is a "Concept Template."

- Who?
  - Who's important to business?
  - Employee, Patient, Player, Suspect

- What?
  - What's important to business?
  - What keeps business running?
  - Product or Service of business.
  - Product, Service, Raw Materials

- When?
  - When is the business in operation?
  - Time, Month, Date

- Where?
  - Where is business conducted?
  - Mail Address, IP Address

- Why?
  - Why is business in business?
  - Business Events
  - Order, Return, Complaint

- How?
  - How does the business keep track of Events?
  - Invoice, Purchase Order, Sale Order

You need to get answers to all these Questions.

##### Identify & Define Concepts: For Dimensional

1. Define business questions.
    1. Show me the number of students receiving financial aid by department and
       semester for the last five years.
       (From Financial Aid Office)

#### Capture Relationships

##### Capture Relationships: For Relational

Determine how entities relate to each other & articulate the rules.

There should be NO granular data here, like A/C Open Date, A/C Balance.  
Granular data isn't conceptual, it's logical, and therefore should appear in
LDM.

Answer these **eight** Questions to determine relationships between two
entities:

- 4 Crow's notation questions:

  - 2 participation questions:
    - One Entity A related to more than one Entity B?
    - One Entity B related to more than one Entity A?

  - 2 optionality questions:
    - Entity A exists without Entity B?
    - Entity B exists without Entity A?

- 2 "should you show" sub-typing questions for each Entity:

  - Is there any sub-typing relationship example you should show?
    - **Person** can be **Teacher** or **Student**

  - Does Entity go through a lifecycle?
    - **Order** is **New Order** -> **Processed Order** -> **Shipped Order**

##### Capture Relationships: For Dimensional

Create a "grain matrix".

It's a spreadsheet where:

- Columns:
  - are "what do you need to find" in a business question
  - represent Dimensions
  - Each Dimension should be defined by a DB Query.

- Rows:
  - are "granularity... how deep you need to go to find it"
  - represent Granularity

Example:

- Business question:
  - Show me the number of students receiving financial aid by department and
      semester for the last five years.
    (From Financial Aid Office)

- Column:
  - Student Count

- Rows:
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

#### Create diagrams that validators will look at

Repeat your "validation-feedback-update" loops till both you & the validators
reach a conclusion.

### LDM (Logical Data Model)

### PDM (Physical Data Model)

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
