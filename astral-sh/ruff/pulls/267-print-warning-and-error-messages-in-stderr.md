```yaml
number: 267
title: Print warning and error messages in stderr
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: print-warnings-in-stderr
created_at: 2022-09-24T08:13:46Z
updated_at: 2022-09-24T13:27:36Z
url: https://github.com/astral-sh/ruff/pull/267
synced_at: 2026-01-12T05:48:45Z
```

# Print warning and error messages in stderr

---

_Pull request opened by @harupy on 2022-09-24 08:13_

I found this issue when I was parsing the JSON output. Warning messages should be written in stderr, not stdout:

Actual:

```
> ruff --format json code.py 2> /dev/null
No pyproject.toml found. <-
Falling back to default configuration... <-
[
  {
    "kind": "IfTuple",
    "fixed": false,
    "location": {
      "row": 1,
      "column": 1
    },
    "filename": "/home/haru/Desktop/repositories/ruff-extension-test/code.py"
  }
]
```

Expected:

```
> ruff --format json code.py 2> /dev/null
[
  {
    "kind": "IfTuple",
    "fixed": false,
    "location": {
      "row": 1,
      "column": 1
    },
    "filename": "/home/haru/Desktop/repositories/ruff-extension-test/code.py"
  }
]
```

---

_Renamed from "Print warnings messages in stderr" to "Print warning and error messages in stderr" by @harupy on 2022-09-24 13:11_

---

_Comment by @charliermarsh on 2022-09-24 13:27_

Thank you!

---

_Merged by @charliermarsh on 2022-09-24 13:27_

---

_Closed by @charliermarsh on 2022-09-24 13:27_

---
