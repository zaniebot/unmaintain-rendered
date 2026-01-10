```yaml
number: 2308
title: Consider more precise return type for dynamic overload calls, if all overload returns share some component
type: issue
state: open
author: Drizzelkun
labels:
  - overloads
assignees: []
created_at: 2026-01-02T20:52:12Z
updated_at: 2026-01-05T17:07:14Z
url: https://github.com/astral-sh/ty/issues/2308
synced_at: 2026-01-10T01:56:41Z
```

# Consider more precise return type for dynamic overload calls, if all overload returns share some component

---

_Issue opened by @Drizzelkun on 2026-01-02 20:52_

### Question

When calling `plt.subplots` (possibly any overloaded function) with dynamic kwargs ty is unable to infer the type of `fig` even though all overloaded function calls return a 2-item tuple where the first item is always Figure. So no matter what kwargs could possibly provide, the first unpacked item must always be Figure.

I assume this is already tracked somewhere so I'm hesitant to create a bug report but I couldn't find a specific matching issue.

<img width="1014" height="390" alt="Image" src="https://github.com/user-attachments/assets/98448e31-527c-47e2-80b1-4587c68a40f4" />

### Version

ty 0.0.8 (aa7559db8 2025-12-29

---

_Label `question` added by @Drizzelkun on 2026-01-02 20:52_

---

_Comment by @carljm on 2026-01-04 21:20_

Thanks for the report!

This behavior is actually mandated by the typing spec currently, and it is the same behavior as mypy and pyright. (I reproduced it using this standalone snippet: https://play.ty.dev/c01994b8-d5c7-4589-b470-eead268bddeb)

It is true that in some cases a more precise type could safely be inferred without causing false positives, if all overloads return a common outer type, by pushing down the `Unknown` to the point where they differ (and inferring e.g. `tuple[int, Unknown]` instead of `Unknown` in my example.) This seems doable, but probably not an immediate priority for us, given that our current behavior is mandated by spec and shared by all type checkers.

---

_Label `question` removed by @carljm on 2026-01-04 21:20_

---

_Label `overloads` added by @carljm on 2026-01-04 21:20_

---

_Renamed from "Unknown inlayHint for theoretically narrowed type in overloaded function known?" to "Consider more precise return type for dynamic overload calls, if all overload returns share some component" by @carljm on 2026-01-04 21:21_

---

_Comment by @Drizzelkun on 2026-01-05 17:04_

Thanks for the answer. 

Where can I find the spec to read? It might help me in the future to clear up confusions.

---

_Comment by @AlexWaygood on 2026-01-05 17:07_

It's here: https://typing.python.org/en/latest/spec/. The overload evaluation algorithm is here: https://typing.python.org/en/latest/spec/overload.html#overload-call-evaluation. The conformance test suite is here: https://github.com/python/typing/tree/main/conformance

---
