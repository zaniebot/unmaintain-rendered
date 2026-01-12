```yaml
number: 13191
title: "[red-knot] Fix call expression inference edge case for decorated functions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/decorated-calls
created_at: 2024-09-01T11:06:51Z
updated_at: 2024-09-01T15:19:41Z
url: https://github.com/astral-sh/ruff/pull/13191
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Fix call expression inference edge case for decorated functions

---

_@AlexWaygood_

## Summary

If a function is decorated, we can't rely on the function's return annotation to give us accurate information about the type the function returns. The classic example of this is `contextlib.contextmanager`: although it is annotated correctly, the following function doesn't return an `Iterator` of `str`s, it returns an instance of `contextlib._GeneratorContextManager`:

```py
from collections.abc import Iterator
from contextlib import contextmanager

@contextmanager
def foo() -> Iterator[str]:
    yield "foo"
```

To understand decorators properly we'll need to understand `typing.Callable`, which we don't yet. For now, `Unknown` is a better type to infer if a function has any decorators.

I changed the `FunctionType::returns` method to return a `Type<'db>` as part of this PR (rather than an `Option<Type<'db>>`, as the distinction between an annotated function and an unannotated function becomes much muddier when you consider decorators: if the function is decorated, it's the type of _decorator_ that becomes important as to whether we can infer the return type just from looking at the signatures, rather than the type of the function being passed into the decorator.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2024-09-01 11:06_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-01 11:06_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-01 11:06_

---

_Comment by @github-actions[bot] on 2024-09-01 11:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-09-01 15:17_

Looks good!

---

_Merged by @AlexWaygood on 2024-09-01 15:19_

---

_Closed by @AlexWaygood on 2024-09-01 15:19_

---

_Branch deleted on 2024-09-01 15:19_

---
