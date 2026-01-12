```yaml
number: 10911
title: "`PLR6104` has false positive for non-commutative additions"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-04-12T15:00:29Z
updated_at: 2024-04-18T04:37:01Z
url: https://github.com/astral-sh/ruff/issues/10911
synced_at: 2026-01-12T15:54:50Z
```

# `PLR6104` has false positive for non-commutative additions

---

_@charliermarsh_

@charliermarsh Is this fixed for:
```python
test_string = "desired = " + test_string  # False positive PLR6104
```
since `+` is sometimes commutative.

_Originally posted by @NeilGirdhar in https://github.com/astral-sh/ruff/issues/10900#issuecomment-2051918134_
            

---

_Label `bug` added by @charliermarsh on 2024-04-12 15:00_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-12 15:00_

---

_Closed by @charliermarsh on 2024-04-12 19:02_

---

_Comment by @NeilGirdhar on 2024-04-12 19:23_

I'm not sure from the code, but you still catch:
```python
x = x + "string"  # should be x += "string"
```
right?

---

_Comment by @charliermarsh on 2024-04-12 19:25_

Yeah

---

_Comment by @NeilGirdhar on 2024-04-12 20:45_

Awesome, thanks for fixing this so fast! 😄 

---

_Comment by @ofek on 2024-04-18 04:14_

I ran into another case:

![image](https://github.com/astral-sh/ruff/assets/9677399/1f719766-dce5-495f-9672-01375573d0ba)


---

_Comment by @charliermarsh on 2024-04-18 04:36_

This is fixed on `main` IIUC.

---
