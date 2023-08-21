# mira
A DSL for "intermediate" representation of data models.


# About
This repository is intended as a specification for a domain-specific language with which data can be modeled. The syntax is intended to be intuitive and sufficient to translate between data models in different languages/frameworks.

Here's an example
```
STRING = VARCHAR(255)
DECIMAL = DECIMAL(10, 2)

BaseEntity (id SERIAL)

Person < BaseEntity
    first_name, last_name STRING
    date_of_birth DATE
    email* STRING
    address TEXT
    
    FriendsWith
        OTHER Person (0..*)
    
    WorksAt
        MANY Company (0..1)
    
Company < BaseEntity
    name STRING
    industry STRING
    established_year INT
    headquarters STRING
    
    Employees
        MANY Employee (0..*)
    
    PartnersWith
        MANY Company (0..*) VIA CompanyPartners (id=partner_id)
    
Employee < BaseEntity
    first_name, last_name STRING
    position STRING
    salary# DECIMAL # (>= 0)
    start_date, end_date DATE
    
    BelongsTo
        ONE Company
    
    Manages
        MANY Employee (0..*) FK_Employee_Manages > Employee(id)
```

In this example we can see support for:
* Custom data type definitions ```TYPE String = VARCHAR(255)```
* Inheritance ```Person < BaseEntity```
* Relationships via the `ONE` and `MANY` key words
* Explicit definition of Many-to-Many join tables using the optional `VIA` syntax
* Nullable `?` and unique `*` operators
* Foreign key relationships `FK_Employee_Manages > `Employee(id)`
* Primary key fields are specified in parentheses alongside the model
