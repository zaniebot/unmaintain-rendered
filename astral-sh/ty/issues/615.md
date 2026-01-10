```yaml
number: 615
title: datetimes and dates are not substitutable but ty thinks they are
type: issue
state: open
author: m-aciek
labels:
  - wish
assignees: []
created_at: 2025-06-09T11:18:29Z
updated_at: 2025-11-18T16:10:30Z
url: https://github.com/astral-sh/ty/issues/615
synced_at: 2026-01-10T01:58:59Z
```

# datetimes and dates are not substitutable but ty thinks they are

---

_Issue opened by @m-aciek on 2025-06-09 11:18_

```python
from datetime import date, datetime

if datetime.now() < date.today():
    print("that's a surprise!")
```

* What is the actual behavior/output?
No error!

* What is the behavior/output you expect?
A warning, since at runtime we get `TypeError: can't compare datetime.datetime to datetime.date`.

`datetime` subclassing `date` is a violation of Liskov substitution principle in Python. Though I'd expect typecheckers to at least warn the developer.

Related report in mypy: https://github.com/python/mypy/issues/9015.

---

_Comment by @AlexWaygood on 2025-06-09 11:41_

Thanks for opening the issue. No type checker that I'm aware of issues a complaint about this code, so I would classify this as a feature rather than a bug. As you say, the fundamental problem is that `datetime` breaks the Liskov Substitution Principle.

It's possible we could add some logic like Jukka describes in https://github.com/python/mypy/issues/9015#issuecomment-646678572, but this definitely wouldn't be a priority for us since it goes above and beyond compliance with the typing spec. It also seems like the kind of thing that could possibly be expensive to check fully.

Another option would, I suppose, be to hardcode some special-cased logic in specifically for `datetime` and `date`. I don't like that option though: adding special-casing like that doesn't scale well (several other classes in the CPython stdlib are unsound in the same way), creates inconsistencies that can be confusing to users, and is prone to breakage. 

---

_Label `wish` added by @AlexWaygood on 2025-06-09 11:41_

---

_Comment by @AlexWaygood on 2025-06-09 11:47_

(In the meantime, [as you know](https://github.com/python/mypy/issues/9015#issuecomment-1621406542), another option is to use a more modern datetime library than the CPython stdlib one, such as [`datetype`](https://github.com/glyph/DateType) or [`whenever`](https://github.com/ariebovenberg/whenever))

---

_Comment by @MichaReiser on 2025-06-09 14:50_

Somewhat related to https://github.com/astral-sh/ty/issues/576

---

_Comment by @AlexWaygood on 2025-06-09 14:58_

> Somewhat related to [#576](https://github.com/astral-sh/ty/issues/576)

eh, not really:
- strict-equality is about catching expressions that likely always evaluate to `False` or `True` (probably unintentionally). This issue is about catching exceptions that arise from non-equality comparisons. It's debatable whether strict-equality should even be a type checker issue or whether it should be a linter issue; this issue is definitely a type-checking issue.
- strict-equality is something that existing type checkers already have some kind of solution for (even though all existing solutions use somewhat ugly heuristics). This is something that no type checker has a solution for really.
- This issue only arises because the CPython stdlib is breaking some fundamental rules of subtyping, which typeshed is forced to mimic in its stubs, and this breaks some fundamental assumptions made by ty. (You can see this from the number of `type: ignore[override]` comments in [typeshed's stubs for the datetime module](https://github.com/python/typeshed/blob/8f5a80e7b741226e525bd90e45496b8f68dc69b6/stdlib/datetime.pyi#L331-L334)!) None of that's the case for strict-equality considerations; strict-equality isn't really an issue to do with type safety at all.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
