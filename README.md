# 🔒 Isolation Levels in MySQL / MariaDB

> An interactive web-based presentation on transaction isolation — built for a DBMS course project, Spring 2026.

![HTML](https://img.shields.io/badge/HTML-E34F26?style=flat&logo=html5&logoColor=white)
![CSS](https://img.shields.io/badge/CSS-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-green?style=flat)

---

## 📌 Overview

This project is an interactive presentation exploring **transaction isolation** in MySQL and MariaDB — one of the four core ACID properties in database management systems.

Instead of a traditional slide deck, we built a **GitHub-styled interactive webpage** that lets you step through concurrency anomalies one action at a time, view a complete isolation level matrix, and follow along with speaker notes on every slide.

---

## 🎯 What's Covered

| Topic | Description |
|-------|-------------|
| **What is Isolation?** | The "I" in ACID — why it matters in multi-user databases |
| **Concurrency Anomalies** | Dirty reads, non-repeatable reads, and phantom reads |
| **The 4 Isolation Levels** | READ UNCOMMITTED → READ COMMITTED → REPEATABLE READ → SERIALIZABLE |
| **Tradeoffs** | Safety vs performance — why not everything needs SERIALIZABLE |
| **MySQL Specifics** | InnoDB, MVCC, gap locks, and how to set isolation in SQL |

---

## ✨ Features

- **Interactive anomaly stepper** — click through dirty reads, non-repeatable reads, and phantom reads step by step with animated T1/T2 transaction lanes
- **Isolation level matrix** — color-coded table showing which level prevents which anomaly
- **Speaker notes** — every slide has a short paragraph written in plain English so you know exactly what to say
- **GitHub light theme** — clean, readable UI with monospace fonts, syntax-highlighted code blocks, and tab-style navigation
- **Keyboard navigation** — use `←` `→` arrow keys to move between slides
- **Zero dependencies** — no frameworks, no server, no install needed

---

## 🚀 How to Run

**Option 1 — Just open it:**
```bash
# Clone the repo
git clone https://github.com/your-username/isolation-levels-mysql.git

# Open in browser
open isolation-final.html
```

**Option 2 — Download and open:**
1. Download `isolation-final.html`
2. Open it in any modern browser (Chrome, Firefox, Edge, Safari)
3. That's it — no server required

---

## 🗂️ Slides

| # | File Tab | Content |
|---|----------|---------|
| 1 | `README.md` | Title slide — project intro and tags |
| 2 | `what-is-isolation.md` | ACID breakdown + banking race condition example |
| 3 | `anomalies.ts` | Interactive stepper for all 3 concurrency anomalies |
| 4 | `isolation-matrix.sql` | Full 4-level comparison matrix |
| 5 | `tradeoffs.md` | Safety vs performance + MVCC explained |
| 6 | `summary.sql` | SQL syntax + key takeaways |

---

## 🧪 The Three Anomalies

### Dirty Read
T1 reads data written by T2 before T2 commits. If T2 rolls back, T1 acted on data that never really existed.

```
T2 writes balance = $800  (uncommitted)
T1 reads balance = $800   ← dirty read
T2 rolls back
T1 used ghost data 💀
```
**Prevented by:** READ COMMITTED and above

---

### Non-Repeatable Read
T1 reads the same row twice within one transaction and gets different values because T2 updated it in between.

```
T1 reads price = $50
T2 updates price = $75, commits
T1 reads price = $75  ← not $50 anymore!
```
**Prevented by:** REPEATABLE READ and above

---

### Phantom Read
T1 runs the same range query twice and sees different rows because T2 inserted new records in between.

```
T1 counts orders = 5
T2 inserts a new order, commits
T1 counts orders = 6  ← phantom row appeared!
```
**Prevented by:** SERIALIZABLE only

---

## 📊 Isolation Level Matrix

| Isolation Level | Dirty Read | Non-Repeatable | Phantom Read | Performance |
|----------------|------------|----------------|--------------|-------------|
| READ UNCOMMITTED | ✗ possible | ✗ possible | ✗ possible | Highest |
| READ COMMITTED | ✓ prevented | ✗ possible | ✗ possible | High |
| **REPEATABLE READ ★** | ✓ prevented | ✓ prevented | ✗ possible | Medium |
| SERIALIZABLE | ✓ prevented | ✓ prevented | ✓ prevented | Lowest |

> ★ MySQL and MariaDB default — powered by InnoDB + MVCC

---

## 🛠️ MySQL / MariaDB Quick Reference

```sql
-- Check your current isolation level
SELECT @@transaction_isolation;

-- Set isolation level for the current session
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;

-- Use it in a transaction
START TRANSACTION;
SELECT balance FROM accounts WHERE id = 1;
-- ... your logic ...
COMMIT;
```

**Available levels:**
```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;   -- default
SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

> **Note:** On older MySQL versions and MariaDB, use `@@tx_isolation` instead of `@@transaction_isolation`

---

## 💡 Key Takeaways

1. **Isolation = the "I" in ACID** — each transaction runs as if it's the only one, even when hundreds run simultaneously
2. **Three anomalies** can happen without proper isolation: dirty reads, non-repeatable reads, and phantom reads
3. **Four levels** give you control over the tradeoff between data safety and query performance
4. **MySQL defaults to Repeatable Read** — a smart balance for most web applications using InnoDB and MVCC
5. **Not every app needs Serializable** — choose the level that fits your use case

---

## 🏗️ Tech Stack

- **HTML5** — structure and content
- **CSS3** — GitHub light theme, animations, layout
- **Vanilla JavaScript** — slide navigation, interactive steppers, anomaly animations
- **Google Fonts** — JetBrains Mono + Inter
- No frameworks, no build tools, no dependencies

---

## 👥 Team

Built by a team of 2 for a DBMS course project — Spring 2026, SUNY Old Westbury.

---

## 📄 License

This project is open source under the [MIT License](LICENSE).

---

<div align="center">
  <sub>Built with ☕ and too many SQL queries</sub>
</div>
