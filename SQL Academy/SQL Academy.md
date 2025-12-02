# AS Watson SQL Academy

![AS Watson Logo](images/asw_logo_blank_background.png)

## Table of Contents


[1 Getting Started](#1-getting-started)
- [1.1 Databricks SQL](#11-databricks-sql) 
- [1.2 Creating a Workbook](#12-opening-and-saving-a-file)
- [1.3 SQL Language & Database Concepts](#13-sql-language--database-concepts)
- [1.4 Multi-Country Server](#14-multi-country-server)
- [1.5 AS Watson Data Model Structure and Data Tables](#15-as-watson-data-model-structure-and-data-tables)

[2 SQL Basics](#2-sql-basics)
- [2.1 Basic SQL Querying Concepts](#21-basic-sql-querying-concepts)
    - [2.1.1 Select Statements](#211-select-statements)
    - [2.1.2 Where, And & Setting Conditions](#212-where-and--setting-conditions)
    - [2.1.3 Aggregate Functions](#2313-aggregate-functions)
    - [2.1.4 Like Clause](#214-like-clause)
    - [2.1.5 Distinct Clause](#215-distinct-clause)
    - [2.1.6 Joining Tables](#215-joining-tables)
    - [2.1.7 Case When](#217-case-when)
- [2.2 B_TRANSACTION Table](#22-b_transaction-table)
- [2.3 Main KPIs](#23-main-kpis)
- [2.4 Time Periods](#24-main-time-periods)
- [2.5 Examples](#25-examples)
- [2.6 B_PRODUCT Table](#26-b_product-table)
- [2.7 Final Note](#27-recap-and-final-note)

[3 Exercises](#4-exercises)
- [3.1 Warm Up Questions - B_TRANSACTION only](#41-warm-up-questions---b_transaction-only)
- [3.2 Exercises 1 - B_TRANSACTION](#42-exercises-1---b_transaction)
- [3.3 Exercises 2 - B_STORE and B_TIME tables](#43-exercises-2---b_store-and-b_time-tables)
- [3.4 Exercises 3 - B_CONTACT and More Complex Queries](#44-exercises-3---b_contact-b_member-and-more-complex-queries)
- [3.5 Exercises 4 - Segmentation Tables and More on Tables](#45-exercises-4---segmentation-tables-and-more-on-tables)
- [3.6 Exercises 5 - Age Calculations](#46-exercises-5---age-calculations)


## 1 Getting Started 
[Back to Top](#as-watson-sql-academy)

### 1.1 Databricks SQL
- AS Watson DataLab uses Databricks and Oracle SQL. You will be using Databricks.
- Internally, we call Databricks 'EDP' (Enterprise Data Platform), and we call Oracle 'ADM' (Application Dependency Management). 
- _You don't need to remember why these are called what - just remember that you will be developing in the software we call EDP_.
- There are other versions, including SQL Server, PostgreSQL, MySQL, SQLite etc. but we do not use these.
- There are minor differences, but you may find that a solution you find online might use a function that is in SQL Server but not Databricks SQL or visa versa.
- When using Google to find an answer (Stack Overflow is your best friend), make sure to add Databricks SQL to get the relevant result.

### 1.2 Creating a Workbook
- Head over to <u>[Databricks](https://adb-4609280326114182.2.azuredatabricks.net/?o=4609280326114182)</u> and login using your ASW account details
- On the left hand side, click 'New', and then 'Notebook'
![Creating a Workbook Diagram](images/creating-a-workbook.png)
- In your new notebook type `%sql`, and then on a new line you can begin coding
- To run any files, your notebook must be connected. To do so, click the 'Connect' box in the top right hand corner, then click 'sd-datalab-workbench-cluster'.
![Connecting to Schema Diagram](images/connecting-to-schema.png)
- If this isn't running, start the cluster (it will take a few minutes)
- If you leave the site idle for long (around 10 mins) and don't run any queries, the schema will detatch. If this happens, just restart it.

### 1.3 SQL Language & Database Concepts

- SQL = Structured Query Language 
- Originally designed for creating and manipulating data stored in a relational database 
- Schemas are collections of database objects, namely tables, but also procedures, packages etc. 
- For example, if you want to look at ICI Paris Netherlands data, this can only be accessed in the `adb-eu-s5-edpwb-icinl-prod` schema, which can be switched to in the top right of your screen
- The `rdm` schema is constant across all BUs (business units) and holds all important data.


### 1.4 Multi-Country Server
- The MC server contains data for all the different BUs, so that you don't have to choose a single business to pull back data from
- This data is (as you can probably guess), incredibly large. Therefore, we don't use it.

### 1.5 AS Watson Data Model Structure and Data Tables
- <u>[ASW EDP Data Model](https://aswatsonuk.sharepoint.com/:x:/s/ASWGD/EW2zNVjuRkdKiC8W1a4HKZYBc3rYGRFGYJpE7Rf8qkprxw?email=Simon.Rogerson%40uk.aswatson.com&e=4%3AgyaDtA&fromShare=true&at=9&CID=99B96C9F-26EF-4F82-80BC-08CD3D0F4483&wdOrigin=TEAMS-MAGLEV.p2p_ns.rwc&wdExp=TEAMS-TREATMENT&wdhostclicktime=1755169260866&web=1)</u>
- Shows the link between the main tables in EDP and all the rules for each BU

# 2 SQL Basics

[Back to Top](#table-of-contents)

### 2.1 Basic SQL Querying Concepts

### 2.1.1 Select Statements
- Two basic ‘clauses’ for querying data from a table:
    1. tell it what you want ```select```
    2. tell it where to get it from ```from```
- Selecting everything in a specified table:
    ```sql
    select 
        *
    from 
        table_name
    ```
- Run code using CTRL + Enter
- Every query should have a ```;``` at the end
- If we wanted to select just one column:
    ```sql
    select 
        column_name
    from 
        table_name;
    ```
- You can name a column using ```as```
    ```sql
    select 
        column_name as new_name
    from 
        table_name;
    ```

### 2.1.2 Where, And & Setting Conditions
- If we want to filter a table to enter select certain features we use a where clause.
- Filter only female members e.g.:
    ```sql
    select 
        * 
    from 
        table_name
    where 
        gender = ‘F’;
    ```
- An ```and``` clause will be used if we are using more than one filter/exclusion.
- Example: filter by female and older than 25 years
    ```sql
    select 
        *
    from 
        table_name
    where 
        gender = ‘F’
        and age > 25;
    ```


### 2.1.3 Aggregate Functions
- We use built in functions to aggregate data 
- Has to be used whenever an aggregate function has been used 
- `group by` – e.g. if we wanted sales by date:
    ```sql
    select 
        transaction_date_key, 
        sum(sale_amount)
    from 
        rdm.f_transaction
    group by 
        transaction_date_key ; 
    ```
- Wide range of aggregate and analytical functions which can be used: 

    - `sum()` – sums the values on selected column 
    - `count()` – counts the # of values returned on selected column 
    - `count(distinct )` – counts the # of unique valued returned on selected column 
    - `min()` – returns the smallest value on selected column 
    - `max()` – returns the largest value on selected column 
    - `avg()` – returns the mean average of values on selected column


### 2.1.4 Like Clause
- When it is unknown what the correct format of a variable is in one of the tables (as they might differ BU to BU – e.g. brand names in the b_product table), we can use like function to help us out. 
- Note that this is typically quite inefficient, so avoid if possible.
- It is case sensitive so usually we use `upper(brand_name) like ‘%CLINIQUE%’` which converts the brand_name value to uppercase
- It is used in the where clause and is used like: 
    ```sql
    select 
        brand_name
    from 
        rdm.d_product 
    where 
        upper(brand_name) = ‘%CLINIQUE%’
    group by 
        brand_name ;
    ```

### 2.1.5 Distinct Clause
- In order to get unique values from an output, use the distinct keyword.
- Used after the `select`, this will only return unique rows.

- E.g. `select distinct contact_key from table_name`
    - Returns only unique contact keys
    - This can be used in conjunction with a counting argument to count unique shoppers
        - E.g. `select count(distinct contact_key) from table_name`


### 2.1.6 Joining Tables
- You can join tables together to get more data 
- Documentation: https://www.w3schools.com/sql/sql_join.asp
- There are a few types of joins but the terminology for joining the tables is the same
- Note that `join` is the same as `inner join`, but `inner join` was used in older versions of SQL.
- You will mostly use `join` and `left join`, but the others can occasionally be handy.

- Duplicate columns
    -   ```sql
        select 
            a.contact_key,
            b.contact_key
        from 
            rdm.f_transaction a
            join rdm.d_contact b on a.contact_key = b.contact_key
        ```
    - If each table contains the same column name, you will need to specify which column from each table to join on
    - Give each table an alias here we use `a` and `b`
    - Another example:
        ```sql
        select 
            p.product_name,	
            sum(t.item_amount) as sales
        from 
            rdm.f_transaction t 
            inner join rdm.d_product p on p.product_key = t.product_key
        where 
            t.bu_key = 17 
            and t.transaction_date_key between 20251201 and 20251212
            and member_sale_flag = ‘Y’
        group by 
            p.product_name;
        ```
        
    

### 2.1.7 Case When
- If we wanted to create a variable based of a condition (‘IF’ statement for example), we can use `case when`:
    ```sql
    select 
        case 
            when column_1 = condition_1 then ‘A’
            when column_1 = condition_2 then ‘B’
            else ‘C’ end 
        as letters,	
        column_2
    from 
        table_name
    group by 
        case 
            when column_1 = condition_1 then ‘A’
            when column_1 = condition_1 then ‘B’
            else ‘C’ end, 
        column_2
    ```
- Often used for creating age bands


### 2.2 F_TRANSACTION Table
[<u>Back to Top</u>](#sql-academy)

- ```rdm.f_transaction```
- Contains all transactional data 
- We use ```transaction_date_key``` (date of transaction) to set which dates we want to see data for
- There are multiple ```transation_type_name``` (header rows) in the ```f_transaction``` table. The most used are:
    - Sale – shows the total sale breakdown for the purchase
    - Item – breaks the purchase down to item level 
    - Promotion – breaks the purchase down into Promotions which were used
    - Point – shows the point breakdown for the purchase (for member transactions)
- `member_key` / `contact_key` are unique numbers used to represent members. 
- A non-member will have a contact key of 0
- `member_sale_flag` can be used to distinguish a member transaction from a non member transaction. 



### 2.3 Main KPIs
[<u>Back to Top</u>](#sql-academy)

- ATV – $\text{average transaction value}=\frac{\text{total sales}}{\text{total transactions}}$

- ATF – $\text{average transaction frequency}=\frac{\text{total transactions}}{\text{no. of members}}$

- ACV – $\text{average customer value}=\frac{\text{total sales}}{\text{no. of members}}$

- IPT – $\text{items per transaction}=\frac{\text{no. of items}}{\text{total transactions}}$

- PPU – $\text{price per unit}=\frac{\text{total sales}}{\text{no. of items}}$

- YoY change – $\text{year on year change} = \frac{\text{this year value}}{\text{last year value}} - 1$ &nbsp; &nbsp; &nbsp; &nbsp; *(multiply by 100 for %)*

- MSP – $\text{member sales participation}=\frac{\text{member sales}}{\text{total sales}}\times 100$

- $\text{6 month active rate}=\frac{\text{no. of members shopped in last 6 months}}{\text{no. of members shopped in last 3 years}}$


### 2.4 Main Time Periods
- YTD – Year to Date – e.g. YTD March 2025 = Jan 2025 to March 2025
- MTD – Month to Date – e.g. first 3 weeks of MTD May 2025 = W18-20 May 2025
- WTD – Week to Date – a given week period- e.g. WTD Week 11 = Week 11 only
- MAT – Moving Annual Total - last 12 months of data – e.g. MAT April 2025 = May 2021 to April 2025 
- LTM – Last 12 Months – same as MAT
- YoY – Year on Year – This Year vs Last Year


### 2.5 Examples
- Total number of members shopping in Marionnaud Austria in January 2025
    ```sql
    select 
        count(distinct contact_key) 
    from 
        rdm.f_transaction
    where 
        transaction_date_key between 20250103 and 20250130
        and member_sale_flag = ‘Y’
        and contact_key > 0
        and transaction_type_name = ‘Item’;
    ```
- Total Sales in MFR, YTD May 2025
    ```sql
    select 
        sum(item_amt) 
    from 
        rdm.f_transaction
    where 
        transaction_dt_key between 20250103 and 20250529
        and transaction_type_name = ‘Item’; 
    ```

### 2.6 B_PRODUCT Table
- `rdm.d_product`
- This table shows the information for every product sold
- We use product hierarchies to determine different categories and sub-categories BUs use (found in EU EDP model Document) 
- When calculation KPIs, we use always use the following exclusion: 
`kpi_exclusion_flag = ‘N’`


### 2.7 Recap and Final Note
- Basic Code Structure
    ```sql
    select 
        column_1,  
        sum(column_2)
    from 
        table_name
    where 
        condition_1
        and condition_2
    group by 
        column_1;
    ```
- SQL is not case sensitive for syntax but is for record contents.
- It does not have significant whitespace like Python, so you don’t need to worry about indentation, although try to keep things neat.
- Typical best practice is to use all-caps, but this doesn’t matter too much.
- However, if you would like to specify a record, e.g. Estée Lauder, you will need to write it exactly as it is listed in the database, matching the upper and lowercase letters.
- You can always use something like select distinct brand_name and find your desired record to see how it is formatted.

# 3 Exercises 

## 3.1 Warm Up Questions - F_TRANSACTION_DETAIL only


1. Return all transactions and unique members that happened in MIT on the 1st of January 2025.

2. How many items were bought and what is the total Member Sales on the 30th of May 2025 in MIT?

## 3.2 Exercises 1 - F_TRANSACTION_DETAIL

1. How many Dolce and Gabbana products are listed in D_PRODUCT which are sold in SD?

2. For all transactions that occurred in 2025 Fiscal week 15 in SD, return the following columns: `bu_key, contact_key, transaction_date_key, order_number, sku, product_description, quantity and item_total_regular_unit_price`

3. Return the following KPIs for 2025 fiscal week 30 (ATV, ATF, ACV, IPT, PPU) in SD. 
### [KPI definitions]
- ATV – $\text{average transaction value}=\frac{\text{total sales}}{\text{total transactions}}$

- ATF – $\text{average transaction frequency}=\frac{\text{total transactions}}{\text{no. of members}}$

- ACV – $\text{average customer value}=\frac{\text{total sales}}{\text{no. of members}}$

- IPT – $\text{items per transaction}=\frac{\text{no. of items}}{\text{total transactions}}$

- PPU – $\text{price per unit}=\frac{\text{total sales}}{\text{no. of items}}$

5. What was the most popular SKIN CARE product SKU in SD in 2025 in terms of total sales?

## 3.3 Exercises 2 - D_STORE and D_DATE tables

1. Top 5 stores by Member Sales in TPS for fiscal week 20 2025.

3. Top 5 men's fragrance SKUs sold in The Perfume Shop (TPS) within Liverpool throughout the February 2025 period.

4. For each week of January 2025, what percentage of members shopped on weekdays and on weekends in TPS?

## 3.4 Exercises 3 - D_CONTACT, D_MEMBER and More Complex Queries

1. How many members shopped 2+ times in January 2025 in ICINL?
2. How many male ICINL members who enrolled in 2025 are shopping again in 2024 (up to 202405)?


## 3.5 Exercises 4 - Segmentation tables and more on Tables

1. How many VIP Members bought Lancôme in SD in YTD March 2025?
2. What were the sales by week of Regular Female Members shopping in SD in February 2025?
3. What is the current distribution of RFM segments for SD Members?
5. Find the RFM Member Sales Split for SD in August 2025.

## 3.6 Exercises 5 - Age Calculations

The following code can be used for calculating generation bands, which is based off of `birth_date` to enable historical age data:

```
CASE
      WHEN EXTRACT(YEAR FROM C.birth_date) BETWEEN 2013 AND 2020 THEN 'Gen Alpha'
      WHEN EXTRACT(YEAR FROM C.birth_date) BETWEEN 1997 AND 2012 THEN 'Gen Z'
      WHEN EXTRACT(YEAR FROM C.birth_date) BETWEEN 1981 AND 1996 THEN 'Millennials (Gen Y)'
      WHEN EXTRACT(YEAR FROM C.birth_date) BETWEEN 1965 AND 1980 THEN 'Gen X'
      WHEN EXTRACT(YEAR FROM C.birth_date) BETWEEN 1946 AND 1964 THEN 'Baby Boomers'
      WHEN EXTRACT(YEAR FROM C.birth_date) BETWEEN 1928 AND 1945 THEN 'Silent Generation'
      ELSE 'UNSPECIFIED'
    END AS Generation
```
- note that it will need to be put in both the SELECT and the GROUP BY statements

### Exercises 5
1. What is the average age of MHU members which shopped Clinique in Week 25 2024? [Note: You would have to refer to birth date up till the current date to give age]
2. What were the total sales for MILLENIAL members for the online MHU store in May 2025?
3. What is the member generation split of MHU members that shopped Shiseido products in October 2025?

