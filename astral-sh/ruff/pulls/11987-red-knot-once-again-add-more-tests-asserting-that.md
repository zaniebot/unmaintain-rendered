```yaml
number: 11987
title: "[red-knot] Once again, add more tests asserting that the `VendoredFileSystem` and the `VERSIONS` parser work with the vendored typeshed stubs"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: retry-more-typeshed-tests
created_at: 2024-06-23T13:45:52Z
updated_at: 2024-06-23T14:04:27Z
url: https://github.com/astral-sh/ruff/pull/11987
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] Once again, add more tests asserting that the `VendoredFileSystem` and the `VERSIONS` parser work with the vendored typeshed stubs

---

_Pull request opened by @AlexWaygood on 2024-06-23 13:45_

This reapplies https://github.com/astral-sh/ruff/pull/11970, which was previously reverted in https://github.com/astral-sh/ruff/pull/11975 as the tests failed on Windows. The tests should now pass on Windows thanks to https://github.com/astral-sh/ruff/pull/11982; but if they do fail again, it should be easier to debug them now, thanks to https://github.com/astral-sh/ruff/pull/11983.

---

_Label `red-knot` added by @AlexWaygood on 2024-06-23 13:45_

---

_Comment by @codspeed-hq[bot] on 2024-06-23 13:54_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/retry-more-typeshed-tests)

### Merging #11987 will **improve performances by 5.06%**

<sub>Comparing <code>retry-more-typeshed-tests</code> (e9b377c) with <code>main</code> (92b145e)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `retry-more-typeshed-tests` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.8 ms | +5.06% |


---

_Merged by @AlexWaygood on 2024-06-23 13:57_

---

_Closed by @AlexWaygood on 2024-06-23 13:57_

---

_Branch deleted on 2024-06-23 13:57_

---

_Comment by @github-actions[bot] on 2024-06-23 14:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
