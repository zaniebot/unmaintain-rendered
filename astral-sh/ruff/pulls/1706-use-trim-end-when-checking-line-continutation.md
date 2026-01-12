```yaml
number: 1706
title: "Use `trim_end` when checking line continutation"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: use-trim_end
created_at: 2023-01-07T02:58:51Z
updated_at: 2023-01-07T03:26:35Z
url: https://github.com/astral-sh/ruff/pull/1706
synced_at: 2026-01-12T05:36:32Z
```

# Use `trim_end` when checking line continutation

---

_Pull request opened by @harupy on 2023-01-07 02:58_

Just a minor optimization.

---

_Merged by @charliermarsh on 2023-01-07 02:59_

---

_Closed by @charliermarsh on 2023-01-07 02:59_

---

_Branch deleted on 2023-01-07 03:15_

---

_Comment by @harupy on 2023-01-07 03:16_

@charliermarsh Maybe this is a nice lint rule for Python.

```python
"s".strip().endswith(...)  # should be rstrip().endswith(...)
"s".strip().startswith(...)  # should be lstrip().endswith(...)
```

---

_Comment by @andersk on 2023-01-07 03:21_

They’re not the same in all cases.

```pycon
>>> " s ".strip().endswith(" s")
False
>>> " s ".rstrip().endswith(" s")
True
```

---

_Comment by @harupy on 2023-01-07 03:26_

@andersk Good catch!

---
