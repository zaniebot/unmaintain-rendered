```yaml
number: 14263
title: "[`flake8-simplify`] Infer \"unknown\" truthiness for literal iterables whose items are all unpacks (`SIM222`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
assignees: []
merged: true
base: main
head: SIM222
created_at: 2024-11-11T05:15:40Z
updated_at: 2024-11-11T20:38:16Z
url: https://github.com/astral-sh/ruff/pull/14263
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-simplify`] Infer "unknown" truthiness for literal iterables whose items are all unpacks (`SIM222`)

---

_Pull request opened by @InSyncWithFoo on 2024-11-11 05:15_

## Summary

Resolves #14237.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-11-11 05:34_

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

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/helpers.rs`:1196 on 2024-11-11 07:59_

```suggestion
                if elts.iter().all(Expr::is_starred_expr) {
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/helpers.rs`:1218 on 2024-11-11 08:01_

nit: I'd just inline the closure logic here

---

_@dhruvmanila approved on 2024-11-11 08:01_

---

_Label `bug` added by @dhruvmanila on 2024-11-11 08:02_

---

_Merged by @charliermarsh on 2024-11-11 20:23_

---

_Closed by @charliermarsh on 2024-11-11 20:23_

---

_Branch deleted on 2024-11-11 20:38_

---
