```yaml
number: 8439
title: Add flake8-trio support
type: pull_request
state: merged
author: karpetrosyan
labels:
  - rule
assignees: []
merged: true
base: main
head: add-flake8-trio
created_at: 2023-11-02T05:48:23Z
updated_at: 2023-11-03T01:12:22Z
url: https://github.com/astral-sh/ruff/pull/8439
synced_at: 2026-01-10T23:40:55Z
```

# Add flake8-trio support

---

_Pull request opened by @karpetrosyan on 2023-11-02 05:48_

## Summary

This pull request adds [flake8-trio](https://github.com/Zac-HD/flake8-trio) support to ruff, which is a very useful plugin for trio users to avoid very common mistakes.

Part of https://github.com/astral-sh/ruff/issues/8451.

## Test Plan

Traditional rule testing, as [described in the documentation](https://docs.astral.sh/ruff/contributing/#rule-testing-fixtures-and-snapshots).

---

_Comment by @github-actions[bot] on 2023-11-02 06:27_

## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @charliermarsh on 2023-11-02 13:58_

---

_Label `rule` added by @charliermarsh on 2023-11-02 13:58_

---

_Comment by @github-actions[bot] on 2023-11-02 14:07_

## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_trio/rules/timeout_without_await.rs`:32 on 2023-11-02 16:22_

I think we should prefix all these rules with `Trio`, like `TrioTimeoutWithoutAwait`, similar to our Pytest rules.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_trio/rules/timeout_without_await.rs`:47 on 2023-11-02 16:24_

I think we should add a `visit_stmt` here that avoids recursing into functions and class bodies.

---

_@charliermarsh reviewed on 2023-11-02 16:24_

Looks great! Just some small comments.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-02 16:24_

---

_Comment by @charliermarsh on 2023-11-03 00:57_

Thanks! I also created https://github.com/astral-sh/ruff/issues/8451 to track flake8-trio.

---

_Merged by @charliermarsh on 2023-11-03 01:05_

---

_Closed by @charliermarsh on 2023-11-03 01:05_

---

_Comment by @github-actions[bot] on 2023-11-03 01:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
