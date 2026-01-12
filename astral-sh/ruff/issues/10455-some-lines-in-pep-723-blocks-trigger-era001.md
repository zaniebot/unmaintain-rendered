```yaml
number: 10455
title: Some lines in PEP 723 blocks trigger ERA001
type: issue
state: closed
author: zeevro
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2024-03-18T12:50:06Z
updated_at: 2024-03-19T12:12:53Z
url: https://github.com/astral-sh/ruff/issues/10455
synced_at: 2026-01-12T15:54:50Z
```

# Some lines in PEP 723 blocks trigger ERA001

---

_@zeevro_

Ruff (version 0.3.3) treats some lines from PEP 723 blocks as commented-out code.

Example:
```python
# /// script
# requires-python = ">=3.11"
# dependencies = [
#   "requests<3",
#   "rich",
# ]
# ///
```

```
$ ruff check --select=ERA001 pep723.py
pep723.py:3:1: ERA001 Found commented-out code
pep723.py:6:1: ERA001 Found commented-out code
Found 2 errors.
```


---

_Label `configuration` added by @MichaReiser on 2024-03-18 13:51_

---

_Label `core` added by @MichaReiser on 2024-03-18 13:51_

---

_Label `rule` added by @MichaReiser on 2024-03-18 13:51_

---

_Label `configuration` removed by @MichaReiser on 2024-03-18 13:52_

---

_Label `core` removed by @MichaReiser on 2024-03-18 13:52_

---

_Label `help wanted` added by @MichaReiser on 2024-03-18 13:54_

---

_Comment by @MichaReiser on 2024-03-18 13:54_

Thanks for reporting. I agree that `ERA001` should skip over these comments. This also brings up that ruff should probably support configurations in the script section, but we can track this separately

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-18 20:59_

---

_Label `bug` added by @charliermarsh on 2024-03-18 20:59_

---

_Closed by @charliermarsh on 2024-03-18 21:48_

---

_Comment by @zeevro on 2024-03-19 12:12_

Wow, that was seriously speedy!

---
