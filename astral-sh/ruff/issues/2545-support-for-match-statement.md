```yaml
number: 2545
title: Support for match statement
type: issue
state: closed
author: AxTheB
labels: []
assignees: []
created_at: 2023-02-03T15:00:11Z
updated_at: 2023-02-22T00:04:30Z
url: https://github.com/astral-sh/ruff/issues/2545
synced_at: 2026-01-10T11:09:45Z
```

# Support for match statement

---

_Issue opened by @AxTheB on 2023-02-03 15:00_

README  claims python3.11 compatibility, but ruff fails at match statements:

```
"""This is working match statement"""
match "data":
    case "data":
        print("Match!")
```

When run in python, produces "Match!". Pylint says its 10.00/10, ruff fails:
```
ruff -v test1.py 
[2023-02-03][15:55:04][globset][DEBUG] built glob set; 19 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
[2023-02-03][15:55:04][ruff::commands][DEBUG] Identified files to lint in: 611.569µs
error: Failed to parse test1.py: invalid syntax. Got unexpected token "data" at line 2 column 7
[2023-02-03][15:55:04][ruff::commands][DEBUG] Checked 1 files in: 1.152863ms
test1.py:2:8: E999 SyntaxError: invalid syntax. Got unexpected token "data"
Found 1 error.
```

There is no pyproject.toml or other setup. `ruff --version` says `ruff 0.0.240`



---

_Comment by @Jackenmen on 2023-02-03 15:01_

Duplicate of #282 

---

_Closed by @AxTheB on 2023-02-03 15:10_

---

_Comment by @charliermarsh on 2023-02-22 00:04_

Fixed as of v0.0.250.

---
