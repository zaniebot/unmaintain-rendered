```yaml
number: 9529
title: "add the \"__prepare__\" method to the list of recognized dunder method"
type: pull_request
state: merged
author: takaya0
labels:
  - bug
assignees: []
merged: true
base: main
head: PLW3201-add-prepare-to-known-dunder-method
created_at: 2024-01-15T14:31:26Z
updated_at: 2024-01-15T14:45:04Z
url: https://github.com/astral-sh/ruff/pull/9529
synced_at: 2026-01-12T15:55:29Z
```

# add the "__prepare__" method to the list of recognized dunder method

---

_@takaya0_

## Summary
Closes #9508 .
Add `__prepare__` method to dunder method list in `is_known_dunder_method`.

## Test Plan
1. add "__prepare__" method to `Apple` class in crates/ruff_linter/resources/test/fixtures/pylint/bad_dunder_method_name.py.
2. run `cargo test`


---

_Label `bug` added by @charliermarsh on 2024-01-15 14:32_

---

_@charliermarsh approved on 2024-01-15 14:32_

Thanks!

---

_Merged by @charliermarsh on 2024-01-15 14:37_

---

_Closed by @charliermarsh on 2024-01-15 14:37_

---

_Comment by @codspeed-hq[bot] on 2024-01-15 14:38_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/takaya0:PLW3201-add-prepare-to-known-dunder-method)

### Merging #9529 will **improve performances by 4.55%**

<sub>Comparing <code>takaya0:PLW3201-add-prepare-to-known-dunder-method</code> (98a9db3) with <code>main</code> (66804ec)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `takaya0:PLW3201-add-prepare-to-known-dunder-method` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/ctypeslib.py]` | 12.8 ms | 12.2 ms | +4.55% |


---

_Comment by @github-actions[bot] on 2024-01-15 14:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
