```yaml
number: 17678
title: Upgrade Salsa to a more recent commit
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/upgrade-salsa
created_at: 2025-04-28T12:12:04Z
updated_at: 2025-04-28T12:32:24Z
url: https://github.com/astral-sh/ruff/pull/17678
synced_at: 2026-01-10T19:03:00Z
```

# Upgrade Salsa to a more recent commit

---

_Pull request opened by @AlexWaygood on 2025-04-28 12:12_

## Summary

This upgrades our Salsa pin to https://github.com/salsa-rs/salsa/commit/c75b0161aba55965ab6ad8cc9aaee7dc177967f1. It's not the latest commit on the Salsa `main` branch, but there's a compile error when building red-knot with the latest commit.

The motivation is that there have been some Salsa fixes recently that @MichaReiser speculated might help with some panics on my WIP branch for `typing.Protocol` support. But bumping Salsa to a more recent commit seems like a good thing to do anyway.

## Test Plan

`cargo build` / CI on this PR


---

_Label `red-knot` added by @AlexWaygood on 2025-04-28 12:12_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-04-28 12:12_

---

_Comment by @codspeed-hq[bot] on 2025-04-28 12:18_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fupgrade-salsa)

### Merging #17678 will **improve performances by 4.08%**

<sub>Comparing <code>alex/upgrade-salsa</code> (a6601fb) with <code>main</code> (92f95ff)</sub>



### Summary

`⚡ 1` improvements  
`✅ 32` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_check_file[cold] `` | 99.8 ms | 95.9 ms | +4.08% |


---

_Comment by @github-actions[bot] on 2025-04-28 12:22_

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

_@MichaReiser approved on 2025-04-28 12:30_

Nice perf wins :) Thank you

---

_Merged by @AlexWaygood on 2025-04-28 12:32_

---

_Closed by @AlexWaygood on 2025-04-28 12:32_

---

_Branch deleted on 2025-04-28 12:32_

---
