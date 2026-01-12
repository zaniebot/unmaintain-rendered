```yaml
number: 13177
title: "[red-knot] support non-local name lookups"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: nonlocal
created_at: 2024-08-31T00:26:51Z
updated_at: 2024-09-03T21:18:07Z
url: https://github.com/astral-sh/ruff/pull/13177
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] support non-local name lookups

---

_@carljm_

Add support for non-local name lookups.

There's one TODO around annotated assignments without a RHS; these need a fair amount of attention, which they'll get in an upcoming PR about declared vs inferred types.

Fixes #11663 


---

_Label `red-knot` added by @carljm on 2024-08-31 00:26_

---

_Review requested from @MichaReiser by @carljm on 2024-08-31 00:26_

---

_Review requested from @AlexWaygood by @carljm on 2024-08-31 00:26_

---

_Comment by @codspeed-hq[bot] on 2024-08-31 00:32_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/nonlocal)

### Merging #13177 will **not alter performance**

<sub>Comparing <code>nonlocal</code> (28e4e1d) with <code>main</code> (29c36a5)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @carljm on 2024-08-31 00:38_

Getting closer to zero bogus diagnostics in tomllib!

---

_Comment by @github-actions[bot] on 2024-08-31 00:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:24 on 2024-08-31 17:18_

I think this comment is still accurate: this still is the reason why we're emitting the bogus `Module 'collections.abc' has no member 'Iterable'` diagnostic

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1848 on 2024-08-31 17:31_

Does this handle e.g.

```pycon
>>> class Foo:
...     x = 1
...     y = [x * 2 for x in range(5)]
... 
>>> Foo.y
[0, 2, 4, 6, 8]
```

?

---

_@AlexWaygood reviewed on 2024-08-31 17:32_

Looks great. One question about comprehensions in class scopes. In general, it might be great to add some tests with comprehensions (and comprehensions inside comprehensions?)

---

_@carljm reviewed on 2024-09-01 15:19_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:24 on 2024-09-01 15:19_

Yeah, I found it more confusing than illuminating because it talks about "unresolved import" when that wording isn't used in any error mesage, but I can re-add it with different wording (and maybe immediately above the relevant error.)

---

_@AlexWaygood reviewed on 2024-09-01 15:21_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:24 on 2024-09-01 15:21_

yeah, the comment got outdated when I changed the error message. But it's still relevant.

The reason why it's not immediately above the relevant error is it's really nice to be able to copy and paste the actual diagnostics from CI straight into the file when the assertion fails :P

---

_@carljm reviewed on 2024-09-01 15:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1848 on 2024-09-01 15:22_

Is that the example you intended to give? I think that example isn't really relevant to this PR; `x` is a defined local inside the comprehension, so there's no nonlocal name reference; the `x = 1` is irrelevant, and this PR shouldn't change anything about the inference.

I think everything in this PR _should_ work correctly for comprehensions; they're just another function-like scope. I'll add some tests to be sure.

---

_@AlexWaygood reviewed on 2024-09-01 15:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1848 on 2024-09-01 15:23_

Argh. You're quite right. Sorry for the noise!

---

_@AlexWaygood approved on 2024-09-01 15:26_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1841 on 2024-09-02 10:49_

Nit: Add a `is_global()` function to `FileScopeId`

---

_@MichaReiser approved on 2024-09-02 10:51_

Nice and thanks for writing the extensive comments and referring to the runtime semantics.

---

_Merged by @carljm on 2024-09-03 21:18_

---

_Closed by @carljm on 2024-09-03 21:18_

---

_Branch deleted on 2024-09-03 21:18_

---
