---
number: 18399
title: B909 false positive when directly returning mutation
type: issue
state: closed
author: TeamSpen210
labels:
  - rule
  - help wanted
assignees: []
created_at: 2025-05-31T03:25:36Z
updated_at: 2025-06-13T14:39:57Z
url: https://github.com/astral-sh/ruff/issues/18399
synced_at: 2026-01-10T01:22:59Z
---

# B909 false positive when directly returning mutation

---

_Issue opened by @TeamSpen210 on 2025-05-31 03:25_

### Summary

The B909 rule checks for mutations to loop iterables, but permits cases where `return`/`break` would prevent further iterations. However if the `return` and the mutation are the same statement it flags the value.

Example:
```py
def fail_map(mapping):
    for key in mapping:
        return mapping.pop(key)  # false positive

def success_map(mapping):
    for key in mapping:
        ret = mapping.pop(key)  # passes
        return ret

def fail_list(seq):
    for val in seq:
        return seq.pop(4)  # also false positive
```
Used `ruff check --preview --isolated --select B909 iter_mut_bug.py`.

### Version

ruff 0.11.11 (0397682f1 2025-05-22)

---

_Label `rule` added by @ntBre on 2025-05-31 14:03_

---

_Label `help wanted` added by @ntBre on 2025-05-31 14:03_

---

_Comment by @ntBre on 2025-05-31 14:04_

Thanks for the report! Without looking at the code this _seems_ like a pretty straightforward fix since we already handle the second case here.

---

_Referenced in [astral-sh/ruff#18408](../../astral-sh/ruff/pulls/18408.md) on 2025-06-01 04:42_

---

_Closed by @ntBre on 2025-06-13 14:39_

---
