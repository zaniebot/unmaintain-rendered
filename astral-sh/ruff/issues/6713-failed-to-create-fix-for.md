```yaml
number: 6713
title: "Failed to create fix for `UnnecessaryComprehensionAnyAll: Expected Expression::ListComp`"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fixes
  - fuzzer
assignees: []
created_at: 2023-08-21T08:46:55Z
updated_at: 2023-08-21T23:41:14Z
url: https://github.com/astral-sh/ruff/issues/6713
synced_at: 2026-01-12T15:54:46Z
```

# Failed to create fix for `UnnecessaryComprehensionAnyAll: Expected Expression::ListComp`

---

_@qarmin_


Ruff 0.0.285

```
ruff  *.py --select ALL
```

file content:
```
any(x.id for x in bar)
any({x.id for x in bar})
```

error:
```
error: Failed to create fix for UnnecessaryComprehensionAnyAll: Expected Expression::ListComp
PY_FILE_TEST_51040573.py:1:1: D100 Missing docstring in public module
```


[PY_FILE_TEST_51040573.py.zip](https://github.com/astral-sh/ruff/files/12394105/PY_FILE_TEST_51040573.py.zip)


---

_Comment by @dhruvmanila on 2023-08-21 10:19_

It seems that neither `flake8-comprehension` or `flake8-pie` raises a violation when set comprehensions are used. \cc @charliermarsh any idea why this was added in https://github.com/astral-sh/ruff/pull/3824?

Sets are unordered so we might not know when `any` stops and if it benefits or not. Depending on the reason for the PR, the fix would be either to not flag it for set comprehensions or update the autofix to account for that.

---

_Label `bug` added by @dhruvmanila on 2023-08-21 10:20_

---

_Label `autofix` added by @dhruvmanila on 2023-08-21 10:20_

---

_Comment by @charliermarsh on 2023-08-21 12:54_

@dhruvmanila - It still makes sense to avoid allocating the set IMO. We should update the autofix, I think.

---

_Label `fuzzer` added by @charliermarsh on 2023-08-21 12:54_

---

_Closed by @charliermarsh on 2023-08-21 23:41_

---
