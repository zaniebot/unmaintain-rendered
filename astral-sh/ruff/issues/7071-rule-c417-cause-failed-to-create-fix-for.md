```yaml
number: 7071
title: "Rule C417 cause \"Failed to create fix for UnnecessaryMap: Expected tuple for dict comprehension\""
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T08:35:23Z
updated_at: 2023-09-03T13:34:11Z
url: https://github.com/astral-sh/ruff/issues/7071
synced_at: 2026-01-10T11:09:49Z
```

# Rule C417 cause "Failed to create fix for UnnecessaryMap: Expected tuple for dict comprehension"

---

_Issue opened by @qarmin on 2023-09-03 08:35_

Ruff 0.0.287 (latest changes from main branch)

```
ruff --fix *.py  --no-cache --select C417
```

file content:
```
def isInt(v):
		self.dv = dict(map(lambda x: [x, os.path.realpath(os.path.join('/dev/disk/by-id', x))], self.dn))
```

error:
```
error: Failed to create fix for UnnecessaryMap: Expected tuple for dict comprehension
1791844.py:2:13: C417 Unnecessary `map` usage (rewrite using a `dict` comprehension)
Found 1 error.
```

[1791844.py.zip](https://github.com/astral-sh/ruff/files/12505512/1791844.py.zip)


---

_Label `bug` added by @charliermarsh on 2023-09-03 13:09_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-03 13:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 13:09_

---

_Closed by @charliermarsh on 2023-09-03 13:34_

---
