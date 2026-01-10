```yaml
number: 13978
title: Separate type check diagnostics builder
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/diagnostics-builder
created_at: 2024-10-29T11:11:59Z
updated_at: 2024-10-30T18:59:50Z
url: https://github.com/astral-sh/ruff/pull/13978
synced_at: 2026-01-10T20:59:37Z
```

# Separate type check diagnostics builder

---

_Pull request opened by @dhruvmanila on 2024-10-29 11:11_

## Summary

This PR creates a new `TypeCheckDiagnosticsBuilder` for the `TypeCheckDiagnostics` struct. The main motivation behind this is to separate the helpers required to build the diagnostics from the type inference builder itself. This allows us to use such helpers outside of the inference builder like for example in the unpacking logic in https://github.com/astral-sh/ruff/pull/13979.

## Test Plan

`cargo insta test`


---

_Label `red-knot` added by @dhruvmanila on 2024-10-29 11:11_

---

_Comment by @github-actions[bot] on 2024-10-30 08:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2024-10-30 10:11_

---

_Review comment by @dhruvmanila on `crates/ruff_benchmark/benches/red_knot.rs`:34 on 2024-10-30 10:11_

I couldn't figure out why the order of the diagnostics changed here as nothing obvious stands out to me.

---

_Marked ready for review by @dhruvmanila on 2024-10-30 10:12_

---

_Review requested from @carljm by @dhruvmanila on 2024-10-30 10:12_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-10-30 10:12_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-10-30 10:12_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1648 on 2024-10-30 10:44_

Nit: I suggest prefixing the builder methods with `add` because I otherwise expect that the `builder` returns the diagnostic. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3472 on 2024-10-30 10:52_

I think what's different now is that `extend` adds the diagnostics to `self.types.diagnostics` and `finish` merges the two. 

We should not write to `types.diagnostics` and instead push all diagnostics to the `DiagnosticsBuilder` (we otherwise risk allocating more vecs)
```suggestion
        self.types.diagnostics. = self.diagnostics.finish();
```

---

_@MichaReiser reviewed on 2024-10-30 10:52_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:3472 on 2024-10-30 10:56_

I don't think we can directly override the `self.types.diagnostics` because otherwise it'll drop the existing diagnostics that's present. This happens because the inference builder is mutually recursive.

Did you mean the following?
```rs
self.diagnostics.extend(self.diagnostics.finish());
```

---

_@dhruvmanila reviewed on 2024-10-30 10:57_

---

_@MichaReiser reviewed on 2024-10-30 11:23_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3472 on 2024-10-30 11:23_

types.diagnostics should be empty at  this point. If it isn't (e.g because of extend), then these code paths should be updated as well to push the diagnostics to the builder instead. Pushing some diagnostics to the builder and some to types.diagnostics is the source of the reordering 

---

_@MichaReiser reviewed on 2024-10-30 11:24_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3472 on 2024-10-30 11:24_

We could argue that the reordering doesn't matter and we should instead sort the diagnostics but it still has the disadvantage that we allocate one extra vec when the diagnostics are not empty 

---

_@dhruvmanila reviewed on 2024-10-30 11:27_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:3472 on 2024-10-30 11:27_

Oh ok, got it. I see the issue.

---

_@dhruvmanila reviewed on 2024-10-30 11:31_

---

_Review comment by @dhruvmanila on `crates/ruff_benchmark/benches/red_knot.rs`:34 on 2024-10-30 11:31_

This is resolved in https://github.com/astral-sh/ruff/pull/13978/commits/8d3a2ff5ee818405fbd7f2c28ed37d6b58a741d0

---

_@MichaReiser approved on 2024-10-30 12:45_

Nice!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:238 on 2024-10-30 18:27_

This maybe duplicates the TODO below, but it seems confusing for the docstring to claim behavior that isn't implemented yet:
```suggestion
    /// TODO: the diagnostic does not get added if the rule isn't enabled for this file.
```

---

_@carljm approved on 2024-10-30 18:28_

This is great!

---

_@MichaReiser reviewed on 2024-10-30 18:31_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:238 on 2024-10-30 18:31_

I would leave it as is. It works correctly because the diagnostic gets omitted for third-party code (the rule is not enabled). There's just no way to disable rules for first-party files and all rules are always enabled.

---

_Merged by @dhruvmanila on 2024-10-30 18:50_

---

_Closed by @dhruvmanila on 2024-10-30 18:50_

---

_Branch deleted on 2024-10-30 18:50_

---
