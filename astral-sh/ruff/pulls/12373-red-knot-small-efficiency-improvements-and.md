```yaml
number: 12373
title: "[red-knot] small efficiency improvements and bugfixes to use-def map building"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/use-def-improvements
created_at: 2024-07-17T23:40:38Z
updated_at: 2024-07-18T16:33:19Z
url: https://github.com/astral-sh/ruff/pull/12373
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] small efficiency improvements and bugfixes to use-def map building

---

_Pull request opened by @carljm on 2024-07-17 23:40_

Adds inference tests sufficient to give full test coverage of the `UseDefMapBuilder::merge` method.

In the process I realized that we could implement visiting of if statements in `SemanticBuilder` with fewer `snapshot`, `restore`, and `merge` operations, so I restructured that visit a bit.

I also found one correctness bug in the `merge` method (it failed to extend the given snapshot with "unbound" for any missing symbols, meaning we would just lose the fact that the symbol could be unbound in the merged-in path), and two efficiency bugs (if one of the ranges to merge is empty, we can just use the other one, no need for copies, and if the ranges are overlapping -- which can occur with nested branches -- we can still just merge them with no copies), and fixed all three.

---

_Label `red-knot` added by @carljm on 2024-07-17 23:40_

---

_Review requested from @MichaReiser by @carljm on 2024-07-17 23:40_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-17 23:40_

---

_Comment by @github-actions[bot] on 2024-07-17 23:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:301 on 2024-07-18 06:31_

Nit: (and `snapshot`?)
```suggestion
            let current = &mut current.definitions_range;
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:306 on 2024-07-18 06:32_

I think these checks are unnecessary because they are enforced by the `Range` constructor.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:314 on 2024-07-18 06:32_


```suggestion
                *cur = snap.clone();
```

The same everywhere else where you assign to the range or don't take a `&mut` for `cur`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:318 on 2024-07-18 06:35_


```suggestion
                    cur.start.min(snap.start)..cur.end.max(snap.end);
```

---

_@MichaReiser approved on 2024-07-18 06:37_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:413 on 2024-07-18 06:39_

Nit: Or, to avoid a write in every loop and make it a constant access instead

```suggestion
                let has_else = node.elif_else_clauses.last().is_some_and(|clause| clause.test.is_none());
                if !has_else {
```

---

_@MichaReiser reviewed on 2024-07-18 06:39_

---

_Comment by @MichaReiser on 2024-07-18 06:40_

We should extend our benchmarks with code that has some control flow or we won't know if any of those improvements are any good (separate PR)

---

_@carljm reviewed on 2024-07-18 14:59_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:301 on 2024-07-18 14:59_

I don't like shadowing, I find it confusing :/ I'll try to find a solution that doesn't involve either shadowing or abbreviations :)

---

_@carljm reviewed on 2024-07-18 15:14_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:306 on 2024-07-18 15:14_

It looks like they aren't? If I do `let rev_range = 5..2;` then `dbg!(rev_range);` shows `rev_range = 5..2`. If you iterate such a range, it's empty, but it seems like you are allowed to create one.

May not be necessary to check but the logic below definitely depends on it, so I thought it didn't hurt.

---

_@carljm reviewed on 2024-07-18 16:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:301 on 2024-07-18 16:04_

Ok, I got over it and just shadowed. I think it's not too bad: we shift gears to merging the definition ranges, and from that point on `current` and `snapshot` refer to just the ranges.

---

_@MichaReiser reviewed on 2024-07-18 16:06_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:301 on 2024-07-18 16:06_

I disliked shadowing in JS but it's more common in the Rust world and I kind of got used to it.

---

_Merged by @carljm on 2024-07-18 16:24_

---

_Closed by @carljm on 2024-07-18 16:24_

---

_Branch deleted on 2024-07-18 16:25_

---

_@AlexWaygood reviewed on 2024-07-18 16:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:301 on 2024-07-18 16:33_

I _hate_ shadowing in Python, but actually kinda like it in Rust. It's impossible to get confused about what the type of a symbol is in Rust because the compiler immediately starts screaming at you (so the major drawback is much less, in my opinion). And it's nice to keep the number of variables in any given function to a minimum; it sometimes makes the logic easier to understand, I think.

---
