---
number: 4659
title: Unexpected number of statements(PLR0915 Too many statements)
type: issue
state: closed
author: FishAlchemist
labels:
  - bug
assignees: []
created_at: 2023-05-25T16:32:19Z
updated_at: 2023-06-02T04:04:27Z
url: https://github.com/astral-sh/ruff/issues/4659
synced_at: 2026-01-10T01:22:43Z
---

# Unexpected number of statements(PLR0915 Too many statements)

---

_Issue opened by @FishAlchemist on 2023-05-25 16:32_

## Ruff Version
**ruff 0.0.269**
## Command(Powershell)
```powershell
ruff check --isolated --extend-select=PLR  .\main.py
```
## Output
```powershell
main.py:1:5: PLR0915 Too many statements (51 > 50)
Found 1 error.
```
## Source
**Please change the file extension from txt to py**
[main.txt](https://github.com/charliermarsh/ruff/files/11567206/main.txt)


---

_Renamed from "Unexpected number of statements(PLR0915 Too many statements )" to "Unexpected number of statements(PLR0915 Too many statements)" by @FishAlchemist on 2023-05-25 16:32_

---

_Comment by @charliermarsh on 2023-05-25 16:37_

Interestingly, now that I test it, this _is_ consistent with Pylint.

---

_Comment by @charliermarsh on 2023-05-25 16:42_

It looks like that choice predates the history on GitHub, can't find any information on it...

---

_Comment by @charliermarsh on 2023-05-25 16:42_

I am inclined to change it though.

---

_Label `bug` added by @charliermarsh on 2023-05-25 19:56_

---

_Referenced in [astral-sh/ruff#4794](../../astral-sh/ruff/pulls/4794.md) on 2023-06-02 03:58_

---

_Closed by @charliermarsh on 2023-06-02 04:04_

---
