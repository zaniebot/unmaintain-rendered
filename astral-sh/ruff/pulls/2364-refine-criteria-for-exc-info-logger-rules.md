```yaml
number: 2364
title: "Refine criteria for `exc_info` logger rules"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/G
created_at: 2023-01-30T21:13:58Z
updated_at: 2023-01-30T21:32:02Z
url: https://github.com/astral-sh/ruff/pull/2364
synced_at: 2026-01-12T15:55:08Z
```

# Refine criteria for `exc_info` logger rules

---

_@charliermarsh_

We now only trigger `logging-exc-info` and `logging-redundant-exc-info` when in an exception handler, with an `exc_info` that isn't `true` or `sys.exc_info()`.

Closes #2356.

---

_Label `rule` added by @charliermarsh on 2023-01-30 21:13_

---

_Renamed from "Only trigger `logging-exc-info` for certain `exc_info` values" to "Only trigger `exc_info` logger rules in handlers with non-constant `exc_info` values" by @charliermarsh on 2023-01-30 21:25_

---

_Renamed from "Only trigger `exc_info` logger rules in handlers with non-constant `exc_info` values" to "Refine criteria for `exc_info` logger rules" by @charliermarsh on 2023-01-30 21:25_

---

_Comment by @charliermarsh on 2023-01-30 21:25_

\cc @ngnpope - I also limited the `logging.exception` rule to "being in an exception handler", though it wasn't mentioned in your issue.

---

_Merged by @charliermarsh on 2023-01-30 21:32_

---

_Closed by @charliermarsh on 2023-01-30 21:32_

---

_Branch deleted on 2023-01-30 21:32_

---
