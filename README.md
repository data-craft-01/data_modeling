# Data Modeling

Key Concepts:
1. D
2. s

## Best Practices
1. Primary objective is to build a normalised data model right from the start.
- One table in a model should serve a single, distinct purpose.
- Narrow tables are better than short and wide tables.
- Use join conditions for relationships and not single wide merged tables.
2. Organise lookup tables above data tables in the diagram view.
- This serves as a visual reminder that filters flow downstream.
3. Avoid complex cross-filtering unless absolutely necessary.
- Don't use two-way filters when one-way filters will get the job done.
4. Hide foreign keys from the data tables so that users can only access valid fields. This will make their work easier and more accurate.
