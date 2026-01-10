```yaml
number: 13016
title: Upgrade to Salsa with tables
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: upgrade-salsa
created_at: 2024-08-20T19:52:50Z
updated_at: 2024-08-21T07:13:16Z
url: https://github.com/astral-sh/ruff/pull/13016
synced_at: 2026-01-10T21:38:32Z
```

# Upgrade to Salsa with tables

---

_Pull request opened by @MichaReiser on 2024-08-20 19:52_

## Summary
Upgrade to the latest salsa version

* [x] Use tagged version or use upstream
* [x] Fix const query assertion
* [x] Review salsa setter calls because they now preserve durability by default

## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2024-08-20 19:53_

---

_Comment by @codspeed-hq[bot] on 2024-08-20 19:58_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/upgrade-salsa)

### Merging #13016 will **improve performances by 27.18%**

<sub>Comparing <code>upgrade-salsa</code> (c82e041) with <code>main</code> (678045e)</sub>



### Summary

`⚡ 2` improvements
`✅ 30` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `upgrade-salsa` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[numpy/globals.py]` | 181.2 µs | 170.1 µs | +6.52% |
| ⚡ | `red_knot_check_file[incremental]` | 3.1 ms | 2.4 ms | +27.18% |


---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace.rs`:147 on 2024-08-20 20:08_

We no longer need to specify the durability if the durability is the same as when the input was constructed (see https://github.com/salsa-rs/salsa/pull/559)

---

_@MichaReiser reviewed on 2024-08-20 20:08_

---

_Comment by @github-actions[bot] on 2024-08-20 20:26_

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

_Marked ready for review by @MichaReiser on 2024-08-21 06:53_

---

_Review requested from @carljm by @MichaReiser on 2024-08-21 06:53_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-21 06:53_

---

_Merged by @MichaReiser on 2024-08-21 06:58_

---

_Closed by @MichaReiser on 2024-08-21 06:58_

---

_Branch deleted on 2024-08-21 06:58_

---
