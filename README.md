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
   - Database models generally have two types of tables:
   > - Fact Tables - contain data or values,  typically at a granular level with an identifier column having an ID or key, which can be used to create table relationships.
   > - Dimension/Lookup Tables - provide descriptive information about each dimension in a table.

3. **Primary vs Foreign Key**
   - Primary key - uniquely identifies each record/row of a table and match foreign keys in related data tables.
   - Foreign keys - may contain multiple instances of each value and are used to match the primary keys in related lookup tables.

4. **Narrow vs Wide Table
   - Wide/merged tables create redundant data and utilises significantly more memory and processing power than creating relationships between multiple narrow tables.

5. **Types of Database Schema**
   > a. Snowflake
   > b. Star Schema

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
