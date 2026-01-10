```yaml
number: 11784
title: "[red-knot] condense int literals"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/2unionliterals
created_at: 2024-06-06T21:34:00Z
updated_at: 2024-06-06T22:30:41Z
url: https://github.com/astral-sh/ruff/pull/11784
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] condense int literals

---

_Pull request opened by @carljm on 2024-06-06 21:34_

Display `(Literal[1] | Literal[2])` as `Literal[1, 2]`, and `(Literal[1] | Literal[2] | OtherType)` as `(Literal[1, 2] | OtherType)`.

Fixes #11782

---

_Label `red-knot` added by @carljm on 2024-06-06 21:34_

---

_Review requested from @AlexWaygood by @carljm on 2024-06-06 21:34_

---

_Review requested from @MichaReiser by @carljm on 2024-06-06 21:34_

---

_@AlexWaygood reviewed on 2024-06-06 21:34_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/semantic/types.rs`:793 on 2024-06-06 21:34_

```suggestion
// efficiency in the within-intersection case.
```

---

_Review comment by @AlexWaygood on `crates/red_knot/src/semantic/types.rs`:774 on 2024-06-06 21:37_

Why are we parenthesizing any of these displays, OOI? `Literal[1] | None` feels just as clear to me as `(Literal[1] | None)`

---

_Review comment by @AlexWaygood on `crates/red_knot/src/semantic/types.rs`:774 on 2024-06-06 21:41_

Could get rid of the need for the `first` variable here:

```suggestion
            for (i, num) in nums.iter().enumerate() {
                if i != 0 {
                    f.write_str(", ")?;
                }
                write!(f, "{num}")?;
            }
```

---

_@AlexWaygood approved on 2024-06-06 21:42_

Nice!

---

_@carljm reviewed on 2024-06-06 21:44_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/types.rs`:774 on 2024-06-06 21:44_

Yeah, I was doing this for clarity in case the union was embedded in other unions/intersections, but I think a) we should always normalize to disjunctive normal form (union of intersections), and b) even there, we should only add parens contextually as needed for clarity, not always. So I'll remove this.

---

_@carljm reviewed on 2024-06-06 21:46_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/types.rs`:774 on 2024-06-06 21:46_

We also need the `first` variable below, though, to know whether we added any literal type at all. Once you account for that, I don't think it ends up cleaner without the `first` variable.

---

_@AlexWaygood reviewed on 2024-06-06 21:47_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/semantic/types.rs`:774 on 2024-06-06 21:47_

Ah didn't spot that! Ignore this then :)

---

_Comment by @github-actions[bot] on 2024-06-06 21:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2024-06-06 22:30_

---

_Closed by @carljm on 2024-06-06 22:30_

---

_Branch deleted on 2024-06-06 22:30_

---
