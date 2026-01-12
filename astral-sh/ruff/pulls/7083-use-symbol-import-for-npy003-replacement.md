```yaml
number: 7083
title: Use symbol import for NPY003 replacement
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: charlie/npy
created_at: 2023-09-03T15:38:01Z
updated_at: 2023-09-03T15:55:11Z
url: https://github.com/astral-sh/ruff/pull/7083
synced_at: 2026-01-12T02:45:38Z
```

# Use symbol import for NPY003 replacement

---

_Pull request opened by @charliermarsh on 2023-09-03 15:38_

## Summary

This moves the `NPY003` autofix to our symbol import infrastructure. It closes https://github.com/astral-sh/ruff/issues/7077, or at least, means we can merge that issue with https://github.com/astral-sh/ruff/issues/6842. It also ensures that if the symbol is imported using `from numpy import round_`, we don't _assume_ that `round` is bound to `np.round` as we did before.


---

_Label `bug` added by @charliermarsh on 2023-09-03 15:38_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-03 15:38_

---

_Merged by @charliermarsh on 2023-09-03 15:53_

---

_Closed by @charliermarsh on 2023-09-03 15:53_

---

_Branch deleted on 2023-09-03 15:53_

---

_Comment by @github-actions[bot] on 2023-09-03 15:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
