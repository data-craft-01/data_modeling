# Data Modeling  

Dimensional modeling is a preferred technique for presenting analytic data because it deliver's data that’s understandable to the business users with fast query performance.  
Best Practice - Dimensional models should correspond to physical data capture events of business's process and shouldn't be just designed to deliver for analytics use-cases/report.  

Designing a 'Dimensional Model' for a business is based in following key decisions:
1. Identifying which business process to be considered
2. Agree on the grain suitability
3. Identify which attributes qualify for dimensions
4. Identify which attributes qualify for facts

Each business process is represented by a dimensional model that consists of a fact table containing the event’s numeric measurements surrounded by a halo of dimension tables that contain the textual context that was true at the moment the event occurred.
Most often dimensional models are applied in relational database management systems and referred to as star schemas.


Data Movemement:  
**Transactions from the Source** >>> **ETL Layer** (ETL System - Transform from Source to Target , Normalization , Conform Dimensions | Design Goals - Throughput , Integrity & Consistency) >>> **Presentation Layer** (Dimensional Schema - Star vs Snowflake vs Cube, Atomic & Summary Data, Organized by business processes, Consider confirmed dimensions | Design Goals - Ease of use, Query Performance) >>> **BI Application** (Data mining & models, Analytic apps, Ad-hox queries, Standard reports)  


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
     |contain data or values or events or observations,  typically at a granular level with an identifier column having an ID or key, which can be used to create table relationships.| Provide descriptive information about each dimension in a table|
     |Ex. sales transactions, exchange rates, temperatures|Ex. products, customers, locations, dates|
     |---| Make sure to have unique values in each dimension to establish proper 1-M relationships between dimension and fact table by removing duplicates|

| Fact Tables for Measurements | Dimensional Tables for Descriptive Text |
|---|---|
| The fact table in a dimensional model stores the  performance measurements resulting from an organization’s business process events. | Dim tables contain textual context associated with a business process measurement event. They describe the “who, what, where, when, how, and why” associated with the event. |
| The term fact represents a business measure | The term fact represents description of a business event |
| Fact tables tend to be deep in terms of the number of rows, but narrow in terms of the number of columns. | Dimension tables tend to have fewer rows than fact tables, but can be wide with many large text columns. |
| Each row in a fact table corresponds to a measurement event. |  |
| One of the core tenets of dimensional modeling is that
all the measurement rows in a fact table must be at the same grain. |  |
| The idea that a measurement event in the physical world has a one-to-one relationship to a single row in the corresponding fact table is a bedrock principle for dimensional modeling.  |  |
| Facts are often described as continuously valued to help sort out what is a fact versus a dimension attribute. |  |
| The designer should make every effort to put textual data into dimensions where they can be correlated more effectively with the other textual dimension attributes and consume much less space |  |
| Unless the text is unique for every row in the fact table, it belongs in the dimension table. |  |
| All fact table grains fall into one of three categories: transaction, periodic snapshot, and accumulating
snapshot. |  |
| All fact tables have 2/more FK that connect to the dimension tables’ PK. |  |
| When all the keys in the fact table correctly match their respective PKs in the corresponding dimension tables, the tables satisfy referential integrity. | Each dimension is defined by a single PK, which serves as the basis for referential integrity with any given fact table to which it is joined.
DWH is only as good as the dimension attributes; the analytic power of the DW/BI environment is directly proportional to the quality and depth of the dimension attributes. |
| Fact table generally has its own PK composed of a subset of the FKs. This key is often called a composite key. Every table that has a composite key is a fact table. |  |
| Fact tables express many-to-many relationships. All others are dimension tables. |  |
| Fact attributes are referred for numeric values/aggregations in select clause. | Dimension attributes serve as the primary source of report filters, query constraints, groupings, and report labels.
Robust dimension attributes deliver robust analytic slicing-and-dicing capabilities. |
| When triaging operational source data, it is sometimes unclear whether a numeric data element is a fact or dimension attribute. You often make the decision by asking whether the column is a measurement that takes on lots of values and participates in calculations (making it a fact) or is a discretely valued description that is more or less constant and participates in constraints and row labels (making it a dimensional attribute). The designer’s dilemma of whether a numeric quantity is a fact or a dimension attribute is rarely a difficult decision. Continuously valued numeric observations are almost always facts; discrete numeric observations drawn from a small list are almost always dimension attributes. |  |
|  | Instead of 3NF (third normal form), dimension tables typically are highly denormalized with flattened many-to-one relationships within a single dimension table.
Because dimension tables typically are geometrically smaller than fact tables, improving storage efficiency by normalizing or snowflaking has virtually no impact on the overall database size. |
| Do not normalize dimension data by storing only one attribute in one of the dimension table and
creating a separate lookup dim table, and likewise for the other descriptions in separate lookup dim table - this normalization is called snowflaking |  |

    

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
