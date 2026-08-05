# SQL Learning Roadmap

### From Absolute Beginner to Expert / Master Level

**A Professional Self-Study Engineering Curriculum**

---

> **How to use this document**
> Work through each stage in order. Do not skip stages based on familiarity with syntax — each stage builds conceptual foundations the next depends on. Mark milestones as you pass them. The readiness checklists are gates, not suggestions.

---

## Table of Contents

1. [SQL vs Database Engineering — The Critical Distinction](#1-sql-vs-database-engineering)
2. [Stage 1 — Beginner](#2-stage-1--beginner)
3. [Stage 2 — Intermediate](#3-stage-2--intermediate)
4. [Stage 3 — Advanced](#4-stage-3--advanced)
5. [Stage 4 — Expert / Master](#5-stage-4--expert--master)
6. [Realistic Time Estimates](#6-realistic-time-estimates)
7. [What Separates Each Level](#7-what-separates-each-level)
8. [What Companies Actually Expect](#8-what-companies-actually-expect)

---

## 1. SQL vs Database Engineering

Understanding the difference between these three things is the most important meta-concept in this curriculum. Most people conflate them. They are not the same.

### 1.1 SQL Syntax Mastery

Knowing what keywords exist, what they do, and how to combine them. Learning the grammar of the language. This is the _smallest_ part of becoming proficient with databases. Syntax can be looked up in 30 seconds. Understanding cannot.

**What it looks like:** Someone who has memorized SELECT, JOIN, GROUP BY, and window function syntax. Can copy-paste and adapt queries. Struggles when a problem doesn't match a pattern they've seen before.

### 1.2 Relational Understanding

Grasping the mathematical foundation: sets, relations, keys, and functional dependencies. Knowing _why_ normalization works. Being able to look at any schema and reason about its correctness, redundancy, and query implications without being guided.

**What it looks like:** Someone who can read an unfamiliar schema and immediately begin reasoning about entity relationships, join paths, and data quality. Can design schemas from scratch. Understands what a query _means_, not just what it returns.

### 1.3 Database Engineering

Understanding storage engines, buffer pools, write-ahead logging, indexing internals, query planning, and concurrency control. Designing systems that handle real workloads reliably at scale. This extends well beyond SQL — it is the engineering of the database itself.

**What it looks like:** Someone who reads an execution plan like a blueprint, predicts the performance implications of a schema decision before writing code, and makes architectural trade-offs (denormalization, partitioning, replication) with full awareness of consequences.

### 1.4 Why This Distinction Matters

A person can memorize every SQL keyword and still design schemas with catastrophic redundancy. A person can write flawless normalized schemas and still write queries that destroy a production database at scale. A person can write excellent queries against a well-designed schema and still be unable to make a single architectural decision about the system.

**These are three genuinely different skill sets.** This curriculum builds all three — in order — because each layer depends on the one before it.

---

## 2. Stage 1 — Beginner

> **Focus:** The relational model, single-taonble queries, filtering, sorting, and aggregation.

### 2.1 What You Are Building

Before writing a single complex query, you must internalize a mental model: data is organized into tables of rows and columns, every table should have a primary key, tables relate to each other through foreign keys, and NULL is not zero or empty string — it is the _absence of a value_. Most bugs beginners write stem from not having this model clear.

### 2.2 Topics

#### Foundational Concepts

- What a relational database is (tables, rows, columns, schemas)
- Primary keys — purpose, not just syntax
- Foreign keys — concept of referential integrity
- Data types: `INT`, `VARCHAR`, `CHAR`, `DATE`, `TIMESTAMP`, `BOOLEAN`, `DECIMAL`, `FLOAT`
- NULL semantics — what NULL means and how it behaves in comparisons

#### Core Query Syntax

- `SELECT`, `FROM`, `WHERE` — the foundation of every query
- `AND`, `OR`, `NOT` — boolean logic in filter conditions
- `ORDER BY` — ascending and descending sort
- `LIMIT` / `TOP` / `FETCH FIRST` — restricting row count (dialect-specific)
- `DISTINCT` — deduplication (conceptual understanding, not just syntax)
- `BETWEEN`, `IN` — shorthand conditions
- `LIKE` — pattern matching with `%` and `_` wildcards
- `IS NULL` / `IS NOT NULL` — null-safe comparisons
- Column and table aliases using `AS`

#### Aggregation

- `COUNT(*)` vs `COUNT(column)` — the difference matters
- `SUM`, `AVG`, `MIN`, `MAX`
- `GROUP BY` — understanding that it changes the _grain_ of the output
- `HAVING` — filtering on aggregate results (not the same as WHERE)

#### Basic Data Manipulation

- `INSERT INTO` — adding rows
- `UPDATE ... SET ... WHERE` — modifying rows (always with a WHERE clause)
- `DELETE FROM ... WHERE` — removing rows (always with a WHERE clause)

#### Basic Data Definition

- `CREATE TABLE` — defining structure
- `DROP TABLE` — removing a table
- `ALTER TABLE` — adding/removing columns (conceptual awareness)

### 2.3 Why These Topics Matter

Every SQL query — regardless of complexity — is SELECT + FROM + WHERE at its core. If you don't understand how a table is structured, what nulls mean, and how GROUP BY changes what a row represents, nothing more complex will make sense. This stage builds the foundational mental model that all later learning depends on.

### 2.4 Difficulty Level

`LOW` — Practice with small, flat tables (2–3 columns). Problems are single-table queries. Logic is explicit — you can trace each row manually and verify the output.

### 2.5 SQL Capability After This Stage

Can query a single table correctly. Can filter, sort, group, and aggregate. Can answer most basic business questions ("how many orders were placed last month?", "what is the average order value by region?") against a clean, simple table.

### 2.6 Common Beginner Mistakes

> ⚠️ **Mistake:** Using `WHERE` to filter aggregate results.
> **Fix:** `WHERE` runs before aggregation. Use `HAVING` to filter on aggregated values.

> ⚠️ **Mistake:** Treating `COUNT(*)` and `COUNT(column)` as identical.
> **Fix:** `COUNT(*)` counts all rows. `COUNT(column)` skips NULLs in that column. The difference matters.

> ⚠️ **Mistake:** Not understanding that `GROUP BY` changes the grain of the output.
> **Fix:** After `GROUP BY`, every row in the result represents a _group_, not an individual row from the original table.

> ⚠️ **Mistake:** Selecting non-aggregated columns without including them in `GROUP BY`.
> **Fix:** Every column in SELECT must either be in GROUP BY or wrapped in an aggregate function.

> ⚠️ **Mistake:** Treating NULL as zero or empty string.
> **Fix:** `NULL = NULL` evaluates to NULL (not TRUE). Always use `IS NULL` / `IS NOT NULL`. Any arithmetic with NULL returns NULL.

> ⚠️ **Mistake:** Writing `UPDATE` or `DELETE` without a `WHERE` clause.
> **Fix:** Always write the `WHERE` clause first, before the rest of the statement, until it becomes habit.

> ⚠️ **Mistake:** Thinking `ORDER BY` affects how data is stored or how joins work.
> **Fix:** `ORDER BY` only affects the presentation of results. It has no semantic effect on anything else.

### 2.7 Readiness Checklist — Gate to Intermediate

Do not advance until you can check every item:

- [ ] Can write any single-table query from a plain English description
- [ ] Can explain what each SQL clause does and when it runs (logical processing order)
- [ ] Can mentally trace which rows exist after each filter condition
- [ ] Can explain the difference between row-level and group-level operations
- [ ] Have internalized NULL behavior — not just avoided it
- [ ] Can explain WHY a query is correct, not just that it runs

---

## 3. Stage 2 — Intermediate

> **Focus:** Joins, subqueries, set operations, multi-table reasoning, and basic data modeling.

### 3.1 The Key Conceptual Shift

Stop thinking about rows. Start thinking about **sets of data**. A join is not a loop — it is an operation on two sets with a combining condition. A subquery is not a second query that runs separately — it is an expression that returns a set, used inside another expression. Internalizing this makes every join problem easier and eliminates the most common join bugs.

### 3.2 Topics

#### Joins

- `INNER JOIN` — the Cartesian product + filter model (understand it mechanically)
- `LEFT OUTER JOIN` — which rows are preserved and why
- `RIGHT OUTER JOIN` — equivalent to LEFT JOIN with tables swapped
- `FULL OUTER JOIN` — all rows from both sides, nulls for missing matches
- Self joins — joining a table to itself with different aliases
- Non-equi joins — join conditions using `<`, `>`, `BETWEEN`
- Multi-table joins — chaining more than two tables
- Understanding row multiplication when joining on non-unique columns

#### Subqueries

- Subqueries in `WHERE` — non-correlated (runs once) vs correlated (runs per row)
- Subqueries in `FROM` — derived tables / inline views
- Subqueries in `SELECT` — scalar subqueries
- `EXISTS` and `NOT EXISTS` — row existence checks
- `IN` vs `EXISTS` — when each is appropriate and the NULL behavior difference
- `ANY` / `ALL` — comparison against a set of values

#### Set Operations

- `UNION` vs `UNION ALL` — when to use each (UNION deduplicates, UNION ALL does not)
- `INTERSECT` — rows that appear in both result sets
- `EXCEPT` / `MINUS` — rows in first set not in second

#### Conditional Logic

- `CASE WHEN ... THEN ... ELSE ... END` — conditional expressions inside queries
- Nested CASE expressions
- CASE inside aggregate functions (conditional aggregation pattern)

#### Functions

- **String:** `CONCAT`, `UPPER`, `LOWER`, `TRIM`, `LTRIM`, `RTRIM`, `SUBSTRING`, `REPLACE`, `LENGTH`, `POSITION`, `LEFT`, `RIGHT`
- **Date/Time:** `DATEADD`, `DATEDIFF`, `EXTRACT`, `DATE_TRUNC`, `NOW()`, `CURRENT_DATE`, `TO_DATE`, `FORMAT`
- **Numeric:** `ROUND`, `FLOOR`, `CEIL`, `ABS`, `MOD`, `POWER`, `SQRT`
- **Null handling:** `COALESCE`, `NULLIF`, `ISNULL`/`IFNULL`
- **Type conversion:** `CAST(x AS type)`, `CONVERT(type, x)`

#### Data Modeling Concepts

- First Normal Form (1NF) — atomic values, no repeating groups
- Second Normal Form (2NF) — no partial dependencies on composite keys
- Third Normal Form (3NF) — no transitive dependencies
- One-to-many relationships — foreign keys and referential integrity
- Many-to-many relationships — junction/bridge tables
- Entity-relationship (ER) diagram reading

#### Indexes (Conceptual Introduction)

- What an index is and why it exists
- Primary key indexes vs secondary indexes
- When indexes help (high-selectivity queries) and when they don't

### 3.3 Why These Topics Matter

Real production databases have dozens to hundreds of tables. The data you need for any given business question almost never lives in one place. Joins and subqueries are the primary tools for combining information across a normalized schema. Understanding the _semantics_ of each join type — not just the syntax — is what separates engineers who accidentally double-count rows from engineers who never do.

### 3.4 Difficulty Level

`MEDIUM` — 3–5 table schemas. Problems require tracing data across relationships. Subtle mistakes (wrong join type, missing GROUP BY columns, NOT IN with nulls) produce results that look plausible but are wrong.

### 3.5 SQL Capability After This Stage

Can query any normalized database at basic-to-moderate complexity. Can answer the majority of analytics questions against a production schema. Meets the SQL bar for most data analyst roles. Can write queries that would appear in real analytical work.

### 3.6 Common Intermediate Mistakes

> ⚠️ **Mistake:** Joining on non-unique columns without understanding row multiplication.
> **Fix:** If the join key appears N times on the left and M times on the right, the result has N×M rows for that key. Always verify the uniqueness of your join columns.

> ⚠️ **Mistake:** Using `LEFT JOIN` then filtering on the right-side column in `WHERE`.
> **Fix:** A `WHERE` condition on a right-side column turns the LEFT JOIN into an INNER JOIN, silently. Move nullable conditions into the `ON` clause, or use `WHERE right_col IS NULL OR right_col = value`.

> ⚠️ **Mistake:** `NOT IN` with a subquery that can return NULLs.
> **Fix:** `NOT IN (1, 2, NULL)` returns zero rows — any comparison with NULL evaluates to NULL. Use `NOT EXISTS` when the subquery column may contain nulls.

> ⚠️ **Mistake:** Using `UNION` when `UNION ALL` is correct, silently dropping legitimate duplicates.
> **Fix:** Default to `UNION ALL` unless you specifically need deduplication. `UNION` has a sort/deduplicate overhead cost on top of this.

> ⚠️ **Mistake:** Correlated subqueries used where a join would work, with no awareness of the performance difference.
> **Fix:** A correlated subquery in WHERE re-executes once per row of the outer query. For large tables, this is catastrophic. Learn to recognize when it can be rewritten as a JOIN.

### 3.7 Readiness Checklist — Gate to Advanced

- [ ] Can write multi-table queries without row-count bugs
- [ ] Can explain what each join type does at the set level, not just by syntax
- [ ] Can choose consciously between subquery and join approaches
- [ ] Can read a simple schema and reason about entity relationships
- [ ] Can explain the `NOT IN` / NULL problem without looking it up
- [ ] Can normalize a flat table to 3NF given a set of functional dependencies
- [ ] Can explain WHY a query is correct, in terms of sets and relationships

---

## 4. Stage 3 — Advanced

> **Focus:** Window functions, CTEs, query optimization, execution plans, transactions, and schema design.

### 4.1 The Key Conceptual Shift

From _making queries work_ to _making queries work correctly, efficiently, and maintainably_. You start caring about the database as a system, not just a box that returns rows. You begin thinking about indexes before writing queries, not after. Performance is no longer an afterthought.

### 4.2 Topics

#### Window Functions — The Most Important SQL Concept to Learn

Window functions are the single biggest leap in SQL expressive power. They operate on a set of rows _related to the current row_ without collapsing them into groups the way GROUP BY does.

- `OVER()` clause structure — the window definition
- `PARTITION BY` — dividing rows into independent windows
- `ORDER BY` inside OVER — ordering within the window
- Frame specification: `ROWS BETWEEN` vs `RANGE BETWEEN`
  - `UNBOUNDED PRECEDING`, `CURRENT ROW`, `UNBOUNDED FOLLOWING`, `N PRECEDING/FOLLOWING`
- Ranking functions: `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`, `NTILE(n)`
- Offset functions: `LAG(col, n)`, `LEAD(col, n)` — accessing adjacent rows
- Value functions: `FIRST_VALUE(col)`, `LAST_VALUE(col)`, `NTH_VALUE(col, n)`
- Aggregate functions as window functions: `SUM() OVER()`, `AVG() OVER()`, `COUNT() OVER()`
- Running totals, moving averages, cumulative distributions, percent of total
- Window functions cannot be used in WHERE or HAVING (use a subquery/CTE to filter on them)

#### Common Table Expressions (CTEs)

- `WITH` clause syntax — naming a result set for reuse
- Multiple CTEs in a single query
- CTEs vs subqueries — readability, structure, and reusability
- CTEs vs temporary tables — when each is appropriate
- **Recursive CTEs** — the pattern for hierarchical and graph data
  - Anchor member + recursive member structure
  - Termination conditions (essential — a missing condition creates an infinite loop)
  - Applications: org charts, category trees, bill of materials, path traversal

#### Query Execution Internals

- The logical processing order of a SQL query:
  `FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT`
- Why this order matters for understanding what you can and cannot reference where
- `EXPLAIN` and `EXPLAIN ANALYZE` — reading execution plans
- Key plan nodes: Sequential Scan, Index Scan, Index-Only Scan, Nested Loop, Hash Join, Merge Join
- Estimated vs actual row counts — when they diverge and what it means
- Cost numbers in plans — what they represent and their limits

#### Indexing in Practice

- B-tree indexes — structure, traversal, range query support
- Hash indexes — equality only, no range support
- Composite (multi-column) indexes — column order matters
- Covering indexes — index-only scans
- Partial indexes — indexes on a filtered subset of rows
- Expression indexes — indexing on a function of a column
- Index selectivity and cardinality — why low-cardinality indexes hurt
- Sargable vs non-sargable predicates — wrapping a column in a function kills index use
- Index maintenance overhead — writes become slower as indexes are added

#### Transactions and Concurrency

- ACID properties: Atomicity, Consistency, Isolation, Durability
- `BEGIN`, `COMMIT`, `ROLLBACK`, `SAVEPOINT`
- Read phenomena: dirty reads, non-repeatable reads, phantom reads
- Isolation levels: `READ UNCOMMITTED`, `READ COMMITTED`, `REPEATABLE READ`, `SERIALIZABLE`
- Optimistic vs pessimistic locking (conceptual)
- Deadlocks — what they are and how they occur
- `SELECT FOR UPDATE` — explicit row locking

#### Schema Design

- Choosing appropriate data types for performance and correctness
- `ENUM` types — when useful and when dangerous
- Computed/generated columns
- Constraints: `NOT NULL`, `UNIQUE`, `CHECK`, `DEFAULT`, `FOREIGN KEY`
- Cascading actions: `ON DELETE CASCADE`, `ON DELETE SET NULL`, `ON UPDATE CASCADE`
- Junction tables for many-to-many relationships
- Star schema and snowflake schema — introduction to dimensional modeling
- Fact tables and dimension tables
- Natural vs surrogate keys — trade-offs

#### Procedural SQL (Introduction)

- Stored procedures — definition, parameters, use cases
- User-defined functions (scalar and table-valued)
- Triggers — BEFORE/AFTER, INSERT/UPDATE/DELETE, when they're useful and dangerous
- Views — simple views vs materialized views

#### Advanced Patterns

- Conditional aggregation: `SUM(CASE WHEN ... THEN 1 ELSE 0 END)`
- PIVOT and UNPIVOT (or equivalent conditional aggregation)
- Gap and island problems — a window function pattern to know deeply
- Top-N per group — the ROW_NUMBER() pattern
- Deduplication with window functions
- Date spine / calendar table patterns

### 4.3 Why These Topics Matter

Window functions alone replace entire categories of complex, error-prone subquery patterns. CTEs make long query chains readable and debuggable. Execution plans transform performance work from guesswork into engineering. This is the stage where SQL becomes a tool for building analytical systems, not just retrieving data.

### 4.4 Difficulty Level

`HIGH` — Complex schemas, multi-step analytical problems. Subtle frame specification errors in window functions silently produce wrong results. Understanding execution plans requires simultaneous SQL and systems thinking.

### 4.5 SQL Capability After This Stage

Can solve virtually any analytical problem in SQL. Can write complex reports, cohort analyses, time-series calculations, and hierarchical traversals. Can review a slow query and identify why it's slow. Meets the SQL bar for senior data analyst, analytics engineer, and data engineer roles.

### 4.6 Common Advanced Mistakes

> ⚠️ **Mistake:** Confusing `RANGE BETWEEN` and `ROWS BETWEEN` in window frames.
> **Fix:** `ROWS` is physical (N rows before/after). `RANGE` is logical (values within N of current). For running totals with duplicate ORDER BY values, `ROWS` is almost always what you want.

> ⚠️ **Mistake:** Recursive CTE without a proper termination condition.
> **Fix:** The recursive member must reduce the result set with each iteration. Always include a depth counter or a termination predicate, and test with small inputs first.

> ⚠️ **Mistake:** Over-indexing — adding indexes to every column.
> **Fix:** Every index slows down INSERT, UPDATE, and DELETE. Add indexes based on measured query patterns, not preemptively.

> ⚠️ **Mistake:** Trusting query cost numbers without checking row estimates.
> **Fix:** Plan costs are only as good as the statistics the optimizer has. If estimated rows diverge wildly from actual rows, the plan is based on stale statistics — run ANALYZE first.

> ⚠️ **Mistake:** Assuming CTEs are always materialized (or always inlined).
> **Fix:** Different engines treat CTEs differently. PostgreSQL inlines them by default in v12+. Earlier versions and other engines may materialize. When performance is critical, test with and without CTEs.

> ⚠️ **Mistake:** Wrapping an indexed column in a function in WHERE.
> **Fix:** `WHERE UPPER(email) = 'X'` cannot use an index on `email`. Either store data in normalized form, or create an expression index on `UPPER(email)`.

### 4.7 Readiness Checklist — Gate to Expert

- [ ] Can write window functions with custom frames from memory, correctly
- [ ] Can read a basic execution plan and identify the bottleneck
- [ ] Can design a schema for a described problem — not just query an existing one
- [ ] Can write a recursive CTE for a tree traversal without reference
- [ ] Understands ACID and can reason about what happens if a transaction fails mid-way
- [ ] Knows at least two SQL dialects and their key differences
- [ ] Can explain the gap-and-island pattern from first principles

---

## 5. Stage 4 — Expert / Master

> **Focus:** Storage internals, optimizer behavior, concurrency control, architectural design, and scale.

### 5.1 The Key Conceptual Shift

You stop thinking in SQL and start thinking in _execution_. You see a query and immediately visualize the access path. You design schemas with future query shapes and data volumes in mind. You understand that correctness and performance are both hard constraints, and you navigate trade-offs deliberately rather than accidentally.

Expert SQL is really **database engineering** — not just proficiency with a query language.

### 5.2 Topics

#### Query Optimizer Internals

- Cost-based optimizer (CBO) — how the optimizer estimates plan cost
- Rule-based optimizer — transformation rules applied before cost estimation
- Statistics: column histograms, n-distinct, most common values
- Statistics maintenance: `ANALYZE`, `UPDATE STATISTICS`, `DBCC UPDATEUSAGE`
- Plan hints — when and how to force a specific plan (and why to avoid it)
- Query rewrite rules — how the optimizer transforms your query before planning
- Join order selection — why order matters and how the optimizer chooses

#### Storage Engine Internals

- Pages and blocks — the fundamental unit of storage I/O
- Buffer pool / shared buffer cache — how data is kept in memory
- Write-Ahead Logging (WAL) — how durability is guaranteed
- Heap storage vs index-organized (clustered) table storage
- Row storage vs columnar storage — OLTP vs OLAP trade-offs
- TOAST (PostgreSQL) — handling large column values
- Table bloat — dead tuples and why VACUUM matters

#### Multi-Version Concurrency Control (MVCC)

- How MVCC enables non-blocking reads
- Transaction IDs and tuple visibility
- How VACUUM reclaims dead tuple space
- The autovacuum system and its configuration
- Read phenomena under each isolation level, concretely
- Serialization failures and retry logic

#### Partitioning

- Range partitioning — partition by date or numeric range
- List partitioning — partition by discrete values
- Hash partitioning — distribute rows evenly
- Partition pruning — how the optimizer skips irrelevant partitions
- Partition key selection — the most important design decision
- Partitioning for maintenance: `DETACH PARTITION` for fast deletes

#### Advanced Indexing

- B-tree internals — balanced tree, page splits, fill factor
- GIN (Generalized Inverted Index) — for full-text search and array operators
- GiST (Generalized Search Tree) — for geometric data, range types
- BRIN (Block Range Index) — for naturally ordered large tables
- Index bloat and REINDEX
- Covering indexes vs include columns (PostgreSQL `INCLUDE`)
- Partial and expression indexes — advanced use cases

#### Full-Text Search

- `tsvector` and `tsquery` — the PostgreSQL full-text model
- `to_tsvector()`, `to_tsquery()`, `plainto_tsquery()`
- GIN index for full-text
- Ranking: `ts_rank()`, `ts_rank_cd()`
- When full-text search is appropriate vs LIKE vs pattern matching vs dedicated search engines

#### Advanced Data Modeling

- Boyce-Codd Normal Form (BCNF), 4NF, 5NF — and when they matter
- Deliberate denormalization — when it is the right choice and how to document it
- Slowly Changing Dimensions (SCD Types 1–6) — tracking historical state
- Temporal tables and bi-temporal data — valid time vs transaction time
- Event sourcing patterns in SQL
- Graph data representation in relational databases:
  - Adjacency list — simple but recursive queries are expensive
  - Nested sets — fast reads, painful writes
  - Closure table — the generally recommended pattern
  - Path enumeration — trades storage for simplicity

#### Replication and Distributed Concerns

- Streaming replication — primary/replica topology
- Logical replication — use cases and limitations
- Read replicas — query routing implications
- Sharding strategies and their effect on query structure
- Cross-shard queries — why they are expensive and how to avoid them
- CAP theorem as it applies to distributed SQL systems

#### Schema Migration at Scale

- Online schema changes — tools: `pt-online-schema-change`, `gh-ost`, `pglogical`
- Adding columns with defaults on large tables — why it can be dangerous
- Creating indexes concurrently (`CREATE INDEX CONCURRENTLY`)
- Zero-downtime migration patterns
- Backward-compatible vs breaking schema changes

#### Performance Engineering

- Query regression testing methodology
- Benchmarking: `pgbench`, `sysbench`
- Identifying N+1 query patterns in application code
- Connection pooling — `PgBouncer`, `ProxySQL`
- Caching layers and their interaction with SQL

### 5.3 Why These Topics Matter

At expert level, the problems are architectural. A slow query at 10 million rows becomes a system failure at 1 billion rows unless the storage and access patterns were designed correctly from the start. Understanding the engine is what enables you to make decisions that hold up under scale — and to fix decisions that don't.

### 5.4 Difficulty Level

`VERY HIGH` — Problems are no longer "write this query." They are "this query runs in 40 seconds on 200M rows, find and fix every contributing factor" or "design a schema for this access pattern that will support 10,000 writes per second with sub-100ms read latency." Requires synthesizing knowledge from multiple areas simultaneously.

### 5.5 SQL Capability After This Stage

Can design, build, audit, and tune production database systems. Can diagnose any performance problem. Can make architecture decisions with full understanding of trade-offs. Meets the bar for database engineer, data architect, and staff-level data roles.

### 5.6 Expert Mastery Markers

| Capability                                            | How to verify                                                                  |
| ----------------------------------------------------- | ------------------------------------------------------------------------------ |
| Read a cold execution plan and predict the bottleneck | Given a plan for an unfamiliar query, diagnose the problem before running it   |
| Design a schema for a given access pattern            | Given requirements, produce a schema with justified choices                    |
| Know when NOT to normalize                            | Can explain a deliberate denormalization decision with full trade-off analysis |
| Explain MVCC's effect on VACUUM and table bloat       | Can trace what happens to a table after 1M updates without VACUUM              |
| Implement any graph traversal in pure SQL             | Can write recursive CTEs and closure table queries without reference           |
| Know the full-text vs pattern search boundary         | Can explain when each is appropriate and what the performance cost is          |
| Reason about lock contention in concurrent workloads  | Can trace a deadlock scenario and redesign the transaction to eliminate it     |
| Migrate a billion-row table with zero downtime        | Can name the tools and steps, understand the risks at each stage               |

---

## 6. Realistic Time Estimates

> **Important:** These estimates assume focused, problem-solving study — not passive reading or tutorial-watching. An hour of solving a problem you cannot immediately solve counts for significantly more than an hour of reading documentation you mostly already understand.

### 6.1 Hours Required Per Stage

| Stage        | Focused Hours Needed | Notes                                            |
| ------------ | -------------------- | ------------------------------------------------ |
| Beginner     | 40 – 60 hrs          | Single-table problems, conceptual foundations    |
| Intermediate | +80 – 120 hrs        | Multi-table reasoning, schema reading            |
| Advanced     | +150 – 250 hrs       | Window functions, execution plans, schema design |
| Expert       | +300 – 600 hrs       | Internals, architecture, scale — high variance   |

### 6.2 Calendar Time by Study Intensity

| Level Reached         | 1 hr / day  | 2 hrs / day   | 4 hrs / day   |
| --------------------- | ----------- | ------------- | ------------- |
| Beginner complete     | ~2 months   | ~1 month      | ~2 weeks      |
| Intermediate complete | ~6 months   | ~3 months     | ~6 weeks      |
| Advanced complete     | ~14 months  | ~7 months     | ~4 months     |
| Expert / Master       | 2 – 3 years | 1.5 – 2 years | 7 – 10 months |

### 6.3 Critical Caveats

> ⚠️ **Expert level is not purely a function of hours.** It requires _variety_ and _depth_ of problems. Doing the same type of query 500 times is not 500 hours of expert progress — it is one hour of progress repeated 500 times.

> ⚠️ **The Advanced → Expert gap is the hardest to estimate.** Exposure to production systems with real query tuning challenges dramatically compresses this timeline. Without that exposure, progress slows substantially.

> ⚠️ **Sustained 4 hrs/day is cognitively demanding.** The estimates assume high-quality focused practice, not time with a screen open. Cognitive fatigue reduces the effective rate in the back half of long sessions.

> ⚠️ **Background matters.** Developers with prior programming experience (Python, Java, etc.) tend to move faster through Beginner and Intermediate, primarily because control flow, boolean logic, and data types are already familiar. The Advanced and Expert stages equalize faster.

---

## 7. What Separates Each Level

### 7.1 Beginner → Intermediate

**The gap is relational reasoning.**

A beginner's mental model: data lives in one table. A query is a filter over that table. The database is a spreadsheet with a query language.

An intermediate's mental model: data is distributed across tables with relationships. A query is a declaration of which sets to combine and how. The problem is decomposing a business question into join paths and subquery expressions.

The practical test: can you look at a schema you have never seen, read the foreign key relationships, and begin writing a correct multi-table query without being shown how? A beginner cannot — they need the query explained first. An intermediate can — because they reason from relationships, not from examples.

### 7.2 Intermediate → Advanced

**The gap is analytical thinking and performance awareness.**

An intermediate's ceiling: queries either work or don't. Performance is not part of the mental model. Window functions don't exist. Complexity is handled by writing _more_ SQL rather than _better_ SQL.

An advanced engineer writes queries differently — not just more accurately. They evaluate what the engine will do with the query before finishing it. Window functions are the clearest signal: an intermediate who hasn't learned them will write a 20-line correlated subquery for something that should be a single `LAG()` call. An advanced engineer sees that immediately.

The practical test: given a slow query on a real schema, can you read the execution plan, identify the access pattern problem, and fix it? An intermediate cannot do this. An advanced engineer can.

### 7.3 Advanced → Expert

**The gap is systems thinking.**

The Expert/Advanced boundary is the least about SQL syntax and the most about understanding databases as _systems_. Storage, memory, concurrency, disk I/O, and query planning all interact. An expert understands those interactions.

An advanced engineer can write excellent queries against a well-designed schema. An expert can design the schema, predict the performance characteristics of both the schema and the queries against it, and make architectural decisions about the storage system itself — all before writing a single row of data.

This transition almost always requires direct exposure to large, real datasets with genuine performance problems. It is extremely difficult to reach without that experience.

---

## 8. What Companies Actually Expect

### 8.1 SQL Requirements by Role

| Role                | Minimum SQL Level              | What Actually Differentiates Candidates                                                                                         |
| ------------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- |
| Junior data analyst | Beginner complete              | Whether they can read a schema and ask the right questions. SQL is verified, not deeply tested.                                 |
| Data analyst        | Mid-Intermediate               | Speed and accuracy on ad hoc questions. Can they write queries they've never seen before, correctly, without looking things up? |
| Senior data analyst | Intermediate–Advanced boundary | Can they teach others? Can they audit existing queries for correctness? Do they notice data model problems?                     |
| Analytics engineer  | Advanced, beginning Expert     | Data modeling philosophy. Can they design a semantic layer that serves multiple downstream consumers?                           |
| Data engineer       | Full Advanced                  | Performance thinking, schema design, ETL patterns. Can they build pipelines that are both correct and efficient?                |
| Database engineer   | Expert                         | Storage internals, concurrency, replication, partition design. Judgment under architectural constraints.                        |
| Data architect      | Expert                         | Given requirements and constraints, can they make the right architectural decision and defend it with trade-off analysis?       |

### 8.2 What Companies Test vs What They Actually Need

Most SQL interviews test _syntax recall_ — can you write a window function from memory, can you explain the difference between RANK and DENSE_RANK, can you identify what's wrong with a given query. This is a proxy for depth, not a direct measure of it.

What companies actually care about:

1. Does this person make _correct analytical decisions_ with data?
2. Will their queries serve the database or damage it?
3. Can they reason about data they have never seen before?

The best preparation is not memorizing syntax. It is developing deep relational intuition so that every problem — even unfamiliar ones — yields to the same underlying principles.

### 8.3 The Skills Gap Companies Rarely Articulate

Most job descriptions list SQL as a bullet point without specifying what level they actually need. In practice:

- A company asking for "SQL proficiency" in a junior role means: can you write a GROUP BY query and not make obvious mistakes.
- A company asking for "strong SQL skills" in a mid-level role means: can you write multi-table queries involving window functions and subqueries, quickly and correctly.
- A company asking for "expert SQL" in a senior or engineering role means: do you understand the database as a system, and can you make it perform under real workloads.

Calibrate your preparation to the actual role, not the vague language in job descriptions. When in doubt, prepare one level above what the description implies — depth is rarely penalized, and it signals genuine expertise.

---

## Appendix: Quick Reference

### Logical Query Processing Order

```
1. FROM
2. JOIN (ON conditions evaluated here)
3. WHERE
4. GROUP BY
5. HAVING
6. SELECT (expressions evaluated here, including window functions)
7. DISTINCT
8. ORDER BY
9. LIMIT / TOP / FETCH FIRST
```

> **Why this matters:** You cannot reference a SELECT alias in a WHERE clause because WHERE runs before SELECT. You cannot filter on a window function result in WHERE or HAVING — wrap the query in a CTE or subquery first.

### Join Type Behavior Summary

| Join Type       | Left rows with no match      | Right rows with no match    |
| --------------- | ---------------------------- | --------------------------- |
| INNER JOIN      | Excluded                     | Excluded                    |
| LEFT JOIN       | Included (right cols = NULL) | Excluded                    |
| RIGHT JOIN      | Excluded                     | Included (left cols = NULL) |
| FULL OUTER JOIN | Included (right cols = NULL) | Included (left cols = NULL) |
| CROSS JOIN      | All combinations             | All combinations            |

### Window Function Frame Defaults

When `ORDER BY` is specified inside `OVER()` without an explicit frame:

- Default frame: `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`
- This means: all rows up to and including the current row's value (not the current physical row)
- For running totals where ORDER BY column has duplicates, use `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`

### NULL Behavior Rules

- `NULL = NULL` → `NULL` (not TRUE)
- `NULL <> NULL` → `NULL`
- `NULL + anything` → `NULL`
- `NULL AND TRUE` → `NULL`
- `NULL OR TRUE` → `TRUE`
- `NOT IN (1, 2, NULL)` → returns no rows (any comparison with NULL is NULL, which is falsy)
- `COUNT(*)` counts NULLs; `COUNT(col)` does not

### Normalization Quick Reference

| Normal Form | What it eliminates                          |
| ----------- | ------------------------------------------- |
| 1NF         | Repeating groups, non-atomic values         |
| 2NF         | Partial dependencies on composite keys      |
| 3NF         | Transitive dependencies (non-key → non-key) |
| BCNF        | Every determinant is a candidate key        |
| 4NF         | Multi-valued dependencies                   |
| 5NF         | Join dependencies                           |

---

_Curriculum version 1.0 — structured for self-directed study_
_Estimated total curriculum scope: 570 – 1,030 focused study hours from beginner to expert_
