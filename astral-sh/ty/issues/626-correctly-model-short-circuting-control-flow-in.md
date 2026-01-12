```yaml
number: 626
title: correctly model short-circuting control flow in boolean expressions
type: issue
state: open
author: carljm
labels:
  - help wanted
  - control flow
assignees: []
created_at: 2025-06-10T16:05:08Z
updated_at: 2025-07-23T22:55:42Z
url: https://github.com/astral-sh/ty/issues/626
synced_at: 2026-01-12T15:54:23Z
```

# correctly model short-circuting control flow in boolean expressions

---

_@carljm_

### Summary

For example, in [this snippet](https://play.ty.dev/337ceb56-569f-4db8-aa55-f00b635b3be3) we should not emit a `possibly-unresolved-reference` error on the use of `x` inside the `if` block, because we can only reach the inside of the `if` block in a case where the assignment expression has definitely executed:

```py
def _(flag: bool, number: int):
    if flag and (x := number):
        reveal_type(x)
```

(Note that we disable `possibly-unresolved-reference` by default, so to reproduce this on the command line you'll have to use `--error possibly-unresolved-reference`.)

This was previously fixed in https://github.com/astral-sh/ruff/pull/18010, but the fix caused a large performance regression in some cases (see https://github.com/astral-sh/ty/issues/431 for details) and was reverted in https://github.com/astral-sh/ruff/pull/18150. So one useful way to approach this issue is to reapply the previous fix and explore the cause of the performance regression (and add a benchmark that would catch that regression in future.)

### Version

_No response_

---

_Label `help wanted` added by @carljm on 2025-06-10 16:05_

---

_Label `control flow` added by @carljm on 2025-06-10 16:05_

---

_Comment by @sharkdp on 2025-06-10 17:52_

Just adding a quick note here that I'm currently doing a major rework of the visibility/reachability constraints system. I don't want to block work on this, but it would probably be more efficient to wait until #365 is resolved. And it might even make the implementation of this easier.

---

_Comment by @TomerBin on 2025-06-10 18:59_

I reproduced the performance issue with a single file with 1000 `if x and y: ...` lines. 
I found something that seems to fix the issue, but then I got #365 in the mdtest suit, so I stopped there. Maybe we should wait for #365 before putting extra work on this


---

_Comment by @sharkdp on 2025-06-17 08:09_

@TomerBin The [reachability-constraints PR](https://github.com/astral-sh/ruff/pull/18621) has now been merged and #365 has been fixed. Let me know if you need any help adapting your changes to the new constraints system.

---

_Comment by @TomerBin on 2025-06-17 08:10_

@sharkdp Awesome! I'll take a look shortly 
Tnx

---

_Comment by @TomerBin on 2025-06-24 21:06_

For clarity - we are now blocked on #690

---

_Added to milestone `GA` by @carljm on 2025-07-23 22:55_

---
