**Definition**

A relational database stores data in tables made of rows and columns, and uses keys to relate different tables. It allows easy access and management using SQL.

-------------

**Normalisation**

**Normalisation** is the process of organising data in a database to **reduce redundancy (duplicate data)** and **improve consistency**.  
It involves **splitting a large table into two or more smaller related tables**.

 **Insertion Anomaly**

An **insertion anomaly** occurs when it's **difficult to add data** into a database because **some required data is missing**.

**Example:**  
1. You want to add a **new department**, but can't do it unless there's at least **one employee** in that department.
2. Suppose a student taking an admission in 6th, But we don't know which section it is so while inserting data, there is an absence of data. 

**Deletion Anomaly**

A **deletion anomaly** happens when **removing a record** also deletes **important related data**.

**Example:**  
If you delete all employee records, you might also **lose information about departments**, their **managers**, and **salaries** — even though that data is still needed.

**Update Anomaly**

An **update anomaly** happens when **changing a value** requires **multiple updates in the table**.

**Example:**  
If you want to **increase the salary** for all employees in the **HR department**, you must update the value in **every HR employee's record**. If you miss one, the data becomes **inconsistent**.
![Anomaly](../../../images/anomaly.png)

**1NF**

Every column need to have single value.
Every row should be unique. either through a single column table or multiple column table

In this image 2 values are coming in same column this should not happen.

![Before 1NF](../../../images/before-1nf.png)

So here we have make an composite key of two column to uniquely identify the rows

![After 1NF](../../../images/after-1nf.png)

**2NF**

1. Must be in 1NF
    
2. All non-key attributes must be fully dependent on candidate key.  
        _i.e., If a non-key column is partially dependent on candidate key (subset of columns forming candidate key) then split them into separate tables._
    
3. Every table should have a primary key and relationship between the tables should be formed using foreign key.

**Candidate Key:**

- Set of columns which uniquely identify a record.
    
- A table can have multiple candidate keys because there can be multiple sets of columns which uniquely identify a record/row in a table.

**Non-key Columns:**

- Columns which are not part of the candidate key or primary key.

**Partial Dependency:**

- If your candidate key is a combination of 2 columns (or multiple columns), then every non-key column (columns which are not part of the candidate key) should be fully dependent on all the columns of the key.
    
- If there is any non-key column which depends only on one of the candidate key columns, then this results in partial dependency.

Example:
So here I have table in that we are considering candidate key as (ORDER_NUMBER, PRODUCT_CODE) but like we have few non key columns like(Quantity, ITEM_PRICE, TOTAL_COST, ORDER_DATE, STATUS) so these columns are only depends upon ORDER_NUMBER so we can separate this into two different tables.
Similarly (PRODUCT_NAME, PRODUCT_PRICE) is depend upon PRODUCT_CODE so again
we can separate it out these tables also.

![Before 2NF](../../../images/before-2NF.png)

After separating out the table it looks like these so here we have introduce the 3 new columns
ORDER_ID, PRODUCT_CODE(removed duplicate), CUSTOMER_ID.
![After 2NF](../../../images/after-2NF.png)

Now following the 2nd point we have to make the relationship between them.

![Relation 2NF](../../../images/relation-2nf.png)


**3NF**
1. Avoid Transitive dependencies.

So Transitive dependencies is lets say we three column A, B, C. If A is a functionally dependent on B and B is functionally dependent on C than we can say A is functionally dependent on C.
Example : We have one column CUSTOMER_NAME(A) which is dependent on ORDER_ID(B) and we have another column SALES_PERSON_NAME(C) which is dependent on ORDER_ID(B) so in a way if we know column CUSTOMER_NAME we can get the ORDER_ID and we know ORDER_ID than we can get the data of  SALES_PERSON_NAME. so this is called transitive dependencies.

So we will separate out SALES_DETAILS and remove the duplicates and create a relationship.

![Relation 3NF](../../../images/3nf.png)

-------------

**ACID Properties**

It ensures that transaction are processed reliably and accurately.

A -> Atomicity: It ensures a single unit of work Either execute all operations (commit) or none of them are applied (rollback).
Example :- so ram is transferring money to Shyam account so money should get deducted from ram account and get credited to Shyam account as a single operation so if at any movement any transaction fails (due to Network Failure/system crash) so the entire transaction should get rollback.
C -> Consistency: Read should fetch upto date data and write shouldn't violate integrity constraints.
I -> Isolation: One transaction should be independent from others.
D -> Durability: The committed transaction should remain even after a failure/crash.

1. 




