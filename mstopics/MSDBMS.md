# MSDBMS — Databases (Microsoft)

> Weightage: ~5–7%. Freshers get concepts + a modeling scenario; SDE-2 gets SQL vs NoSQL + schema changes at scale.
> Atomicity was specifically reported in an intern round.

## 1. ACID
- **Atomicity:** all or nothing. If a transfer debits A but crashes before crediting B, the whole thing rolls back.
- **Consistency:** a transaction moves the DB from one valid state to another (constraints still hold).
- **Isolation:** concurrent transactions don't see each other's half-done work.
- **Durability:** once committed, it survives a crash.

**How each is implemented (the follow-up):** Atomicity and durability come from the **write-ahead log (WAL)**: write the change to the log first, so you can undo or redo after a crash. Isolation comes from **locks or MVCC** (multiple row versions, so readers don't block writers).

## 2. Isolation levels and anomalies
| Level | Dirty read | Non-repeatable read | Phantom |
|---|---|---|---|
| Read Uncommitted | ✔ possible | ✔ | ✔ |
| Read Committed | ✘ | ✔ | ✔ |
| Repeatable Read | ✘ | ✘ | ✔ |
| Serializable | ✘ | ✘ | ✘ |
- Dirty read: you read data another transaction hasn't committed (and might roll back).
- Non-repeatable read: you read the same row twice and get different values.
- Phantom: you rerun the same query and new rows have appeared.
**Concept:** Stronger isolation means more locking means less concurrency. It's a trade-off, not "always use Serializable."

## 3. Normalization
- **1NF:** atomic values, no repeating groups (no "phones = 98xx, 97xx" in one cell).
- **2NF:** 1NF + no partial dependency (non-key columns depend on the *whole* composite key).
- **3NF:** 2NF + no transitive dependency (non-key doesn't depend on another non-key, e.g. zip -> city).
- **BCNF:** every determinant is a candidate key.
**Why bother:** It removes update, insert, and delete anomalies.
**Why sometimes not:** Denormalize for read-heavy systems to avoid joins. Say this; it shows judgment.

## 4. Indexing: how it works and when it hurts
**Answer:** An index is usually a **B+ tree** on a column. It's sorted, shallow (3–4 levels for millions of rows), and its leaf nodes are linked, so range scans are fast. Lookups go from O(n) to O(log n).
- **Clustered index:** the table rows themselves are stored in index order. Only one per table (usually the primary key).
- **Non-clustered index:** a separate structure pointing back to the rows. Many are allowed.
**Costs:** every INSERT/UPDATE/DELETE must also update every index, plus extra storage. Don't index low-cardinality columns like gender.
**Why B+ tree and not a hash index?** Hash gives O(1) equality lookups but can't do ranges or ORDER BY.

## 5. SQL vs NoSQL (SDE-2 report)
| | SQL | NoSQL |
|---|---|---|
| Schema | Fixed, enforced | Flexible |
| Scaling | Mostly vertical (sharding is painful) | Built to scale horizontally |
| Consistency | Strong, ACID | Often eventual (tunable) |
| Good for | Money, orders, relationships | Huge volume, flexible docs, logs, feeds, caches |
**"Payment use case, which DB?"** SQL. You need ACID; a payment that's "eventually consistent" means someone loses money. NoSQL fits things like an activity feed, product catalog, telemetry, or sessions.

## 6. "Add a column to a table with GBs of data?" (SDE-2 report)
**Answer:** Yes. In many modern engines, adding a **nullable column or one with a constant default** is a metadata-only change and nearly instant. Adding a NOT NULL column with a computed value, or changing a column's type, may rewrite the whole table and lock it.
**Safe production approach:** add it as nullable, then backfill in small batches, then add the constraint. Or use online schema-change tools. Never lock a production table for an hour.

## 7. The SQL query they asked
"Top 5 highest-paid employees living in Bangalore with more than 3 years at the company."
```sql
SELECT name, salary
FROM employees
WHERE city = 'Bangalore'
  AND join_date <= CURRENT_DATE - INTERVAL '3 years'
ORDER BY salary DESC
LIMIT 5;          -- SQL Server: SELECT TOP 5 ... (this is Microsoft, know both)
```
**Classic follow-ups:** Nth highest salary (`DENSE_RANK()` or `OFFSET`), employees earning more than their manager (self-join), department-wise max (`GROUP BY` or a window function).

## 8. Joins
INNER = matches only. LEFT = all of left + matches (NULL otherwise). RIGHT = the mirror. FULL = everything. CROSS = every combination. SELF = a table joined with itself (employee -> manager).

## 9. Keys
Primary (unique + not null, one per table), Candidate (any minimal unique set), Foreign (references another table's primary key, enforces referential integrity), Composite (a key made of several columns), Super key (any unique set, not necessarily minimal).

## 10. DELETE vs TRUNCATE vs DROP
DELETE: removes rows (can filter with WHERE), logged row by row, can roll back. TRUNCATE: removes all rows, minimal logging, fast, resets identity. DROP: removes the table itself.

## 11. Data-modeling scenario (Dec 2025 intern report: "model this data", no SQL)
**How to answer any of these:**
1. List the entities (nouns): User, Course, Enrollment...
2. List the relationships and cardinality: one-to-many becomes a foreign key on the "many" side; many-to-many becomes a junction table.
3. Pick keys and the indexes the main queries need.
4. Name the access pattern ("we mostly read X by Y") and pick storage based on it: relational if relationships and consistency matter, key-value or document if it's high-volume lookups by ID.
**Example:** a library. Book(id, title), Member(id, name), Loan(book_id FK, member_id FK, due_date). Loan is the junction table, with an index on member_id for "my loans."

## 12. Replication and sharding (SDE-1 L60 design round)
- **Master–slave replication:** writes go to the master; it ships its log to replicas, which replay it. Reads go to replicas. **Catch:** replication lag, so a read right after your own write might show stale data. Fix: send "critical reads" to the master. (That candidate kept two connection pools, one for the master, one for replicas.)
- **Sharding:** split rows across machines by a key (user_id hash). Scales writes. Cost: cross-shard joins and transactions become painful, and choosing a bad key creates hot shards.

## 13. Deadlocks in a database
Two transactions lock rows in opposite order. The DB detects the cycle and kills one (the "victim"). Fix: access tables and rows in a consistent order, keep transactions short, and index properly so you lock fewer rows.
