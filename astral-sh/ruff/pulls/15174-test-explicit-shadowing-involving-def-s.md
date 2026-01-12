```yaml
number: 15174
title: "Test explicit shadowing involving `def`s"
type: pull_request
state: merged
author: hauntsaninja
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: test-def
created_at: 2024-12-29T00:11:57Z
updated_at: 2024-12-29T00:48:43Z
url: https://github.com/astral-sh/ruff/pull/15174
synced_at: 2026-01-12T15:55:50Z
```

# Test explicit shadowing involving `def`s

---

_@hauntsaninja_

_No description provided._

---

_Review requested from @carljm by @hauntsaninja on 2024-12-29 00:11_

---

_Review requested from @MichaReiser by @hauntsaninja on 2024-12-29 00:11_

---

_Review requested from @AlexWaygood by @hauntsaninja on 2024-12-29 00:11_

---

_Review requested from @sharkdp by @hauntsaninja on 2024-12-29 00:11_

---

_Label `testing` added by @AlexWaygood on 2024-12-29 00:13_

---

_Label `red-knot` added by @AlexWaygood on 2024-12-29 00:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/shadowing/function.md`:48 on 2024-12-29 00:20_

Including annotations on the second and third definitions of `x` but not the first one is potentially confusing and not really related to the test. Also `-> int: ...` in a non-stub is likely to be a type error in future. So I'd be inclined to remove all the parameter and return annotations here? If we need a separate test to show that presence or absence of annotations doesn't affect whether a `def` statement is a declaration or not, we can add that as a separate test, with explicit commentary.
```suggestion
def f(): ...

reveal_type(f)  # revealed: Literal[f]

f: int = 1
reveal_type(f)  # revealed: Literal[1]

def f(): ...
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/shadowing/function.md`:33 on 2024-12-29 00:21_

We support `reveal_type` as a built-in, so it's not necessary to import it, and we usually don't in tests.
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/shadowing/function.md`:30 on 2024-12-29 00:22_

```suggestion
## Explicit shadowing involving `def` statements

Since a `def` statement is a declaration, one `def` can shadow another `def`, or shadow a previous non-`def` declaration, without error.

```

---

_Comment by @github-actions[bot] on 2024-12-29 00:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm reviewed on 2024-12-29 00:23_

Thank you!

---

_@hauntsaninja reviewed on 2024-12-29 00:40_

---

_Review comment by @hauntsaninja on `crates/red_knot_python_semantic/resources/mdtest/shadowing/function.md`:48 on 2024-12-29 00:40_

I kept one of these with annotations, to show that re-declaring a `def` with an incompatible type is not an error.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/shadowing/function.md`:34 on 2024-12-29 00:40_

Explicit path isn't required, and we usually don't include it unless it is relevant to the test; default filename is `test.py`.

```suggestion
```py
```

---

_@carljm reviewed on 2024-12-29 00:40_

---

_@carljm reviewed on 2024-12-29 00:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/shadowing/function.md`:48 on 2024-12-29 00:42_

Oh! Good point, makes sense.

---

_@carljm approved on 2024-12-29 00:43_

---

_Merged by @carljm on 2024-12-29 00:47_

---

_Closed by @carljm on 2024-12-29 00:47_

---

_Branch deleted on 2024-12-29 00:48_

---
