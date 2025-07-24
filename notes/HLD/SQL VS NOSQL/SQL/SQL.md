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

Example: so ram is transferring money to Shyam account so money should get deducted from ram account and get credited to Shyam account as a single operation so if at any movement any transaction fails (due to Network Failure/system crash) so the entire transaction should get rollback.

C -> Consistency: Read should fetch upto date data and write shouldn't violate integrity constraints.
1. Read operation retrieve consistent and up-to-date data from the database.
2. Write operation ensures that data modification maintain the database constraints.(such as
    foreign key relationships or unique constraints so that data remain accurate).
    
Example: Suppose you have ₹X in your account and Ram has ₹R. At the same time, you transfer ₹Y to Ram, and Ram transfers ₹Z to you. Even though these transactions happen simultaneously, the total amount of money in both accounts should remain the same.
    
I -> Isolation: One transaction should be independent from others.
3. it ensures that if there are two transaction 1 and 2, then the changes made by transaction 1
   are not visible to transaction 2 until transaction 1 is commit.

Example:  Transaction 1 update value A to 50 (previously A value was 40)
         Transaction 2 read/get/fetch value A
         if Transaction 1 is committed according to Transaction 2 the value of A = 50
         if Transaction 1 is pending/running according to Transaction 2 the value of A = 40
         which is shown as data inconsistency so we should always wait for Transaction 1 to complete first for data consistency.
         
D -> Durability: The committed transaction should remain even after a failure/crash.

Example: if ram is transferring X money to Shyam account so we will check balance and
deduct the money from ram account and suppose our data is in committed phase and system got crash or encounter network failure. so when the system backup both ram and Shyam account will reflected with updated balance.

-------------

**Transactions**

Transaction is a unit of work that consist one or more database operation (read/write/commit/rollback) and read and write in transaction follows the ACID property.

![transaction-1](../../../images/transaction-1.png)
So here we are making an order in swiggy but if we get network failure so if any money got deducted it will get rollback to our account otherwise if no issues has been encounter than transaction will be successful.
here how things are working is for making an swiggy order it has to cut some money from my account create an order etc. so each operation is an transaction so every transaction will get stored in transaction log and if everything got success than it will go to commit phase and from their data will get saved into database.

Example :
![Transaction-2](../../../images/transaction-2.png)

**Concurrency Control**
Concurrency ensures that multiple transaction can run concurrently without compromising data consistency.

Example:
1. Ram is giving 100rs to Shyam.
2. Shyam is giving 50rs to ram.
So both are different transaction T1 and T2 and it is trying to modify the same value so T1 should be independent for T2 and data should be consistent.

Some technique used here is:
1. Locking (Pessimistic, Optimistic)
2. Two-Phase Locking
3. Timestamp ordering

**Pessimistic Locking**

At given point of time one thread is executing critical section while others are waiting. so it affects the through put like threads or process are ready to execute but waiting for locks. It is not for distributed system.

So Basically in this locking at starting only the lock will be acquire by one threads and until this finishes the set of operation other threads or process who are ready to execute they have to wait to get the lock released.

**Optimistic Locking**

if two threads tried to do same operation at the same time then one succeeds and other fails. It is for distributed system.

it works on comapare_and_swap  which is atomic in nature so I will take an example like their are two transaction which are going to update one value t1 -> count(10, 11) and t2 -> count(10, 15)
so here what will happen that one will get success and one will fail because optimistic locking first compare with old value when it acquires if while updating the old value is same and if it change it will fail the transaction so suppose t2 got failed we can do anything with that either retry or either fail or throw the error.

Example:
here I am considering 3rd column as version suppose same here 2 transaction is their t1 wants to update so initial t1 updates the data and version is v1 and after updating the version will be v2 and now suppose t1 and t2 parallelly trying to update the data than while updating data if version got changed suppose t1 changed the data to v3 but t2 was expecting version to be v2 in that case t2 will get fail. so after that we can do anything with that either retry or either fail or throw the error.
![optimistic](../../../images/optimistic.png)

if there is a article and 500 people are trying to update than it will be not good idea to use optimistic lock because lots of version mismatch will fail.

---------------

**Isolation Level**

There are two transaction t1 and t2 and it should be isolated from each other.

In system where multiple transactions are executed concurrently, isolation levels manage the extent to which the operations of one transactions are isolated from those of other transaction.

**These are anomalies of isolation**

Dirty Read
Reading data written by a transaction that has not yet committed. consider it T1 and T2 are two transaction so suppose t1 is doing set of operations which includes multiple write and read operation and same time t2 comes and read the value which will get updated value but at some point t1 fails and rollback than t2 has read the wrong value which is an data anomaly.
![Dirty Read](../../../images/dirty-read.png)


Non Repeatable Read

![Non Repeatable](../../../images/non-repeat.png)

Example:
Consider again we are doing two transaction t1 and t2 and t1 has two read operation and does some write operation in different table and t2 has read write operation in same table which t1 is using and now what happened t1 reads the value as 10 and after that it got stop for some reason and mean time t2 updated the data from 10 to 20 and again when t1 reads the data so it will get data as 20 but it started operation on 10  so that is another anomaly.

Phantom Read

![phantom](../../../images/phantom.png)
Example: suppose their is one transaction t1 which fetch the records does some operation and again fetch some records but while t1 is performing the operation in same time t2 came and insert one new row in the table so now when t1 reads the same  table data it gets 3 column instead of 2 because new row inserted that is another anomaly.

**Type of Isolation level**

Read Uncommitted
![read uncommitted](../../../images/read-uncommitted.png)
As name suggests it says data is uncommitted but t1 can read the data of t2 transaction so it does not resolve any anomalies.

Read Committed
![read committed](../../../images/read-committed.png)

so it clearly says t1 will be able to read the data when t2 commits its data so it solves only dirty read problem.

Read Repeatable
![repeat read](../../../images/repeat-read.png)

it clearly says t1 is reading the value and doing some operation so t2 cannot come and do not insert any new record which solves the phantom problem.

Serializable
![serial](../../../images/serial.png)
here no anomaly will be present






