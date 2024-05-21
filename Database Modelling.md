It is MANDATORY to organize EVERYTHING.

## Column Naming

Pre-define "classwords", and append your column names with them.
And always write full column names: id ❌ company_id ✔

| S.No. | Class Word | Covers                           |
| ----- | ---------- | -------------------------------- |
| 1     | Name       | Noun                             |
| 2     | Text       | String                           |
| 3     | Amount     | Monetary Value                   |
| 4     | Date       | (¬_¬ )                           |
| 5     | Code       | Short form of some information   |
| 6     | Quantity   | Non-monetary amount              |
| 7     | Number     | NOT math number ❌ It's Phone No. |
| 8     | Identifier | (¬_¬ )                           |
| 9     | Indicator  | Boolean                          |
| 10    | Rate       | Ratio                            |
| 11    | Percent    | (¬_¬ )                           |
| 12    | Complex    | NOTA. It's PDF, MP4, MP3         |

Examples:

1. Country Code
2. Gross Sales ❌ Gross Sales Amount ✔
3. Free Checks Per Month (Quantity)
4. Credit Rating (Rate)

Pointers:

1. Mentally divide the column into one of the categories.
2. Is the column name justified for this category ?

## Normalization

### First Normal Form (1NF)

An Employee can't have more than 1 Phone Number ❌

| Before         | After                     |
| -------------- | ------------------------- |
| Phone Number 1 | Employee Phone Number     |
| Phone Number 2 | Department Phone Number   |
| Phone Number 3 | Organization Phone Number |
| ❌              | ✔                         |

### Second Normal Form (2NF)

Break tables on the basis of relationships.

| employee_id | employee_name | job_code | job_name |
| ----------- | ------------- | -------- | -------- |
| 1           | Alice         | 1        | Waitress |
| 1           | Alice         | 2        | Chef     |

Employee has a many-to-many relationship with Job.
So break it down to 3 tables.

*employees:*

| employee_id | employee_name |
| ----------- | ------------- |
| 1           | Alice         |

*jobs:*

| job_id | job_name |
| ------ | -------- |
| 1      | Waitress |
| 2      | Chef     |

*employee_jobs:*

| employee_id | job_id |
| ----------- | ------ |
| 1           | 1      |
| 1           | 2      |

### Third Normal Form (3NF)

Remove all derived attributes, duplicity, transitive dependencies.

| employee_id | employee_name | supervisor_id | supervisor_name |
| ----------- | ------------- | ------------- | --------------- |
| 1           | Alice         | 1             | Malik           |

❌ NOT in 3NF.
 Because "supervisor_name" is a transitively dependent on "employee_id".

Break the table down based on relationship.
Employee has one-to-one relationship with Supervisor.

*employees:*

| employee_id | employee_name | supervisor_id |
| ----------- | ------------- | ------------- |
| 1           | Alice         | 1             |

*supervisors:*

| supervisor_id | supervisor_name |
| ------------- | --------------- |
| 1             | Malik           |

- Remember that DB doesn't understand relationships. It only understand Foreign Keys.
- ORMs use relationships to query & organize data.

### Abstraction

I refuse to use this 😠.
 I hate this idea.
 DB should be absolutely clear without any IFs & BUTs.

Employee becomes a Party.
 Each Party has a Role. It can be an Employee or a Tennis Player.

Problems:

- Unclear, vague, & irritating:
	- Go into the code, and understand IFs & BUTs.
	- Example: ONLY Role Employee must contain "start_date".

Benefits:

- Flexibility: Mongo doesn't even require this, as it's inherently flexible.

