```yaml
number: 259
title: Include error code and message in JSON output
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: include-code-and-message
created_at: 2022-09-23T00:20:51Z
updated_at: 2022-09-23T04:54:14Z
url: https://github.com/astral-sh/ruff/pull/259
synced_at: 2026-01-12T15:55:04Z
```

# Include error code and message in JSON output

---

_@harupy_

Resolve #242.


```
> cargo run -- --format json resources/test/fixtures/F634.py
    Finished dev [unoptimized + debuginfo] target(s) in 0.07s
     Running `target/debug/ruff --format json resources/test/fixtures/F634.py`
[
  {
    "kind": "IfTuple",
    "code": "F634",
    "message": "If test is a tuple, which is always `True`",
    "fixed": false,
    "location": {
      "row": 1,
      "column": 1
    },
    "filename": "/home/haru/Desktop/repositories/ruff/resources/test/fixtures/F634.py"
  },
  {
    "kind": "IfTuple",
    "code": "F634",
    "message": "If test is a tuple, which is always `True`",
    "fixed": false,
    "location": {
      "row": 7,
      "column": 5
    },
    "filename": "/home/haru/Desktop/repositories/ruff/resources/test/fixtures/F634.py"
  }
]
```

---

_Review requested from @charliermarsh by @charliermarsh on 2022-09-23 00:23_

---

_Merged by @charliermarsh on 2022-09-23 00:29_

---

_Closed by @charliermarsh on 2022-09-23 00:29_

---

_Branch deleted on 2022-09-23 04:54_

---
