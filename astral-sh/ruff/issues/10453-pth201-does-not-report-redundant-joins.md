```yaml
number: 10453
title: "`PTH201` does not report redundant joins"
type: issue
state: closed
author: tjkuson
labels:
  - fixes
  - linter
assignees: []
created_at: 2024-03-18T11:01:14Z
updated_at: 2024-12-30T03:31:37Z
url: https://github.com/astral-sh/ruff/issues/10453
synced_at: 2026-01-10T11:09:52Z
```

# `PTH201` does not report redundant joins

---

_Issue opened by @tjkuson on 2024-03-18 11:01_

Using `path-constructor-current-directory` (`PTH201`),

```python
import pathlib


p = pathlib.Path(".") / "foo.txt"
```

is auto-fixed to

```python
import pathlib


p = pathlib.Path() / "foo.txt"
```

by invoking `ruff check --select PTH201 --fix --isolated /tmp/cwd.py` using version 0.3.2.

The fix is correct. However, it would be more useful if it included the rest of the path (following the motivation of the rule, which is to reduce unnecessary explicit CWD specification). For example,

```python
import pathlib


p = pathlib.Path("foo.txt")
```

Search terms: PTH201, current working directory, pathlib


---

_Renamed from "`PTH201` autofix ignores the rest of the path" to "`PTH201` autofix does not remove redundant joins" by @tjkuson on 2024-03-18 11:05_

---

_Renamed from "`PTH201` autofix does not remove redundant joins" to "`PTH201` does not report redundant joins" by @tjkuson on 2024-03-18 11:07_

---

_Label `rule` added by @AlexWaygood on 2024-03-18 11:59_

---

_Label `linter` added by @AlexWaygood on 2024-03-18 12:00_

---

_Label `rule` removed by @MichaReiser on 2024-03-18 15:59_

---

_Label `fixes` added by @MichaReiser on 2024-03-18 15:59_

---

_Closed by @charliermarsh on 2024-12-30 03:31_

---

_Closed by @charliermarsh on 2024-12-30 03:31_

---
