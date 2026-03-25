# 🔒 Isolation Levels in MySQL / MariaDB

> An interactive web-based presentation on transaction isolation — built as a DBMS course project, Spring 2026 · SUNY Old Westbury.

![HTML](https://img.shields.io/badge/HTML-E34F26?style=flat&logo=html5&logoColor=white)
![CSS](https://img.shields.io/badge/CSS-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-green?style=flat)

---

## 📌 Overview

This project is an interactive 10-slide presentation exploring **transaction isolation** in MySQL and MariaDB — one of the four core ACID properties in database management systems.

Instead of a traditional slide deck, we built a **GitHub-styled interactive webpage** (`index.html`) that you can open directly in any browser. Each slide features animated step-through demos of concurrency anomalies, a full isolation level matrix, real SQL syntax, and speaker notes so you know exactly what to say during the presentation.

---

## 🚀 How to Run

**Option 1 — Clone and open:**
```bash
git clone https://github.com/your-username/isolation-levels-mysql.git
cd isolation-levels-mysql
open index.html
```

**Option 2 — Download only:**
1. Download `index.html`
2. Open it in any modern browser — Chrome, Firefox, Edge, or Safari
3. No server, no install, no dependencies needed

**Navigation:**
- Use `←` `→` arrow keys on your keyboard
- Or click the **prev / next** buttons in the top bar
- Click any tab to jump directly to that slide

---

## 🗂️ Slides

| # | Tab | Content | Time |
|---|-----|---------|------|
| 1 | `index.html` | Title — project intro, topic overview | ~1 min |
| 2 | `what-is-isolation.md` | What isolation means, ACID breakdown | ~1 min |
| 3 | `concurrency-problem.md` | The banking race condition — why isolation matters | ~1 min |
| 4 | `dirty-read.ts` | Anomaly 1: Dirty Read — interactive stepper + timeline | ~1 min |
| 5 | `non-repeatable-read.ts` | Anomaly 2: Non-Repeatable Read — interactive stepper + timeline | ~1 min |
| 6 | `phantom-read.ts` | Anomaly 3: Phantom Read — interactive stepper + timeline | ~1 min |
| 7 | `isolation-matrix.sql` | All 4 levels vs all 3 anomalies — full comparison matrix | ~1 min |
| 8 | `tradeoffs.md` | Safety vs performance — why not always use Serializable | ~1 min |
| 9 | `mysql-specifics.sql` | InnoDB, MVCC, gap locks, SQL syntax | ~1 min |
| 10 | `summary.md` | Key takeaways + quick reference guide | ~1 min |

**Total: ~10 minutes**

---

## 🧪 The Three Anomalies

### 1. Dirty Read
Reading uncommitted data from another transaction. If that transaction rolls back, you acted on data that never officially existed.

```sql
-- T2 writes but hasn't committed yet
UPDATE accounts SET balance = 800 WHERE id = 1;  -- T2, uncommitted

-- T1 reads the dirty value
SELECT balance FROM accounts WHERE id = 1;  -- T1 sees 800 ← WRONG

-- T2 rolls back
ROLLBACK;  -- balance reverts to 1000, but T1 already used 800
```
**Prevented by:** `READ COMMITTED` and above

---

### 2. Non-Repeatable Read
Reading the same row twice in one transaction and getting different values because another transaction modified it in between.

```sql
-- T1 reads price = 50
SELECT price FROM products WHERE id = 1;  -- returns 50

-- T2 updates and commits
UPDATE products SET price = 75 WHERE id = 1;
COMMIT;

-- T1 reads again — different result!
SELECT price FROM products WHERE id = 1;  -- returns 75, not 50
```
**Prevented by:** `REPEATABLE READ` and above

---

### 3. Phantom Read
Re-running a range query in one transaction and getting a different set of rows because another transaction inserted or deleted matching rows.

```sql
-- T1 counts orders
SELECT COUNT(*) FROM orders WHERE status = 'pending';  -- returns 5

-- T2 inserts a new order and commits
INSERT INTO orders (status) VALUES ('pending');
COMMIT;

-- T1 counts again — different result!
SELECT COUNT(*) FROM orders WHERE status = 'pending';  -- returns 6
```
**Prevented by:** `SERIALIZABLE` only

---

## 📊 Isolation Level Matrix

| Isolation Level | Dirty Read | Non-Repeatable | Phantom Read | Performance | Use Case |
|----------------|------------|----------------|--------------|-------------|----------|
| READ UNCOMMITTED | ✗ possible | ✗ possible | ✗ possible | Highest | Analytics only |
| READ COMMITTED | ✓ prevented | ✗ possible | ✗ possible | High | Reporting, PostgreSQL default |
| **REPEATABLE READ ★** | ✓ prevented | ✓ prevented | ✗ possible* | Medium | **MySQL/MariaDB default** |
| SERIALIZABLE | ✓ prevented | ✓ prevented | ✓ prevented | Lowest | Banking, finance, audit |

> ★ MySQL and MariaDB default
> \* InnoDB gap locks partially prevent phantom reads at REPEATABLE READ — stronger than the SQL standard requires

---

## 🛠️ MySQL / MariaDB Quick Reference

```sql
-- Check current isolation level
SELECT @@transaction_isolation;

-- Set for the current session
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;

-- Set globally for all new sessions
SET GLOBAL TRANSACTION ISOLATION LEVEL READ COMMITTED;

-- Use inside a transaction
START TRANSACTION;
SELECT balance FROM accounts WHERE id = 1;
-- ... your application logic ...
COMMIT;
```

**All four levels:**
```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;   -- MySQL default
SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

> **Note:** On MySQL 5.7 and older, or MariaDB, use `@@tx_isolation` instead of `@@transaction_isolation`

---

## 💡 Key Takeaways

1. **Isolation = the "I" in ACID** — each transaction runs as if it's the only one, even when thousands run simultaneously
2. **Three anomalies** can occur without proper isolation: dirty reads, non-repeatable reads, and phantom reads — each prevented at a different level
3. **Four isolation levels** — each one adds one more protection at the cost of some performance
4. **MySQL defaults to Repeatable Read** — powered by InnoDB + MVCC, it prevents the two most common anomalies while staying fast enough for production workloads
5. **There is no perfect level** — higher isolation means safer data but lower throughput; choose based on what your application actually needs

---

## 🏗️ Tech Stack

| Technology | Purpose |
|-----------|---------|
| HTML5 | Structure and content |
| CSS3 | GitHub light theme, animations, layout |
| Vanilla JavaScript | Slide navigation, animated steppers |
| Google Fonts | JetBrains Mono + Inter |

No frameworks. No build tools. No dependencies. Just open `index.html`.

---

## 📁 File Structure

```
isolation-levels-mysql/
├── index.html       ← the entire presentation (open this)
└── README.md        ← this file
```

---

## 👥 Team

Built by a team of 2 for a DBMS course project.
**Course:** Database Management Systems — Spring 2026
**Institution:** SUNY Old Westbury, School of Business

---

## 📄 License

This project is open source under the [MIT License](LICENSE).

---

<div align="center">
  <sub>Built with ☕ and too many SQL queries &nbsp;·&nbsp; Spring 2026</sub>
</div>
