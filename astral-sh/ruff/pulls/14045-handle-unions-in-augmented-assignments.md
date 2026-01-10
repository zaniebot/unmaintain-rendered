```yaml
number: 14045
title: Handle unions in augmented assignments
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/bound-ii
created_at: 2024-11-01T16:01:09Z
updated_at: 2024-11-01T19:58:56Z
url: https://github.com/astral-sh/ruff/pull/14045
synced_at: 2026-01-10T20:59:37Z
```

# Handle unions in augmented assignments

---

_Pull request opened by @charliermarsh on 2024-11-01 16:01_

## Summary

Removing more TODOs from the augmented assignment test suite. Now, if the _target_ is a union, we correctly infer the union of results:

```python
if flag:
    f = Foo()
else:
    f = 42.0
f += 12
```


---

_Label `red-knot` added by @charliermarsh on 2024-11-01 16:01_

---

_Marked ready for review by @charliermarsh on 2024-11-01 16:01_

---

_Review requested from @carljm by @charliermarsh on 2024-11-01 16:01_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-11-01 16:01_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-11-01 16:01_

---

_Review requested from @sharkdp by @charliermarsh on 2024-11-01 16:01_

---

_Comment by @github-actions[bot] on 2024-11-01 16:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-11-01 16:15_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:1538 on 2024-11-01 16:15_

I feel like there may still be a missing case here... Let's see if I can articulate it after his meeting.

---

_@charliermarsh reviewed on 2024-11-01 16:24_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:1538 on 2024-11-01 16:24_

```python
def bool_instance() -> bool:
    return True

flag = bool_instance()

class Foo:
    def __add__(self, other: int) -> str:
        return "Hello, world!"
    if bool_instance():
        def __iadd__(self, other: int) -> int:
            return 42


class Bar:
    def __add__(self, other: int) -> bool:
        return True

    def __iadd__(self, other: int) -> float:
        return 42.0
        
if flag:
    f = Foo()
else:
    f = Bar()
f += 12
```

I was thinking of something like this, where we don't know _which_ part of a union was unbound, and so we incorrectly include the `__add__` types for the _bound_ part of the union too... But this case is actually handled correctly right now.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:181 on 2024-11-01 17:08_

nit: I think this test will be clearer if you use a type other than `bool` here, since `bool` is a subtype of `int` and thus gets simplified out of the eventual union, which is a detail that's kind of extraneous to this test -- the point here is that all four types should be possible. I suggest `bytes`:

```suggestion
    def __add__(self, other: int) -> bytes:
        return True

    def __iadd__(self, other: int) -> float:
        return 42.0
        
if flag:
    f = Foo()
else:
    f = Bar()
f += 12

reveal_type(f)  # revealed: int | str | float | bytes
```

(order might be wrong on this assertion)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1538 on 2024-11-01 17:09_

Worth adding a test for this, if you weren't sure?

---

_@carljm approved on 2024-11-01 17:18_

Looks good, thank you!

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:1538 on 2024-11-01 19:40_

I did! `## Partially bound target union with __add__`

---

_@charliermarsh reviewed on 2024-11-01 19:40_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/resources/mdtest/assignment/augmented.md`:181 on 2024-11-01 19:40_

Thanks!

---

_@charliermarsh reviewed on 2024-11-01 19:40_

---

_Merged by @charliermarsh on 2024-11-01 19:49_

---

_Closed by @charliermarsh on 2024-11-01 19:49_

---

_Branch deleted on 2024-11-01 19:49_

---
