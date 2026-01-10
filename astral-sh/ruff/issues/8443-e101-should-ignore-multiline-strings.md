---
number: 8443
title: E101 should ignore multiline strings
type: issue
state: closed
author: konstin
labels:
  - linter
assignees: []
created_at: 2023-11-02T09:51:11Z
updated_at: 2023-11-02T12:55:21Z
url: https://github.com/astral-sh/ruff/issues/8443
synced_at: 2026-01-10T01:22:48Z
---

# E101 should ignore multiline strings

---

_Issue opened by @konstin on 2023-11-02 09:51_

In the code below, the whitespace ahead of a and b contains tabs and spaces:
```python
from pathlib import Path


def b():
    Path("my_data.txt").write_text("""
    	a
    		b
    """)

```
This raises "E101 Indentation contains mixed spaces and tabs", even though this is not indentation but the contents of a multiline string where the leading indentation is user data and not python code. We should ignore multiline strings for E101.

---

_Label `linter` added by @MichaReiser on 2023-11-02 10:01_

---

_Comment by @charliermarsh on 2023-11-02 12:55_

I believe this is a dupe of https://github.com/astral-sh/ruff/issues/7906 where I was asking for a situation in which this would be used in practice.

---

_Closed by @charliermarsh on 2023-11-02 12:55_

---
