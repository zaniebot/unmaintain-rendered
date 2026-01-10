---
number: 19511
title: C420 false positive when the assignment mutates a pre-existing object
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2025-07-23T15:10:45Z
updated_at: 2025-08-08T19:02:31Z
url: https://github.com/astral-sh/ruff/issues/19511
synced_at: 2026-01-10T01:23:00Z
---

# C420 false positive when the assignment mutates a pre-existing object

---

_Issue opened by @dscorbett on 2025-07-23 15:10_

### Summary

[`unnecessary-dict-comprehension-for-iterable` (C420)](https://docs.astral.sh/ruff/rules/unnecessary-dict-comprehension-for-iterable/) has a false positive when the assignment target includes an attribute reference, subscription, or slicing. The assignment has a side effect beyond the comprehension so the comprehension is not unnecessary. Example:
```console
$ cat >c420.py <<'# EOF'
class C: a = None
{C.a: None for C.a in "abc"}
print(C.a)

x = [None]
{x[0]: None for x[0] in "abc"}
print(x)

class C(list):
    def __getitem__(self, index, /):
        item = super().__getitem__(index)
        if isinstance(index, slice): item = tuple(item)
        return item
x = C()
{x[:0]: None for x[:0] in "abc"}
print(x)
# EOF

$ python c420.py
c
['c']
['c', 'b', 'a']

$ ruff --isolated check c420.py --select C420 --preview --fix
Found 3 errors (3 fixed, 0 remaining).

$ python c420.py
None
[None]
[]
```

### Version

ruff 0.12.4 (ee2759b36 2025-07-17)

---

_Label `bug` added by @ntBre on 2025-07-23 15:39_

---

_Label `rule` added by @ntBre on 2025-07-23 15:39_

---

_Referenced in [astral-sh/ruff#19513](../../astral-sh/ruff/pulls/19513.md) on 2025-07-23 17:10_

---

_Closed by @ntBre on 2025-08-08 19:02_

---
