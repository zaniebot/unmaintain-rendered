```yaml
number: 11919
title: Fix Fuzz build
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: fix-cargo-fuzz
created_at: 2024-06-18T06:29:44Z
updated_at: 2024-06-18T07:00:10Z
url: https://github.com/astral-sh/ruff/pull/11919
synced_at: 2026-01-10T21:56:00Z
```

# Fix Fuzz build

---

_Pull request opened by @MichaReiser on 2024-06-18 06:29_

## Summary

Try to fix the Fuzz CI Job.

I'm not sure how and why it worked before because we always used the MUSL targets from the GitHub releases page. 

An alternative could be to add the Rust musl target, but I ran into issues locally where the MUSL target requires more dependencies.

## Test Plan

CI?


---

_Label `ci` added by @MichaReiser on 2024-06-18 06:29_

---

_Comment by @codspeed-hq[bot] on 2024-06-18 06:34_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/fix-cargo-fuzz)

### Merging #11919 will **improve performances by 9.82%**

<sub>Comparing <code>fix-cargo-fuzz</code> (8b5194d) with <code>main</code> (c53d55a)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `fix-cargo-fuzz` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 775.3 µs | 706 µs | +9.82% |


---

_@dhruvmanila reviewed on 2024-06-18 06:44_

Thank you!

---

_Merged by @MichaReiser on 2024-06-18 06:44_

---

_Closed by @MichaReiser on 2024-06-18 06:44_

---

_Branch deleted on 2024-06-18 06:44_

---

_Comment by @github-actions[bot] on 2024-06-18 07:00_

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
