```yaml
number: 607
title: "`possibly-unresolved-reference` false positive inside function when both variable and function are defined in the same branch of an if statement"
type: issue
state: closed
author: DetachHead
labels:
  - control flow
assignees: []
created_at: 2025-06-08T01:28:20Z
updated_at: 2025-06-26T17:35:54Z
url: https://github.com/astral-sh/ty/issues/607
synced_at: 2026-01-12T15:54:23Z
```

# `possibly-unresolved-reference` false positive inside function when both variable and function are defined in the same branch of an if statement

---

_@DetachHead_

### Summary

```py
def _(value: bool):
   if value:
      a = 1

      def asdf():
         print(a) # possibly-unresolved-reference
```
https://play.ty.dev/f90a868c-cc44-4d7d-af01-9112b87e384a

### Version

0.0.1-alpha.8 (c1337c962 2025-06-02)

---

_Label `control flow` added by @AlexWaygood on 2025-06-08 09:24_

---

_Comment by @AlexWaygood on 2025-06-08 09:28_

Thanks! This is a duplicate of #210 (see https://github.com/astral-sh/ty/issues/210#issuecomment-2859029566)

---

_Closed by @AlexWaygood on 2025-06-08 09:28_

---

_Comment by @carljm on 2025-06-09 17:09_

I suspect the initial version of #210 will not fix this issue; the top-priority fix there is to stop wrongly assuming end-of-scope (and instead assume might-be-called-from-anywhere-in-scope). Once we have that foundation in place, we can explore further trimming of the possibilities.

---

_Comment by @DetachHead on 2025-06-09 21:35_

In that case should we re-open this issue?

---

_Comment by @carljm on 2025-06-10 16:45_

> In that case should we re-open this issue?

Yeah I think it's useful to track this as a separate improvement.

---

_Reopened by @carljm on 2025-06-10 16:45_

---

_Closed by @sharkdp on 2025-06-26 10:24_

---

_Comment by @carljm on 2025-06-26 16:38_

This was closed by https://github.com/astral-sh/ruff/pull/18750, although it's not fixed.

I think we can leave it closed though, as a duplicate of #710 

---

_Comment by @sharkdp on 2025-06-26 17:35_

I think the boundness analysis problem here was indeed fixed. In a rather crude way, though: by always considering nonlocal symbols to be bound.

---
