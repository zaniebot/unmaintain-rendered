```yaml
number: 1186
title: "Error: `Index X is out of bound for tuple` after a if-guard that guarantees in-bounds"
type: issue
state: closed
author: jeertmans
labels: []
assignees: []
created_at: 2025-09-15T08:10:35Z
updated_at: 2025-09-15T08:26:21Z
url: https://github.com/astral-sh/ty/issues/1186
synced_at: 2026-01-10T02:06:25Z
```

# Error: `Index X is out of bound for tuple` after a if-guard that guarantees in-bounds

---

_Issue opened by @jeertmans on 2025-09-15 08:10_

### Summary

Hi!

Apparently, ty is unable to infer the length of tuple inside an `if`-guarded check that exactly checks the length:

```python
def f(tup: tuple[int, int] | tuple[int, int, int]) -> int:
    if len(tup) == 3:
        return tup[0] + tup[1] + tup[2]
    return tup[0] + tup[1]
```

Using `ty check` on this file raises:

```
error[index-out-of-bounds]: Index 2 is out of bounds for tuple `tuple[int, int]` with length 2
 --> issue.py:3:34
  |
1 | def f(tup: tuple[int, int] | tuple[int, int, int]) -> int:
2 |     if len(tup) == 3:
3 |         return tup[0] + tup[1] + tup[2]
  |                                  ^^^
4 |     return tup[0] + tup[1]
  |
info: rule `index-out-of-bounds` is enabled by default
```

I think ty should be able to infer the correct type of `tup` inside the `if` statement.

I searched the issue tracker for a similar bug report, but couldn't find any, so I hope this is not a duplicate :-)

Thanks in advance!

### Version

ty 0.0.1-alpha.20

---

_Comment by @sharkdp on 2025-09-15 08:20_

> I searched the issue tracker for a similar bug report, but couldn't find any, so I hope this is not a duplicate :-)

I think https://github.com/astral-sh/ty/issues/560 asks for the same thing. But no worries, finding the right tickets with GitHub's search is challenging.

---

_Closed by @sharkdp on 2025-09-15 08:20_

---

_Comment by @jeertmans on 2025-09-15 08:26_

Thanks for pointing, and sorry for the duplicate!

---
