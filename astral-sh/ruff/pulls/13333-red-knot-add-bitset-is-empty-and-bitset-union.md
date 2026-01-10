```yaml
number: 13333
title: "[red-knot] add BitSet::is_empty and BitSet::union"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/bitset
created_at: 2024-09-12T02:21:56Z
updated_at: 2024-09-12T18:25:47Z
url: https://github.com/astral-sh/ruff/pull/13333
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] add BitSet::is_empty and BitSet::union

---

_Pull request opened by @carljm on 2024-09-12 02:21_

Add `::is_empty` and `::union` methods to the `BitSet` implementation.

Allowing unused for now, until these methods become used later with the declared-types implementation.


---

_Label `red-knot` added by @carljm on 2024-09-12 02:22_

---

_Comment by @github-actions[bot] on 2024-09-12 03:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @carljm on 2024-09-12 03:05_

---

_Review requested from @MichaReiser by @carljm on 2024-09-12 03:05_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-12 03:05_

---

_@AlexWaygood approved on 2024-09-12 09:18_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:35 on 2024-09-12 11:37_

I'm not sure if we can use it already because of our minimal msrv but this is a perfect use case for the new [`[#expect]`](https://blog.rust-lang.org/2024/09/05/Rust-1.81.0.html#expectlint) attribute.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:112 on 2024-09-12 11:38_

```suggestion
        for (my_block, other_block) in self.blocks_mut().iter_mut().zip(other_blocks) {
            *my_block |= other_block
        }
```

---

_@MichaReiser approved on 2024-09-12 11:38_

---

_@carljm reviewed on 2024-09-12 13:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:35 on 2024-09-12 13:15_

It is! But I probably won't change it now for this case, since I already have the PR up that removes it again, so there isn't really a risk of forgetting, and changing it would just require messing with two different PRs for the exact same end result :)

Will keep it in mind for next time!

---

_Merged by @carljm on 2024-09-12 18:25_

---

_Closed by @carljm on 2024-09-12 18:25_

---

_Branch deleted on 2024-09-12 18:25_

---
