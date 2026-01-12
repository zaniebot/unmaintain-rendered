```yaml
number: 14826
title: Incorrect resolution of nested modules
type: issue
state: closed
author: dcreager
labels:
  - bug
  - ty
assignees: []
created_at: 2024-12-06T22:06:43Z
updated_at: 2024-12-17T19:24:14Z
url: https://github.com/astral-sh/ruff/issues/14826
synced_at: 2026-01-12T15:54:54Z
```

# Incorrect resolution of nested modules

---

_@dcreager_

In #14825 I tried creating a test case that refers to a class in a nested module:

````md
```py path=a/b.py
class C: ...
```

```py
import a.b
x: type[a.b.C]
```
````

This produced an unexpected `unresolved-attribute` error:

> Type `<module 'System("/src/a/b.py")'>` has no attribute `b`

This suggests that our module resolution code is binding `a` to `a.b`, and not to `a` as the Python spec requires.

---

_Label `red-knot` added by @dcreager on 2024-12-06 22:06_

---

_Assigned to @dcreager by @dcreager on 2024-12-06 22:06_

---

_Added to milestone `Red Knot 2024` by @carljm on 2024-12-07 01:24_

---

_Label `bug` added by @MichaReiser on 2024-12-07 13:30_

---

_Comment by @MichaReiser on 2024-12-07 13:30_

@carljm this shouldn't be too hard to fix? Maybe something for a contributor to take on?

---

_Comment by @carljm on 2024-12-07 17:21_

@MichaReiser I suggested @dcreager might take a look, good intro task for the module resolver.

---

_Comment by @AlexWaygood on 2024-12-07 23:36_

Great spot @dcreager! There _is_ an existing TODO in the codebase regarding submodule imports, but this doesn't seem to be the same issue; I think it is a previously unknown bug.

For the record, the existing TODO is here:

https://github.com/astral-sh/ruff/blob/269e47be961d3eb95c0351e8f7006b71e083a6a4/crates/red_knot_python_semantic/src/types/infer.rs#L2155-L2163

The tests referenced by that TODO don't exist anymore, however; that's because they've been ported to mdtests. They're now here:

https://github.com/astral-sh/ruff/blob/269e47be961d3eb95c0351e8f7006b71e083a6a4/crates/red_knot_python_semantic/resources/mdtest/import/relative.md?plain=1#L114-L143

---

_Comment by @dcreager on 2024-12-17 19:24_

Fixed in https://github.com/astral-sh/ruff/pull/14946 and https://github.com/astral-sh/ruff/pull/15026

---

_Closed by @dcreager on 2024-12-17 19:24_

---
