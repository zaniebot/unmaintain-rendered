```yaml
number: 14142
title: "[red-knot] a few metaclass cleanups"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/metaclass
created_at: 2024-11-06T21:35:50Z
updated_at: 2024-11-06T22:22:52Z
url: https://github.com/astral-sh/ruff/pull/14142
synced_at: 2026-01-12T15:55:46Z
```

# [red-knot] a few metaclass cleanups

---

_@carljm_

Just cleaning up a few small things I noticed in post-land review.

---

_Review requested from @MichaReiser by @carljm on 2024-11-06 21:35_

---

_Review requested from @AlexWaygood by @carljm on 2024-11-06 21:35_

---

_Review requested from @sharkdp by @carljm on 2024-11-06 21:35_

---

_Review requested from @charliermarsh by @carljm on 2024-11-06 21:35_

---

_@MichaReiser approved on 2024-11-06 21:42_

---

_Label `red-knot` added by @MichaReiser on 2024-11-06 21:42_

---

_@AlexWaygood reviewed on 2024-11-06 21:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:173 on 2024-11-06 21:43_

If the metaclass is a callable that returns the type `int`, that means the variable `A` created by the `class` statement will be an instance of the class `int`, which means `A.__class__` will be the `int` class itself, which means this revealed type should in fact be the meta-type of `int`, i.e...

```suggestion
# TODO should be `type[int]`
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2104 on 2024-11-06 21:44_

And also verify that it returns a type assignable to `Instance(type)`? I don't think we can realistically support `class` statements that don't create classes 

---

_@AlexWaygood reviewed on 2024-11-06 21:44_

---

_Comment by @github-actions[bot] on 2024-11-06 21:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-11-06 21:50_

Thanks

---

_@carljm reviewed on 2024-11-06 22:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/metaclass.md`:173 on 2024-11-06 22:01_

Ah yeah, good catch!

---

_@carljm reviewed on 2024-11-06 22:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2104 on 2024-11-06 22:01_

Honestly it probably wouldn't be that hard, but it's also likely not worth it.

---

_@AlexWaygood approved on 2024-11-06 22:11_

Thanks!

---

_Merged by @carljm on 2024-11-06 22:13_

---

_Closed by @carljm on 2024-11-06 22:13_

---

_Branch deleted on 2024-11-06 22:13_

---
