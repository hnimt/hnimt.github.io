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
- One table only had a clustered index (primary key).
- Primary key values are stored in the left node of B-tree => query by primary key is fastest.

### Using index scan for sorting:
- Only be used when the index has the same directory with the order
- Follow the leftmost prefix of the index

### Redundant index:
- Index 1 is customer_id and index 2 is (customer_id, order_id) => redundant index because of the same most left prefix. Index 2 covers index 1, but index 2 has more indexed columns => if you query only customer_id or insert and update is more expensive than index 1.
- Index 1 is (customer_id, order_id) and index 2 is (order_id, customer_id) => not redundant because of the different most left prefix.

### Indexes and Locking:
- Indexes permit queries to lock fewer rows because indexes help to query fewer rows on a table.
- 
