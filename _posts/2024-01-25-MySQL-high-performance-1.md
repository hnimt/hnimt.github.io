# High-performance MySQL (Part 1)

## Optimizing Schema and Data Types

### Choosing Optimal Data Types
- Smaller is usually better: Smaller data types are usually better. Because they use less space on disk, in memory, and in CPU cache. They also require fewer CPU cycles to process.
- Avoid NULL if possible: Make indexes, index statistics, and value comparisons more complicated.
- Real Number: Only use Decimal when you want exact calculated data.
- String Types:
  - VARCHAR: Dynamic sized in InnoDB engine. Worth it when the maximum column length is much larger than the average length.
  - CHAR: Fixed length in InnoDB engine. Useful if you want to store very short strings, or if all the values are nearly the same length.
- BLOB and TEXT Types: designed to store large amounts of data.
  - When query, the engine needs to create an implicit temporary table. That results in a serious performance overhead.
- Using ENUM instead of String type:
  - ENUM can store a predefined set of distinct string values.
  - When join table ENUM join to ENUM faster than String join to String.
- Date and Time Types:
  - DATETIME: Allocate 8 bytes space
  - TIMESTAMP: Allocate 4 bytes space => Use TIMESTAMP if can
- BIT: Store String into Bit type => quite confusing => if you want to store boolean type => store CHAR(0)

### Choosing identifiers
- Integer types: best choice, because they're fast and work with AUTO_INCREMENT.
- ENUM and SET: a poor choice, because you need to look up the primary key table that stores enum or set.
- String types: avoid if possible, because they take up a lot of space and are generally slower than integer types.

### Schema design
- Prevent too many columns => high CPU consumptions
- Prevent too many joins.

