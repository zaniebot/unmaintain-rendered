```yaml
number: 3343
title: Skip byte-order-mark at start of file
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/bom
created_at: 2023-03-04T19:16:59Z
updated_at: 2023-03-06T02:37:15Z
url: https://github.com/astral-sh/ruff/pull/3343
synced_at: 2026-01-12T15:55:12Z
```

# Skip byte-order-mark at start of file

---

_@charliermarsh_

This mirrors RustPython's own logic for skipping the `feff` character at start-of-file:

https://github.com/RustPython/RustPython/blob/ab7971de424cd1b8f5ebc1f8931992a1b54af444/compiler/parser/src/lexer.rs#L243

Closes #3336.


---

_Label `bug` added by @charliermarsh on 2023-03-04 19:17_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-04 19:21_

---

_Review request for @MichaReiser removed by @charliermarsh on 2023-03-06 02:37_

---

_Merged by @charliermarsh on 2023-03-06 02:37_

---

_Closed by @charliermarsh on 2023-03-06 02:37_

---

_Branch deleted on 2023-03-06 02:37_

---
