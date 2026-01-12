```yaml
number: 1858
title: support calls to intersection types
type: issue
state: open
author: carljm
labels:
  - calls
  - set-theoretic types
assignees: []
created_at: 2025-12-11T19:45:46Z
updated_at: 2025-12-16T21:08:34Z
url: https://github.com/astral-sh/ty/issues/1858
synced_at: 2026-01-12T15:54:26Z
```

# support calls to intersection types

---

_@carljm_

Right now we infer a todo type, which suppresses false positives. But intersections arise reasonably frequently in narrowing, and often don't simplify (especially with negative or dynamic elements). So this can lead to a lot of false negatives.

The algorithm here isn't particularly hard. We should try to call each element of the intersection. We can probably short-circuit some cases (most negative intersection elements will simply fallback to `object`, and we know `object` is not callable). If calls to any element(s) succeed, the return type is the intersection of their returns, and we should ignore errors from those that failed. If all fail, we probably want to discard errors from some very general types (the same cases we considered short-circuiting above) and display the other errors. (Handling the diagnostics is probably the most complex piece of this issue.)

We could also do a very simple initial version here where we try to short-circuit negative elements and if that reduces the intersection to a single type, call it normally. That would already improve things a lot for narrowing cases.

---

_Added to milestone `Beta` by @carljm on 2025-12-11 19:45_

---

_Label `calls` added by @carljm on 2025-12-11 19:45_

---

_Label `set-theoretic types` added by @carljm on 2025-12-11 19:45_

---

_Comment by @AlexWaygood on 2025-12-11 19:47_

IIRC, according to https://jellezijlstra.github.io/negation-types.html we should just ignore negative elements altogether for most operations on intersection types?

---

_Comment by @carljm on 2025-12-11 19:56_

Yes, that's for the reason I mentioned: except for negation of some special very broad types like `AlwaysTruthy` and `AlwaysFalsy` in very particular scenarios (boolean checks), a negative intersection element generally can't be treated any differently from `object` -- and if it were `object` it would simplify out of the intersection anyway.

I think it would be a fairly easy and worthwhile experiment to just eagerly strip negative elements from intersection types altogether and see where that degrades our inference in practice.

---

_Comment by @AlexWaygood on 2025-12-11 19:59_

sorry, should have read more carefully :( blame it on the fever.

---

_Assigned to @carljm by @carljm on 2025-12-12 03:38_

---

_Removed from milestone `Beta` by @carljm on 2025-12-16 21:08_

---

_Added to milestone `Stable` by @carljm on 2025-12-16 21:08_

---
