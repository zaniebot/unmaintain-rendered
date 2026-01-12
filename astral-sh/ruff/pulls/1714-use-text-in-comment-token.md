```yaml
number: 1714
title: Use text in comment token
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: comment-text
created_at: 2023-01-07T11:46:17Z
updated_at: 2023-01-07T12:29:11Z
url: https://github.com/astral-sh/ruff/pull/1714
synced_at: 2026-01-12T05:36:32Z
```

# Use text in comment token

---

_Pull request opened by @harupy on 2023-01-07 11:46_

https://github.com/RustPython/RustPython/pull/4426 has been merged. We can simplify code using text in comment tokens.

---

_Renamed from "TEST" to "Use text in comment token" by @harupy on 2023-01-07 11:47_

---

_Review comment by @harupy on `src/pycodestyle/snapshots/ruff__pycodestyle__tests__E999_E999.py.snap`:6 on 2023-01-07 11:48_

https://github.com/RustPython/RustPython/pull/4399 seems related

---

_@harupy reviewed on 2023-01-07 11:48_

---

_@harupy reviewed on 2023-01-07 11:59_

---

_Review comment by @harupy on `src/pycodestyle/snapshots/ruff__pycodestyle__tests__E999_E999.py.snap`:6 on 2023-01-07 11:59_

Looks like this is the right change.

```
> python --version
Python 3.10.8
> python resources/test/fixtures/pycodestyle/E999.py
  File "/home/haru/Desktop/repositories/ruff/resources/test/fixtures/pycodestyle/E999.py", line 4
    
IndentationError: expected an indented block after function definition on line 2
```

---

_Merged by @charliermarsh on 2023-01-07 12:29_

---

_Closed by @charliermarsh on 2023-01-07 12:29_

---

_Comment by @charliermarsh on 2023-01-07 12:29_

Excellent, thanks!

---
