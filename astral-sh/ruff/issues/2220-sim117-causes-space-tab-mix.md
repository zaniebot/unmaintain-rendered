```yaml
number: 2220
title: SIM117 causes space-tab mix
type: issue
state: closed
author: spaceone
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-01-26T20:43:07Z
updated_at: 2023-01-26T21:09:36Z
url: https://github.com/astral-sh/ruff/issues/2220
synced_at: 2026-01-12T15:54:42Z
```

# SIM117 causes space-tab mix

---

_@spaceone_

`SIM117` transforms
```python
with open() as fd:
	with open() as fd2:
		foo(
			1,
			2,
			3,
		)
```
(which is indented with tabs only)

into

```python
with open() as fd, open() as fd2:
    foo(
    	1,
    	2,
    	3,
    )
```
(which has a tab+space indentation mixed).

---

_Label `bug` added by @charliermarsh on 2023-01-26 20:49_

---

_Label `autofix` added by @charliermarsh on 2023-01-26 20:49_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-26 20:59_

---

_Comment by @charliermarsh on 2023-01-26 20:59_

Taking a look at this now...

---

_Closed by @charliermarsh on 2023-01-26 21:09_

---
