```yaml
number: 13028
title: Remove linter dependency from red_knot_server
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-server-remove-linter
created_at: 2024-08-21T09:36:33Z
updated_at: 2024-08-21T10:17:12Z
url: https://github.com/astral-sh/ruff/pull/13028
synced_at: 2026-01-10T21:38:32Z
```

# Remove linter dependency from red_knot_server

---

_Pull request opened by @MichaReiser on 2024-08-21 09:36_

## Summary

This PR removes the `ruff_linter` dependency from `red_knot_server`. It should speed up the `red_knot` compilation time quiet a bit.

## Test Plan

`cargo build`


---

_Review requested from @carljm by @MichaReiser on 2024-08-21 09:36_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-21 09:36_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-08-21 09:36_

---

_Label `red-knot` added by @MichaReiser on 2024-08-21 09:36_

---

_@AlexWaygood approved on 2024-08-21 09:48_

If the `display_settings!` macro would be useful in red-knot, we could maybe move it to the `ruff_macros` crate?

---

_@dhruvmanila approved on 2024-08-21 09:49_

---

_Comment by @dhruvmanila on 2024-08-21 09:50_

> If the `display_settings!` macro would be useful in red-knot, we could maybe move it to the `ruff_macros` crate?

It isn't required right away but seems like a good idea.

---

_Merged by @MichaReiser on 2024-08-21 10:02_

---

_Closed by @MichaReiser on 2024-08-21 10:02_

---

_Branch deleted on 2024-08-21 10:02_

---

_Comment by @codspeed-hq[bot] on 2024-08-21 10:03_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/red-knot-server-remove-linter)

### Merging #13028 will **improve performances by 7.69%**

<sub>Comparing <code>red-knot-server-remove-linter</code> (ee7a227) with <code>main</code> (5c5dfc1)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `red-knot-server-remove-linter` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[numpy/globals.py]` | 184.5 µs | 171.3 µs | +7.69% |


---

_Comment by @github-actions[bot] on 2024-08-21 10:17_

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
