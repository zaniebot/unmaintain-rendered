```yaml
number: 7781
title: "Check sequence type before triggering `unnecessary-enumerate` (`FURB148`) `len` suggestion"
type: pull_request
state: merged
author: tjkuson
labels:
  - bug
assignees: []
merged: true
base: main
head: avoid-generators
created_at: 2023-10-03T13:55:27Z
updated_at: 2023-10-03T14:46:03Z
url: https://github.com/astral-sh/ruff/pull/7781
synced_at: 2026-01-12T15:55:24Z
```

# Check sequence type before triggering `unnecessary-enumerate` (`FURB148`) `len` suggestion

---

_@tjkuson_

## Summary

Check that the sequence type is a list, set, dict, or tuple before recommending replacing the `enumerate(...)` call with `range(len(...))`. Document behaviour so users are aware of the type inference limitation leading to false negatives. 

Closes #7656.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-10-03 14:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Marked ready for review by @tjkuson on 2023-10-03 14:12_

---

_Label `bug` added by @charliermarsh on 2023-10-03 14:29_

---

_Comment by @charliermarsh on 2023-10-03 14:29_

Thanks!

---

_Merged by @charliermarsh on 2023-10-03 14:39_

---

_Closed by @charliermarsh on 2023-10-03 14:39_

---

_Branch deleted on 2023-10-03 14:39_

---
