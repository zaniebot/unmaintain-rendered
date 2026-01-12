```yaml
number: 2698
title: "Implement bandit's 'hardcoded-sql-expressions' S608"
type: pull_request
state: merged
author: mattoberle
labels:
  - rule
assignees: []
merged: true
base: main
head: flake8-bandit/S608
created_at: 2023-02-09T23:40:33Z
updated_at: 2023-02-10T22:50:17Z
url: https://github.com/astral-sh/ruff/pull/2698
synced_at: 2026-01-12T04:52:00Z
```

# Implement bandit's 'hardcoded-sql-expressions' S608

---

_Pull request opened by @mattoberle on 2023-02-09 23:40_

This is an attempt to implement `bandit` rule `B608` (renamed here `S608`).
- https://bandit.readthedocs.io/en/latest/plugins/b608_hardcoded_sql_expressions.html

The rule inspects strings constructed via `+`, `%`, `.format`, and `f""`.

- `+` and `%` via `BinOp`
- `.format` via `Call`
- `f""` via `JoinedString`

Any SQL-ish strings that use Python string formatting are flagged.

The expressions and targeted expression types for the rule come from here:
- https://github.com/PyCQA/bandit/blob/7104b336d31a691b145d4a5cc38d6c1d47c6046f/bandit/plugins/injection_sql.py

> Related Issue: https://github.com/charliermarsh/ruff/issues/1646

---

_@charliermarsh reviewed on 2023-02-10 00:18_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:83 on 2023-02-10 00:18_

Confused why the Bandit source has this `execute` and `executemany` detection, but then just throws out the returned value via `if _check_string(val[1])`? Anyway...

---

_Review comment by @mattoberle on `crates/ruff/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:83 on 2023-02-10 00:21_

I think it has to do with bandit's concept of `confidence`, where vulnerability `confidence` is higher when the string formatting is detected within an `execute/executemany` call.

---

_@mattoberle reviewed on 2023-02-10 00:22_

---

_Merged by @charliermarsh on 2023-02-10 00:28_

---

_Closed by @charliermarsh on 2023-02-10 00:28_

---

_Label `rule` added by @charliermarsh on 2023-02-10 00:28_

---

_@not-my-profile reviewed on 2023-02-10 22:50_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:26 on 2023-02-10 22:50_

This section implies that sanitizing inputs is alright ... when actually directly interpolating user input into SQL queries should always be avoided (no matter if sanitized or not).

You should instead use "parameter binding" ... also called "server-side binding", where the parameters are sent separately from the query to the server, see e.g. [psycopg2 server-side binding](https://www.psycopg.org/psycopg3/docs/basic/from_pg2.html#server-side-binding).

So I think it would make sense to update this section accordingly.

---
