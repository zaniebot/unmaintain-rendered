```yaml
number: 3121
title: "Mark `typing.assert_never` as no return"
type: pull_request
state: merged
author: bluetech
labels:
  - bug
assignees: []
merged: true
base: main
head: no-return-assert-never
created_at: 2023-02-22T13:17:34Z
updated_at: 2023-02-22T14:15:40Z
url: https://github.com/astral-sh/ruff/pull/3121
synced_at: 2026-01-12T04:39:44Z
```

# Mark `typing.assert_never` as no return

---

_Pull request opened by @bluetech on 2023-02-22 13:17_

[This function](https://docs.python.org/3/library/typing.html#typing.assert_never) always raises, so RET503 shouldn't trigger for it.

---

_Label `bug` added by @charliermarsh on 2023-02-22 14:15_

---

_Merged by @charliermarsh on 2023-02-22 14:15_

---

_Closed by @charliermarsh on 2023-02-22 14:15_

---
