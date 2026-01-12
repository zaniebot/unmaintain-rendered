```yaml
number: 7249
title: "[`flake8-logging`] Add `flake8_logging` boilerplate and first rule `LOG009` "
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
assignees: []
merged: true
base: main
head: rule/LOG009
created_at: 2023-09-08T22:50:54Z
updated_at: 2023-09-15T15:32:16Z
url: https://github.com/astral-sh/ruff/pull/7249
synced_at: 2026-01-12T02:39:09Z
```

# [`flake8-logging`] Add `flake8_logging` boilerplate and first rule `LOG009` 

---

_Pull request opened by @qdegraaf on 2023-09-08 22:50_

## Summary

Adds `LOG009` from [flake8-logging](https://github.com/adamchainz/flake8-logging). Also adds the boilerplate for a new plugin

Checks for usages of undocumented `logging.WARN` constant and suggests replacement with `logging.WARNING`.

## Test Plan

`cargo test` with fresh fixture

## Issue links

Refers: https://github.com/astral-sh/ruff/issues/7248

---

_Comment by @github-actions[bot] on 2023-09-08 23:15_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_logging/rules/undocumented_warn.rs`:12 on 2023-09-11 07:21_

```suggestion
/// Checks for uses of `logging.WARN`.
```

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_logging/rules/undocumented_warn.rs`:34 on 2023-09-11 07:23_

```suggestion
        format!("Use of undocumented `logging.WARN` constant")
```

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_logging/rules/undocumented_warn.rs`:38 on 2023-09-11 07:24_

```suggestion
        Some(format!("Replace `logging.WARN` with `logging.WARNING`"))
```

---

_@hugovk reviewed on 2023-09-11 07:24_

---

_Review comment by @hugovk on `LICENSE`:1227 on 2023-09-11 07:24_

To match the others:
```suggestion
- flake8-logging, licensed as follows:
```

---

_@konstin approved on 2023-09-11 07:26_

---

_Review requested from @charliermarsh by @konstin on 2023-09-11 07:26_

---

_Label `rule` added by @konstin on 2023-09-11 07:26_

---

_Merged by @charliermarsh on 2023-09-15 01:41_

---

_Closed by @charliermarsh on 2023-09-15 01:41_

---

_Branch deleted on 2023-09-15 15:32_

---
