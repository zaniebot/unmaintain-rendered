```yaml
number: 14744
title: "[red-knot] Simplify tuples containing `Never`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/simplify-tuple-containing-never
created_at: 2024-12-02T21:51:41Z
updated_at: 2024-12-03T11:29:19Z
url: https://github.com/astral-sh/ruff/pull/14744
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Simplify tuples containing `Never`

---

_Pull request opened by @sharkdp on 2024-12-02 21:51_

## Summary

Simplify tuples containing `Never` to `Never`:

```py
from typing import Never

def never() -> Never: ...

reveal_type((1, never(), "foo"))  # revealed: Never
```

I should note that mypy and pyright do *not* perform this simplification. I don't know why.


There is [only one place](https://github.com/astral-sh/ruff/blob/5137fcc9c81610f687b6cb45413ef83c2c5eea73/crates/red_knot_python_semantic/src/types/infer.rs#L1477-L1484) where we use `TupleType::new` directly (instead of `Type::tuple`, which changes behavior here). This appears when creating `TypeVar` constraints, and it looks to me like it should stay this way, because we're using `TupleType` to store a list of constraints there, instead of an actual type. We also store `tuple[constraint1, constraint2, …]` as the type for the `constraint1, constraint2, …` tuple expression. This would mean that we infer a type of `tuple[str, Never]` for the following type variable constraints, without simplifying it to `Never`. This seems like a weird edge case that's maybe not worth looking further into?!
```py
from typing import Never

#         vvvvvvvvvv
def f[T: (str, Never)](x: T):
    pass
```

## Test Plan

- Added a new unit test. Did not add additional Markdown tests as that seems superfluous.
- Tested the example above using red knot, mypy, pyright.
- Verified that this allows us to remove `contains_never` from the property tests (https://github.com/astral-sh/ruff/pull/14178#discussion_r1866473192)

---

_Label `red-knot` added by @sharkdp on 2024-12-02 21:51_

---

_Review requested from @carljm by @sharkdp on 2024-12-02 21:51_

---

_Review requested from @MichaReiser by @sharkdp on 2024-12-02 21:51_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-02 21:51_

---

_Comment by @github-actions[bot] on 2024-12-02 21:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2024-12-02 22:13_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3021 on 2024-12-02 22:13_

```suggestion
            if ty.is_never() {
```

---

_@carljm approved on 2024-12-02 23:08_

Looks great!

---

_Merged by @sharkdp on 2024-12-03 07:28_

---

_Closed by @sharkdp on 2024-12-03 07:28_

---

_Branch deleted on 2024-12-03 07:28_

---

_@AlexWaygood reviewed on 2024-12-03 10:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:582 on 2024-12-03 10:52_

nit: this method now feels a bit redundant. I'd probably have got rid of it and changed the callsites to use the new `TupleType::from_elements()` method

---

_Comment by @AlexWaygood on 2024-12-03 11:28_

> I should note that mypy and pyright do _not_ perform this simplification. I don't know why.

Mypy in general appears not to do much eager simplification at all, e.g. it [doesn't even simplify this union](https://mypy-play.net/?mypy=latest&python=3.12&gist=ed550c85c7b4ec2fcddf9ffb4e14621d):

```py
def f(x: int | int):
    reveal_type(x)  # mypy: revealed type is "Union[builtins.int, builtins.int]"
```

Pyright does a lot more eager simplification than mypy. But it's important to note, I think, that it's only relatively recently that we formalized the idea that `Never`/`NoReturn` constitute the bottom type in Python. Mypy has historically always treated it as such, but I think pyright only started treating it as such [relatively recently](https://github.com/microsoft/pyright/issues/2921). So the issue of simplification may just not have been considered. This is just speculation, though!

---
