```yaml
number: 6450
title: SIM118 false positive in for loops
type: issue
state: closed
author: ilia-kats
labels: []
assignees: []
created_at: 2023-08-09T14:03:22Z
updated_at: 2024-08-02T20:20:22Z
url: https://github.com/astral-sh/ruff/issues/6450
synced_at: 2026-01-12T15:54:46Z
```

# SIM118 false positive in for loops

---

_@ilia-kats_

The SIM118 rule yields a false positive when iterating over a dict's keys. For example,
```python
test = {}
for k in test.keys():
    print(k)
```
and running `ruff check --select SIM test.py` results in
```
test.py:2:5: SIM118 [*] Use `k in test` instead of `k in test.keys()`
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```
with ruff 0.0.283.

---

_Comment by @charliermarsh on 2023-08-09 14:11_

That to me looks like a true positive, iterating over a dictionary iterates over its keys, unless I’m misinterpreting the issue?

---

_Comment by @ilia-kats on 2023-08-09 14:13_

right, my bad, sorry. For some reason I confused `for k in test` with `for k in test.items()`. Closing

---

_Closed by @ilia-kats on 2023-08-09 14:13_

---

_Comment by @gsakkis on 2024-08-02 20:20_

Sorry to resurrect this but the [docs](https://docs.astral.sh/ruff/rules/in-dict-keys/) mention explicitly `Checks for key-existence checks against dict.keys() calls`; there's no mentioning that this applies to loops as well. So there's at least a documentation issue. 

Although IMO this should be a separate rule, say `loop-dict-keys`. It is often more readable to iterate explicitly over the keys than the dict itself so one may want to disable this while keeping the `in-dict-keys` enabled.




---
