# High-performance MySQL (Part 2)

## Indexing for High Performance

### Indexing Basics
- Reduce amounts of data to examine.
- Help server to avoid sorting and temporary table.
- Turn random I/O to sequential I/O.

### Types of Indexes:
- B-Trees Index:
  - Not useful when you do not start lookup from the left node side.
  - Can't skip columns in the index.
  - Can't optimize access with any columns to the right of the first range condition. (Like is not useful with right columns)
- Hash Index: The hash value of the record and index this value => only applies for equal conditions.

### Indexing Strategies for High Performance:
- Isolating the column: should not be a part of an expression or be inside of a function.
  - Example of bad indexing:
    ```
    SELECT actor_id FROM sakila.actor WHERE actor_id + 1 = 5;
    ```
- Prefix Indexes and Index Selectivity: applies for big identifiers such as BLOB, TEXT
  - Index column with the ratio of SELECT COUNT(DISTINCT LEFT(column_name, prefix_number))/COUNT(*)  more than 0.03. Ex:
    ```
    SELECT COUNT(DISTINCT LEFT(column_name, 3))/COUNT(*) 
    ```
- Multicolumn indexes:
  - AND conditions: use a single index
  - OR conditions: merge index
- Choosing a Good Column Order:
  - Place most distinct columns to the left:
    ```
    mysql> SELECT COUNT(DISTINCT staff_id)/COUNT(*) AS staff_id_selectivity,
         > COUNT(DISTINCT customer_id)/COUNT(*) AS customer_id_selectivity,
         > COUNT(*)
         > FROM payment\G
        *************************** 1. row ***************************
         staff_id_selectivity: 0.0001
        customer_id_selectivity: 0.0373
         COUNT(*): 16049
    ```
    In this situation should choose customer_id to the left of your query.

### Clustered Indexes:
- 
