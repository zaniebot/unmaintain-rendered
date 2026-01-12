```yaml
number: 7739
title: Perform insertions before replacements
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: charlie/RUF017
created_at: 2023-10-01T14:44:30Z
updated_at: 2023-10-01T15:00:11Z
url: https://github.com/astral-sh/ruff/pull/7739
synced_at: 2026-01-12T15:55:24Z
```

# Perform insertions before replacements

---

_@charliermarsh_

## Summary

If we have, e.g.:

```python
sum((
            factor.dims for factor in bases
        ), [])
```

We generate three edits: two insertions (for the `operator` and `functools` imports), and then one replacement (for the `sum` call itself). We need to ensure that the insertions come before the replacement; otherwise, the edits will appear overlapping and out-of-order.

Closes https://github.com/astral-sh/ruff/issues/7718.


---

_Label `bug` added by @charliermarsh on 2023-10-01 14:44_

---

_Label `fuzzer` added by @charliermarsh on 2023-10-01 14:44_

---

_Merged by @charliermarsh on 2023-10-01 14:53_

---

_Closed by @charliermarsh on 2023-10-01 14:53_

---

_Branch deleted on 2023-10-01 14:53_

---

_Comment by @github-actions[bot] on 2023-10-01 15:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
