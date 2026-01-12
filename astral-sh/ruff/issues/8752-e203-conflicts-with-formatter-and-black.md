```yaml
number: 8752
title: "`E203` conflicts with formatter (and black)"
type: issue
state: closed
author: bersbersbers
labels:
  - bug
assignees: []
created_at: 2023-11-18T06:42:56Z
updated_at: 2024-02-16T13:56:00Z
url: https://github.com/astral-sh/ruff/issues/8752
synced_at: 2026-01-12T15:54:48Z
```

# `E203` conflicts with formatter (and black)

---

_@bersbersbers_

Rule `E203` still conflicts with ruff's own formatter:

`bug.py`
```python
x = [1, 2, 3]
x[len(x) :]
```

`bug.bat`
```batch
@echo off
pip install black==23.11.0 ruff==0.1.6
black bug.py
ruff format --isolated bug.py
ruff check --isolated --preview --select E203 --fix bug.py
ruff format --isolated bug.py
ruff check --isolated --preview --select E203 --fix bug.py
ruff format --isolated bug.py
```

Output:
```
All done! ✨ 🍰 ✨
1 file left unchanged.
1 file left unchanged
Found 1 error (1 fixed, 0 remaining).
1 file reformatted
Found 1 error (1 fixed, 0 remaining).
1 file reformatted
```

Somewhat related to https://github.com/astral-sh/ruff/issues/7259 (but 0.1.5 behaves the same way, so likely https://github.com/astral-sh/ruff/pull/8654 did not break this, but instead should be extended to one-sided slices)

Potentially relevant for https://github.com/astral-sh/ruff/issues/7993

---

_Label `bug` added by @charliermarsh on 2023-11-18 13:23_

---

_Comment by @charliermarsh on 2023-11-18 23:47_

Is this consistent with PEP 8, though? I feel like it's in conflict with the PEP 8 guidelines (which is fine for the formatter, but the intent of the pycodestyle rules is to enforce PEP 8). Now I'm wondering if we should instead just mark the rule as incompatible, rather than trying to make it compatible with the formatter.

---

_Comment by @bersbersbers on 2023-11-19 05:44_

PEP8 doesn't say anything about this.

> Exception: when a slice parameter is omitted, the space is omitted

This does not apply in our case: it concerns the space after the colon (where the parameter is omitted), not before the colon.

How exactly you solve this, I don't really care. Using black right now, I'm preparing the switch to ruffs formatter and ideally, you would make that as painless as possible :) Disabling rules for this is fine. (Is there a selector to ignore all formatter-incompatible rules?)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-25 18:10_

---

_Closed by @charliermarsh on 2023-11-25 18:16_

---

_Comment by @Blumenkind111 on 2024-02-16 13:55_

Hi, I think there is another case, where it is not working and that is when we have multiple indices, like in pandas: 

My example looks like this: 

`dataframe.loc[index + 1 :, "columnname"] `

`Ruff format` always adds the space and `ruff . --fix` always removes it.

---
