**Tags:**
#programming #programming-note #go #database 
**Docs:**
[DB.Prepare](https://pkg.go.dev/database/sql#DB.Prepare) | [DB.Query / DB.QueryRows](https://pkg.go.dev/database/sql#DB.Query)

---

## ✅ 1. Why `db.Prepare` Was Used in `AddTodo`

### `AddTodo`:

```go
stmt, err := db.DB.Prepare("INSERT INTO todos (title) VALUES (?)")
...
stmt.Exec(title)
```

**Why?**

* `INSERT`, `UPDATE`, `DELETE` operations are **write-heavy**
* When you plan to **reuse the same query** multiple times (e.g., inserting 100 rows), `Prepare` is more efficient.
* Helps separate query and execution — good for batching and transactions.

---

## ✅ 2. Why `db.Query` / `db.QueryRow` is Used in `GetAllTodos` and `GetTodoByID`

```go
rows, err := db.DB.Query("SELECT ...")
```

```go
row := db.DB.QueryRow("SELECT ... WHERE id = ?", id)
```

**Why?**

* `SELECT` operations are usually **lightweight** and run only once.
* These functions are **convenient**, safe, and efficient for single use.
* They internally prepare, execute, and close the statement — less boilerplate.

---

## 🧠 Summary: When to Use Which

| Method                       | When to Use                                                                                   | Notes                                                              |
| ---------------------------- | --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| `db.Prepare` + `stmt.Exec()` | When you're executing the **same SQL multiple times** or inside a **loop** or **transaction** | Improves performance and clarity for repeated use                  |
| `db.Query`, `db.QueryRow`    | For **one-off SELECTs** (e.g., read once and done)                                            | More concise; internally prepares and closes the statement for you |
| `db.Exec`                    | One-off INSERT/UPDATE/DELETE                                                                  | Safe to use if you're not reusing the statement                    |

---

## ✅ Example Use Case for `Prepare`

### Insert many todos:

```go
stmt, _ := db.DB.Prepare("INSERT INTO todos (title) VALUES (?)")
defer stmt.Close()

for _, t := range todos {
    stmt.Exec(t.Title)
}
```

Way more efficient than preparing a new statement on every iteration.

---

### 🚀 Best Practice Rule of Thumb

* Use `Prepare` **only when reusing a statement**.
* Use `Query`/`QueryRow`/`Exec` for **one-liners**.

Let me know if you want to see a benchmark or performance difference in action!
