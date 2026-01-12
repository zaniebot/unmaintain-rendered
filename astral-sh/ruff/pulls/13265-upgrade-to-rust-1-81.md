```yaml
number: 13265
title: Upgrade to Rust 1.81
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/rust-1.81
created_at: 2024-09-06T10:03:20Z
updated_at: 2024-09-06T13:09:10Z
url: https://github.com/astral-sh/ruff/pull/13265
synced_at: 2026-01-12T15:55:43Z
```

# Upgrade to Rust 1.81

---

_@MichaReiser_

## Summary

Upgrades to Rust [1.81](https://blog.rust-lang.org/2024/09/05/Rust-1.81.0.html).

I'm a bit worried about the change that an incorrect `Ord` implementation now panics when sorting instead of resulting in a messed-up sorting.  We discovered at least one such bug thanks to fuzzing. 

## Test Plan

`cargo test`


---

_Review requested from @dhruvmanila by @MichaReiser on 2024-09-06 10:03_

---

_Label `internal` added by @MichaReiser on 2024-09-06 10:03_

---

_Comment by @codspeed-hq[bot] on 2024-09-06 10:12_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha/rust-1.81)

### Merging #13265 will **degrade performances by 5.13%**

<sub>Comparing <code>micha/rust-1.81</code> (e74aea7) with <code>main</code> (a4ebe7d)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha/rust-1.81)._

### Benchmarks breakdown

|     | Benchmark | `main` | `micha/rust-1.81` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 1.8 ms | 1.9 ms | -5.13% |


---

_@dhruvmanila approved on 2024-09-06 10:17_

> I'm a bit worried about the change that an incorrect Ord implementation now panics when sorting instead of resulting in a messed-up sorting. We discovered at least one such bug thanks to fuzzing.

Curious to know which bug is this, not sure I see any changes in this PR that's relevant.

---

_Comment by @github-actions[bot] on 2024-09-06 10:27_

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

_Comment by @MichaReiser on 2024-09-06 11:15_

It's https://github.com/astral-sh/ruff/pull/12471

---

_@AlexWaygood approved on 2024-09-06 13:04_

---

_Merged by @MichaReiser on 2024-09-06 13:09_

---

_Closed by @MichaReiser on 2024-09-06 13:09_

---

_Branch deleted on 2024-09-06 13:09_

---
