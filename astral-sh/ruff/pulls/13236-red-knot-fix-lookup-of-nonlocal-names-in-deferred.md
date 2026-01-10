```yaml
number: 13236
title: "[red-knot] fix lookup of nonlocal names in deferred annotations"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/deferred-lookup
created_at: 2024-09-03T22:07:50Z
updated_at: 2024-09-04T17:10:56Z
url: https://github.com/astral-sh/ruff/pull/13236
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] fix lookup of nonlocal names in deferred annotations

---

_Pull request opened by @carljm on 2024-09-03 22:07_

Initially I had deferred annotation name lookups reuse the "public symbol type", since that gives the correct "from end of scope" view of reaching definitions that we want. But there is a key difference; public symbol types are based only on definitions in the queried scope (or "name in the given namespace" in runtime terms), they don't ever look up a name in nonlocal/global/builtin scopes. Deferred annotation resolution should do this lookup.

Add a test, and fix deferred name resolution to support nonlocal/global/builtin names.

Fixes #13176 

---

_Review requested from @MichaReiser by @carljm on 2024-09-03 22:07_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-03 22:07_

---

_Label `red-knot` added by @carljm on 2024-09-03 22:07_

---

_Comment by @codspeed-hq[bot] on 2024-09-03 22:13_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm/deferred-lookup)

### Merging #13236 will **not alter performance**

<sub>Comparing <code>cjm/deferred-lookup</code> (5260214) with <code>main</code> (e965f9c)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-09-03 22:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1833 on 2024-09-04 09:39_

TIL I learned from @dhruvmanila. Sharing for you https://doc.rust-lang.org/std/error/index.html#common-message-styles 

---

_@MichaReiser approved on 2024-09-04 09:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3564 on 2024-09-04 10:21_

nit: I'd generally only use `panic!()` over `.expect()` if I wanted to give a dynamic error message

```suggestion

        let base = ty
            .expect_class()
            .bases(&db)
            .next()
            .expect("there should be at least one base");
```

---

_@AlexWaygood approved on 2024-09-04 10:24_

---

_Merged by @carljm on 2024-09-04 17:10_

---

_Closed by @carljm on 2024-09-04 17:10_

---

_Branch deleted on 2024-09-04 17:10_

---
