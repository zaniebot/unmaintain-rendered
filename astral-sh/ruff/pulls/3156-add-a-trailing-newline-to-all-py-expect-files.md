```yaml
number: 3156
title: Add a trailing newline to all .py.expect files
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/newline
created_at: 2023-02-23T02:25:09Z
updated_at: 2023-02-23T02:29:29Z
url: https://github.com/astral-sh/ruff/pull/3156
synced_at: 2026-01-12T04:39:44Z
```

# Add a trailing newline to all .py.expect files

---

_Pull request opened by @charliermarsh on 2023-02-23 02:25_

This just re-formats all the `.py.expect` files with Black, both to add a trailing newline and be doubly-certain that they're correctly formatted.

I also ensured that we add a hard line break after each statement, and that we avoid including an extra newline in the generated Markdown (since the code should contain the exact expected newlines).


---

_Label `autoformatter` added by @charliermarsh on 2023-02-23 02:27_

---

_Merged by @charliermarsh on 2023-02-23 02:29_

---

_Closed by @charliermarsh on 2023-02-23 02:29_

---

_Branch deleted on 2023-02-23 02:29_

---
