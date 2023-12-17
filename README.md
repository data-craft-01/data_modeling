# Data Modeling

Key Concepts:
1. **Database Normalisation**
  - Normalisation is defined as the process of organising the tables and fields/columns in a relational database to reduce redundancy and preserve data integrity.
  - After normalisation, each table serves a distinct and specific purpose, i.e. the table showcases transactional data, product related information etc.
  - If normalisation isn't performed, the database will have duplicate information that could have been avoided with a dimension table this can lead to inefficeincies i storing data as the database scales in size.
  - Normalisations is recommended to achieve:
  > - Eliminate redundant data, thereby decreasing table sizes and improve processing speed and efficiency.
  > - Minimise errors and anomalies while modifying (inserting, updating or deleting records) data.

2. **Fact vs Dimension Tables**
   - Dimensional Modeling (approach for data modeling in analytic solutions) assumes that all the tables should be defined either as fact or a dimension table.
     | Fact Tables | Dimension/Lookup Tables |
     |---|---|
     |contain data or values or events or observations,  typically at a granular level with an identifier column having an ID or key, which can be used to create table relationships.| provide descriptive information about each dimension in a table|
     |Ex. sales transactions, exchange rates, temperatures|Ex. products, customers, locations, dates|

4. **Primary vs Foreign Key**
   - Primary key - uniquely identifies each record/row of a table and match foreign keys in related data tables.
   - Foreign keys - may contain multiple instances of each value and are used to match the primary keys in related lookup tables.

5. **Narrow vs Wide Table**
   - Wide/merged tables create redundant data and utilises significantly more memory and processing power than creating relationships between multiple narrow tables.

6. **Types of Database Schema**

### Star Schema:
1. **Structure:**
  - Centralized fact table surrounded by denormalized dimension tables.
  - Fact table contains the primary keys of dimension tables along with the measures.
  - Simple and easy to understand structure.

3. **Normalization:**
   - Fact table is denormalized, meaning redundant data is stored.
   - Redundancy helps in faster query performance.

4. **Join Complexity:**
   - Fewer joins are required for queries, contributing to faster query performance.
   - Well-suited for read-heavy workloads.

5. **Maintenance:**
   - Easier to maintain due to denormalization.
   - Changes in dimension tables do not affect the fact table.

6. **Query Performance:**
   - Generally offers better query performance for analytical queries.
   - Suitable for data warehouses with a star schema design.

### Snowflake Schema:
1. **Structure:**
   - Fact table is linked to normalized dimension tables through foreign key relationships.
   - Dimension tables may be further normalized into sub-dimensions.

2. **Normalization:**
   - Dimension tables are normalized, reducing redundancy.
   - Conforms to the principles of database normalization.

3. **Join Complexity:**
   - More joins are required for queries compared to a star schema.
   - May lead to slightly slower query performance.

4. **Maintenance:**
   - Requires more effort to maintain due to normalization.
   - Changes in dimension tables may impact multiple related tables.

5. **Query Performance:**
   - May experience slightly slower query performance compared to a star schema.
   - Suited for scenarios where normalization is critical, such as transactional systems.

### Summary:

     | **Star Schema** | **Snowflake Schema** |
     |---|---|
     |Denormalized structure|Normalized structure|
     |Faster query performance|More complex queries|
     |Easier maintenance|Requires more maintenance effort|
     |Well-suited for analytical queries and data warehousing|Suited for scenarios where data normalization is a priority, such as transactional databases|
     
- **Star Schema:**
  - Denormalized structure.
  - Faster query performance.
  - Easier maintenance.
  - Well-suited for analytical queries and data warehousing.

- **Snowflake Schema:**
  - Normalized structure.
  - More complex queries.
  - Requires more maintenance effort.
  - Suited for scenarios where data normalization is a priority, such as transactional databases.

Example:  
The `Sales_Fact` table captures the quantitative details of sales transactions, while the dimension tables provide descriptive information related to products, customers, time, and stores.  

### Fact Table: `Sales_Fact`
- The `Sales_Fact` table contains quantitative information about sales transactions. It includes measures such as:
  - `SalesAmount`: The amount of money generated from the sale.
  - `QuantitySold`: The number of units sold.
  - `DiscountAmount`: Any discounts applied to the sale.
  - `Profit`: Profit earned from the sale.

### Dimension Tables:
1. **`Product_Dim` (Product Dimension):**
   - `ProductID`: Primary key for the product dimension.
   - `ProductName`: Name of the product.
   - `Category`: Product category (e.g., electronics, clothing).
   - `Brand`: Brand of the product.

2. **`Customer_Dim` (Customer Dimension):**
   - `CustomerID`: Primary key for the customer dimension.
   - `CustomerName`: Name of the customer.
   - `City`: City where the customer is located.
   - `JoinDate`: Date when the customer joined.

3. **`Time_Dim` (Time Dimension):**
   - `DateID`: Primary key for the time dimension.
   - `Date`: Date of the sale.
   - `Month`: Month of the sale.
   - `Quarter`: Quarter of the year.

4. **`Store_Dim` (Store Dimension):**
   - `StoreID`: Primary key for the store dimension.
   - `StoreName`: Name of the store.
   - `Location`: Location of the store.
   - `Manager`: Store manager's name.

### Relationships:
- The `Sales_Fact` table's primary key is a composite key composed of foreign keys from the dimension tables:
  - `ProductID` (foreign key to `Product_Dim`).
  - `CustomerID` (foreign key to `Customer_Dim`).
  - `DateID` (foreign key to `Time_Dim`).
  - `StoreID` (foreign key to `Store_Dim`).

6. **Relationship Cardinality**
- Cardinality refers to the uniqueness of values in a column.
- All relationships in the data model should follow a "one-to-many" cardinality; one instance of each primary key, but potentially many instances of each foreign key.
- Example: In the case given below, there is one instance of each ProductKey in the Products table (noted by 1), and there are many instances of each product key in the Sales_Data table (noted by the asterisk *). This is due to there being multiple sales associated with each product.

![image](https://github.com/data-craft-01/data_modeling/assets/153006864/765f71b4-7f76-4b81-89df-2e985db6c54b)


## Best Practices
1. Primary objective is to build a normalised data model right from the start.
> - One table in a model should serve a single, distinct purpose.
> - Narrow tables are better than short and wide tables.
> - Use join conditions for relationships and not single wide merged tables.
2. Organise lookup tables above data tables in the diagram view.
> - This serves as a visual reminder that filters flow downstream.
3. Avoid complex cross-filtering unless absolutely necessary.
> - Don't use two-way filters when one-way filters will get the job done.
4. Hide foreign keys from the data tables so that users can only access valid fields. This will make their work easier and more accurate.
