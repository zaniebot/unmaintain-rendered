```yaml
number: 10525
title: "Track ranges of names inside `__all__` definitions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: dunder-all-ranges
created_at: 2024-03-22T13:23:54Z
updated_at: 2024-03-22T18:38:41Z
url: https://github.com/astral-sh/ruff/pull/10525
synced_at: 2026-01-12T15:55:32Z
```

# Track ranges of names inside `__all__` definitions

---

_@AlexWaygood_

## Summary

This resolves the TODO comment here:

https://github.com/astral-sh/ruff/blob/4f06d59ff6f58f2d13c2f8fcfe375b81058f87c3/crates/ruff_linter/src/checkers/ast/mod.rs#L2116-L2118

Fixing this TODO is necessary (though not _quite_ sufficient) for fixing #10508.

This PR adds a new struct for representing `__all__` members to `crates/ruff_python_ast/all.rs`:

```rs
struct DunderAllMember<'a> {
    name: &'a str,
    range: TextRange
}
```

I initially just kept references to `ExprStringLiteral` nodes around, since they have all the information we need here, but apparently that approach leads to a large performance regression 🙃

## Test Plan

`cargo test`


---

_Comment by @codspeed-hq[bot] on 2024-03-22 13:28_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dunder-all-ranges)

### Merging #10525 will **not alter performance**

<sub>Comparing <code>dunder-all-ranges</code> (a6fc510) with <code>main</code> (4f06d59)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-03-22 13:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Converted to draft by @AlexWaygood on 2024-03-22 15:33_

---

_Marked ready for review by @AlexWaygood on 2024-03-22 15:50_

---

_Comment by @AlexWaygood on 2024-03-22 15:51_

I fixed the performance regressions initially reported by codspeed, and edited the PR description to reflect the new approach.

---

_Label `bug` added by @MichaReiser on 2024-03-22 16:28_

---

_Label `fixes` added by @MichaReiser on 2024-03-22 16:28_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-03-22 16:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/definition.rs`:190 on 2024-03-22 16:31_

Nit: We could simplify this by changing `exports` to `&[DunderAllName]`. Call sites can pass an empty slice (or use `unwrap_or_default()` if there are no exports.

---

_@MichaReiser approved on 2024-03-22 16:31_

---

_@AlexWaygood reviewed on 2024-03-22 16:42_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/definition.rs`:190 on 2024-03-22 16:42_

Nice idea! Applied in https://github.com/astral-sh/ruff/pull/10525/commits/20e70c367a8a135989b55bc80e4f0cb4cb0734a6

---

_@AlexWaygood reviewed on 2024-03-22 16:51_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/definition.rs`:190 on 2024-03-22 16:51_

Hmm, that breaks things, and I think I know why: an empty export list (`__all__ = []`) is semantically different to a module not defining `__all__` at all. `__all__ = []` means there are no public re-exports from the module; not defining `__all__` at all means _everything_ is publicly re-exported from the module (unless it has a name starting with a single underscore).

---

_@MichaReiser reviewed on 2024-03-22 16:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/definition.rs`:190 on 2024-03-22 16:52_

That makes sense, sorry. Thanks for trying.

---

_@AlexWaygood reviewed on 2024-03-22 16:54_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/definition.rs`:190 on 2024-03-22 16:54_

No worries!!

---

_@charliermarsh reviewed on 2024-03-22 17:58_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/all.rs`:24 on 2024-03-22 17:58_

Nit: I'd suggest removing the `pub` from these accesses, and add a getter for `name`.

---

_@charliermarsh approved on 2024-03-22 17:58_

---

_Merged by @AlexWaygood on 2024-03-22 18:38_

---

_Closed by @AlexWaygood on 2024-03-22 18:38_

---

_Branch deleted on 2024-03-22 18:38_

---
