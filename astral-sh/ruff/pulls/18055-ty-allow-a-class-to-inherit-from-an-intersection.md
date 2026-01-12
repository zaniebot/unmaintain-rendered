```yaml
number: 18055
title: "[ty] Allow a class to inherit from an intersection if the intersection contains a dynamic type and the intersection is not disjoint from `type`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/invalid-base-intersections
created_at: 2025-05-12T20:16:06Z
updated_at: 2025-05-12T23:07:53Z
url: https://github.com/astral-sh/ruff/pull/18055
synced_at: 2026-01-12T15:56:11Z
```

# [ty] Allow a class to inherit from an intersection if the intersection contains a dynamic type and the intersection is not disjoint from `type`

---

_@AlexWaygood_

## Summary

Stacked on top of #18053. This fixes a new false positive I can see in the primer diff on that PR, and I think it's a good change to make anyway!

## Test Plan

mdtests added


---

_Review requested from @carljm by @AlexWaygood on 2025-05-12 20:16_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-12 20:16_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-12 20:16_

---

_Label `ty` added by @AlexWaygood on 2025-05-12 20:16_

---

_Comment by @github-actions[bot] on 2025-05-12 20:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
sympy (https://github.com/sympy/sympy)
+ warning[unused-ignore-comment] sympy/parsing/autolev/_listener_autolev_antlr.py:402:41: Unused blanket `type: ignore` directive
- error[invalid-base] sympy/utilities/decorator.py:319:27: Invalid class base with type `Unknown & <Protocol with members '__mro__'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)

```
</details>


---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class_base.rs`:111 on 2025-05-12 22:47_

This change looks good, but is there any reason it should be specific to a dynamic element? It seems like as long as the intersection is not disjoint from `type`, and we can find at least one element in it that is a valid base, we should be able to safely consider it that type and construct the base accordingly.

(Not saying that change needs to happen in this PR -- but the thought could maybe go into a comment, to help us out if it comes up in future.)

---

_@carljm approved on 2025-05-12 22:48_

Thank you!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:111 on 2025-05-12 23:03_

That's... an interesting thought. I'm not sure it has any practical consequences right now, because the only other "atomic" types we support as class bases currently are class-literal types and dynamic types. `<dynamic type> & whatever` is handled here. `<class literal type> & whatever` should always either simplify to `<class literal type>` or `Never`, since class-literal types only have a single inhabitant.

(We probably should allow users to inherit from objects of `type[Any]`, though, due to it [actually being an intersection](https://github.com/astral-sh/ty/issues/222). And if we did that, then yes, this would have more practical consequences. I can do that in a followup. And I'll tackle this at the same time!)

---

_@AlexWaygood reviewed on 2025-05-12 23:03_

---

_Merged by @AlexWaygood on 2025-05-12 23:07_

---

_Closed by @AlexWaygood on 2025-05-12 23:07_

---

_Branch deleted on 2025-05-12 23:07_

---
