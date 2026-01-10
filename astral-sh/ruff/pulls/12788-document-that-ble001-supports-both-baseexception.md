```yaml
number: 12788
title: Document that BLE001 supports both BaseException and Exception
type: pull_request
state: merged
author: rhoban13
labels:
  - documentation
assignees: []
merged: true
base: main
head: document_blind_except_supports_Exception
created_at: 2024-08-09T14:42:41Z
updated_at: 2024-08-09T15:28:50Z
url: https://github.com/astral-sh/ruff/pull/12788
synced_at: 2026-01-10T21:47:02Z
```

# Document that BLE001 supports both BaseException and Exception

---

_Pull request opened by @rhoban13 on 2024-08-09 14:42_

## Summary

Document that `flake8-blind-except` supports `Exception` as well as `BaseException`.

## Test Plan

Locally ran mkdocs.


---

_Comment by @rhoban13 on 2024-08-09 14:46_

Motivated by my confusion in https://github.com/astral-sh/ruff/issues/970#issuecomment-2276014197

---

_Comment by @codspeed-hq[bot] on 2024-08-09 14:51_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/rhoban13:document_blind_except_supports_Exception)

### Merging #12788 will **improve performances by 4.15%**

<sub>Comparing <code>rhoban13:document_blind_except_supports_Exception</code> (ee59439) with <code>main</code> (a176679)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `rhoban13:document_blind_except_supports_Exception` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.8 ms | +4.15% |


---

_Comment by @github-actions[bot] on 2024-08-09 14:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-08-09 15:28_

Thanks!

---

_Label `documentation` added by @MichaReiser on 2024-08-09 15:28_

---

_Merged by @MichaReiser on 2024-08-09 15:28_

---

_Closed by @MichaReiser on 2024-08-09 15:28_

---
