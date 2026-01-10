```yaml
number: 9567
title: "[flake8-pyi] Fix PYI049 false negatives on call-based TypedDicts"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: main
head: y049-functional
created_at: 2024-01-17T17:14:56Z
updated_at: 2024-01-18T10:08:46Z
url: https://github.com/astral-sh/ruff/pull/9567
synced_at: 2026-01-10T22:57:09Z
```

# [flake8-pyi] Fix PYI049 false negatives on call-based TypedDicts

---

_Pull request opened by @AlexWaygood on 2024-01-17 17:14_

## Summary

Fixes another of the bullet points from #8771

## Test Plan

`cargo test` / `cargo insta review`


---

_Label `rule` added by @AlexWaygood on 2024-01-17 17:14_

---

_Comment by @codspeed-hq[bot] on 2024-01-17 17:21_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:y049-functional)

### Merging #9567 will **not alter performance**

<sub>Comparing <code>AlexWaygood:y049-functional</code> (9a9af40) with <code>main</code> (7be7066)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-01-17 17:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/unused_private_type_definition.rs`:367 on 2024-01-18 03:16_

I often find it helpful to add comments to illustrate the pattern in Python code, for each branch.


---

_@charliermarsh reviewed on 2024-01-18 03:16_

---

_@charliermarsh approved on 2024-01-18 03:16_

---

_Merged by @AlexWaygood on 2024-01-18 10:01_

---

_Closed by @AlexWaygood on 2024-01-18 10:01_

---

_Branch deleted on 2024-01-18 10:02_

---
