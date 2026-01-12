```yaml
number: 11875
title: Upgrade to Rust 1.79
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: rust-1.79
created_at: 2024-06-14T13:46:32Z
updated_at: 2024-06-17T06:15:11Z
url: https://github.com/astral-sh/ruff/pull/11875
synced_at: 2026-01-12T15:55:39Z
```

# Upgrade to Rust 1.79

---

_@MichaReiser_

## Summary

Use [Rust 1.79](https://blog.rust-lang.org/2024/06/13/Rust-1.79.0.html) as local development version and in the release pipeline. 

1.79 has no breaking platform support change.

## Test Plan

`cargo build`


---

_Review requested from @carljm by @MichaReiser on 2024-06-14 13:46_

---

_Comment by @MichaReiser on 2024-06-14 13:46_

Let's wait with merging this until **after** the release

---

_Label `internal` added by @MichaReiser on 2024-06-14 13:46_

---

_@MichaReiser reviewed on 2024-06-14 13:47_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:935 on 2024-06-14 13:47_

I had to remove this because Rust can now prove that this is always true, lol. I think the remainder of the test still captures the assertions well enough.

---

_Comment by @codspeed-hq[bot] on 2024-06-14 13:52_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/rust-1.79)

### Merging #11875 will **improve performances by 4.22%**

<sub>Comparing <code>rust-1.79</code> (309a696) with <code>main</code> (d681a45)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `rust-1.79` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[large/dataset.py]` | 19.4 ms | 18.6 ms | +4.22% |


---

_@AlexWaygood reviewed on 2024-06-14 14:05_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:935 on 2024-06-14 14:05_

😭

---

_Comment by @github-actions[bot] on 2024-06-14 14:08_

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

_@AlexWaygood approved on 2024-06-14 14:11_

---

_Merged by @MichaReiser on 2024-06-17 06:15_

---

_Closed by @MichaReiser on 2024-06-17 06:15_

---

_Branch deleted on 2024-06-17 06:15_

---
