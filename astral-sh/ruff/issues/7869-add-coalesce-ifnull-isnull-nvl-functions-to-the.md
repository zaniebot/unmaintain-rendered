```yaml
number: 7869
title: Add coalesce/ifnull/isnull/nvl functions to the exclusion list of FBT003 (boolean positional args)
type: issue
state: closed
author: balodja
labels: []
assignees: []
created_at: 2023-10-09T13:00:17Z
updated_at: 2023-10-09T18:50:52Z
url: https://github.com/astral-sh/ruff/issues/7869
synced_at: 2026-01-12T15:54:47Z
```

# Add coalesce/ifnull/isnull/nvl functions to the exclusion list of FBT003 (boolean positional args)

---

_@balodja_

Those functions are sqlalchemy counterparts of commonly used sql functions. Typical uses include using booleans in positional arguments as default values. So I suggest to add them to the exclusion list of boolean positional arguments rule.

Just like it was done for is_ and is_not before: https://github.com/astral-sh/ruff/pull/6307

Example:
```python
import sqlalchemy as sa

tbl = sa.Table(
    "mytable",
    sa.Column("id", sa.Integer, primary_key=True),
    sa.Column("valid", sa.Bool, nullable=True),
)

is_valid = sa.func.coalesce(tbl.c.valid, False)
```

Output:
```
> ruff check test.py 
test.py:9:42: FBT003 Boolean positional value in function call
Found 1 error.
```

---

_Renamed from "Add coalesce/ifnull/isnull/nvl functions to the exceptions of FBT003" to "Add coalesce/ifnull/isnull/nvl functions to the exceptions of FBT003 (boolean positional args)" by @balodja on 2023-10-09 13:00_

---

_Renamed from "Add coalesce/ifnull/isnull/nvl functions to the exceptions of FBT003 (boolean positional args)" to "Add coalesce/ifnull/isnull/nvl functions to the exclusion list of FBT003 (boolean positional args)" by @balodja on 2023-10-09 13:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-09 18:39_

---

_Closed by @charliermarsh on 2023-10-09 18:50_

---
