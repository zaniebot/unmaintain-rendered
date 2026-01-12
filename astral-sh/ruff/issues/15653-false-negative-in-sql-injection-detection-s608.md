```yaml
number: 15653
title: False negative in SQL injection detection (S608) when selecting multiple columns across multiple lines
type: issue
state: closed
author: aripollak
labels:
  - bug
assignees: []
created_at: 2025-01-21T18:55:15Z
updated_at: 2025-01-22T04:50:23Z
url: https://github.com/astral-sh/ruff/issues/15653
synced_at: 2026-01-12T15:54:54Z
```

# False negative in SQL injection detection (S608) when selecting multiple columns across multiple lines

---

_@aripollak_

Here's a file that demonstrates the issue when checked with `ruff check --select S608 test.py`:

```py
user_input = "SELECT * FROM bad_stuff"
bad_detected = f"""
    SELECT *, foo
    FROM ({user_input}) raw
"""

bad_undetected = f"""
    SELECT *,
        foo
    FROM ({user_input}) raw
"""
```

I'd expect both `bad_detected` and `bad_undetected` to be warned about, but this only warns with `test.py:2:16: S608 Possible SQL injection vector through string-based query construction`.

This used to work correctly in ruff 0.4, but broke at some point between then and ruff 0.9.2.

---

_Comment by @InSyncWithFoo on 2025-01-21 19:23_

This regression first appeared in [0.7.0](https://github.com/astral-sh/ruff/releases/tag/0.7.0) via #13574. The regex in question was changed from:

```text
(?i)\b(select\s.+\sfrom\s|delete\s+from\s|(insert|replace)\s.+\svalues\s|update\s.+\sset\s)
```

...to:

```text
(?i)\b(select\s+.*\s+from\s|delete\s+from\s|(insert|replace)\s+.*\s+values\s|update\s+.*\s+set\s)
```

For comparison, here's the corresponding regex [from `flake8-bandit`](https://github.com/PyCQA/bandit/blob/4ac55dfaaacf2083497831294b2dcd7e679f8428/bandit/plugins/injection_sql.py#L74-L80) (it [hasn't changed](https://github.com/PyCQA/bandit/blob/ef0090fd2a56dd420d7578a631e5e24bca952a49/bandit/plugins/injection_sql.py#L74-L80) since then):

```python
SIMPLE_SQL_RE = re.compile(
    r"(select\s.*from\s|"
    r"delete\s+from\s|"
    r"insert\s+into\s.*values\s|"
    r"update\s.*set\s)",
    re.IGNORECASE | re.DOTALL,
)
```

---

_Label `bug` added by @dhruvmanila on 2025-01-22 04:43_

---

_Closed by @dhruvmanila on 2025-01-22 04:50_

---
