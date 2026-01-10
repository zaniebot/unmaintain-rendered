```yaml
number: 12881
title: "[red-knot] Use line/column for server diagnostics if available"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/diagnostic-range
created_at: 2024-08-14T06:56:09Z
updated_at: 2024-08-14T09:41:33Z
url: https://github.com/astral-sh/ruff/pull/12881
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Use line/column for server diagnostics if available

---

_Pull request opened by @dhruvmanila on 2024-08-14 06:56_

## Summary

This PR adds very basic support for using the line / column information from the diagnostic message. This makes it easier to validate diagnostics in an editor as oppose to going through the diff one diagnostic at a time and confirming it at the location.


---

_Label `red-knot` added by @dhruvmanila on 2024-08-14 06:56_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-14 06:56_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-14 06:56_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-14 06:56_

---

_Renamed from "[red-knot] Use line/column for server diagostics if available" to "[red-knot] Use line/column for server diagnostics if available" by @dhruvmanila on 2024-08-14 07:00_

---

_Comment by @codspeed-hq[bot] on 2024-08-14 07:09_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/diagnostic-range)

### Merging #12881 will **degrade performances by 5.04%**

<sub>Comparing <code>dhruv/diagnostic-range</code> (da13d33) with <code>main</code> (89c8b49)</sub>



### Summary

`⚡ 1` improvements
`❌ 1` regressions
`✅ 30` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/diagnostic-range)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/diagnostic-range` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/globals.py]` | 725 µs | 763.5 µs | -5.04% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.8 ms | +4.1% |


---

_@MichaReiser approved on 2024-08-14 07:10_

---

_Comment by @github-actions[bot] on 2024-08-14 07:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dhruvmanila on 2024-08-14 09:41_

---

_Closed by @dhruvmanila on 2024-08-14 09:41_

---

_Branch deleted on 2024-08-14 09:41_

---
