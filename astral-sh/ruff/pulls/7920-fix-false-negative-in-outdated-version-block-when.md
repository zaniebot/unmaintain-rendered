```yaml
number: 7920
title: "Fix false negative in `outdated-version-block` when using greater than comparisons"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/fix-outdated-version-block
created_at: 2023-10-11T15:24:20Z
updated_at: 2023-10-11T19:33:45Z
url: https://github.com/astral-sh/ruff/pull/7920
synced_at: 2026-01-12T15:55:25Z
```

# Fix false negative in `outdated-version-block` when using greater than comparisons

---

_@zanieb_

Closes #7902 

```python
# With target version of Python 3.7
import sys

if sys.version_info >= (3, 7):  # ERROR: UP036
    print('oh no')

if sys.version_info <= (3, 7):  # OK: Can evaluate to different value sometimes e.g. version 3.7 vs 3.8
    print('oh no')

if sys.version_info < (3, 7):  # ERROR: UP036
    print('oh no')

if sys.version_info > (3, 7):  # OK: Can evaluate to different value sometimes e.g. version 3.7 vs 3.8
    print('oh no')

if sys.version_info < (3, 6):  # ERROR: UP036
    print('oh no')

if sys.version_info > (3, 6):  # ERROR: UP036
    print('oh no')

if sys.version_info < (3, 8):  # OK: Can evaluate to a different value sometimes e.g. 3.7 vs 3.8
    print('oh no')

if sys.version_info > (3, 8):  # OK: Can evaluate to a different value sometimes e.g. 3.7 vs 3.9
    print('oh no')
```

---

_@charliermarsh approved on 2023-10-11 15:31_

---

_Comment by @github-actions[bot] on 2023-10-11 15:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @zanieb on 2023-10-11 16:28_

Following the one line fix, I've made some refactors for readability that have no effect on behavior.

---

_Marked ready for review by @zanieb on 2023-10-11 16:32_

---

_Merged by @zanieb on 2023-10-11 19:33_

---

_Closed by @zanieb on 2023-10-11 19:33_

---

_Branch deleted on 2023-10-11 19:33_

---
