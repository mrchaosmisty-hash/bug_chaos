# Database & SQL Fundamentals for Security Researchers

Databases persist application state. When applications fail to separate user data from database query instructions, injection flaws emerge.

---

## 1. Relational Database Concepts & SQL Structure

Relational Database Management Systems (RDBMS) like PostgreSQL, MySQL, SQLite, and Microsoft SQL Server store structured records across tables with defined schemas.

### Core SQL Statements
```sql
-- Read data
SELECT id, username, email, role FROM users WHERE id = 42;

-- Insert new records
INSERT INTO users (username, password_hash, role) VALUES ('alice', '$2b$12$...', 'member');

-- Modify existing records
UPDATE users SET email = 'alice@example.com' WHERE id = 42;

-- Delete records
DELETE FROM sessions WHERE expired_at < NOW();
```

---

## 2. The Root Cause of SQL Injection

SQL Injection occurs when user input is **concatenated directly into a SQL query string**, causing the database engine to interpret untrusted characters (`'`, `"`, `--`, `;`, `UNION`) as SQL syntax rather than literal data.

### Vulnerable Code Example (Direct Concatenation)
```python
# Unsafe: user_input is interpolated directly into the query
query = f"SELECT * FROM products WHERE category = '{user_input}'"
cursor.execute(query)
```
If `user_input` is `Gifts' OR '1'='1' -- `, the executed query becomes:
```sql
SELECT * FROM products WHERE category = 'Gifts' OR '1'='1' -- '
```
The `'1'='1'` condition evaluates to true for every row in the table, bypassing intended filtering.

---

## 3. The Definitive Defense: Parameterized Queries

Parameterized queries (also known as Prepared Statements) ensure the database compiles the query structure *before* binding user data. Untrusted inputs are strictly handled as data literals, never as executable SQL commands.

### Secure Code Example (Parameterized)
```python
# Safe: The database engine separates query structure from data parameters
query = "SELECT * FROM products WHERE category = %s"
cursor.execute(query, (user_input,))
```
Even if `user_input` contains `' OR '1'='1`, the engine searches for a category whose literal name is the entire malicious string.
