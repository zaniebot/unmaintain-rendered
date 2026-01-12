```yaml
number: 13823
title: "[red-knot] Fix edge case for binary-expression inference where the lhs and rhs are the exact same type"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: binary-arithmetic-fix
created_at: 2024-10-19T16:52:42Z
updated_at: 2024-10-19T18:09:56Z
url: https://github.com/astral-sh/ruff/pull/13823
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] Fix edge case for binary-expression inference where the lhs and rhs are the exact same type

---

_@AlexWaygood_

## Summary

This fixes an edge case that @carljm and I missed when implementing https://github.com/astral-sh/ruff/pull/13800. Namely, if the left-hand operand is the _exact same type_ as the right-hand operand, the reflected dunder on the right-hand operand is never tried:

```pycon
>>> class Foo:
...     def __radd__(self, other):
...         return 42
...         
>>> Foo() + Foo()
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    Foo() + Foo()
    ~~~~~~^~~~~~~
TypeError: unsupported operand type(s) for +: 'Foo' and 'Foo'
```

This edge case _is_ covered in Brett's blog at https://snarky.ca/unravelling-binary-arithmetic-operations-in-python/, but I missed it amongst all the other subtleties of this algorithm. The motivations and history behind it were discussed in https://mail.python.org/archives/list/python-dev@python.org/thread/7NZUCODEAPQFMRFXYRMGJXDSIS3WJYIV/

## Test Plan

I added an mdtest for this cornercase.

---

_Label `red-knot` added by @AlexWaygood on 2024-10-19 16:52_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-19 16:52_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-19 16:52_

---

_Comment by @github-actions[bot] on 2024-10-19 17:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-10-19 18:09_

---

_Merged by @carljm on 2024-10-19 18:09_

---

_Closed by @carljm on 2024-10-19 18:09_

---

_Branch deleted on 2024-10-19 18:09_

---
