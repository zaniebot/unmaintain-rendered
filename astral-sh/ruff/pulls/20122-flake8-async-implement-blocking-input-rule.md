```yaml
number: 20122
title: "[`flake8-async`] Implement `blocking-input` rule (`ASYNC250`)"
type: pull_request
state: merged
author: amyreese
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: async-rules
created_at: 2025-08-28T00:20:21Z
updated_at: 2025-08-28T18:04:25Z
url: https://github.com/astral-sh/ruff/pull/20122
synced_at: 2026-01-12T15:56:54Z
```

# [`flake8-async`] Implement `blocking-input` rule (`ASYNC250`)

---

_@amyreese_

## Summary

Adds new rule to catch use of builtins `input()` in async functions.

Issue #8451

## Test Plan

New snapshosts in `ASYNC250.py` with `cargo insta test`.


---

_Review requested from @ntBre by @amyreese on 2025-08-28 00:23_

---

_Comment by @github-actions[bot] on 2025-08-28 00:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @ntBre on 2025-08-28 13:57_

---

_Label `preview` added by @ntBre on 2025-08-28 13:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_input.rs`:16 on 2025-08-28 14:00_

This rule isn't specific to HTTP clients, right? This might be left over from the last one.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_async/rules/blocking_input.rs`:57 on 2025-08-28 14:03_

I think we could use `SemanticModel::match_builtin_expr` here.

https://github.com/astral-sh/ruff/blob/6d507b4e010629b6817b22ead9a7d4368b223567/crates/ruff_python_semantic/src/model.rs#L319

---

_Review comment by @ntBre on `crates/ruff_linter/src/codes.rs`:344 on 2025-08-28 14:06_

All new rules have to go into `RuleGroup::Preview` initially.

---

_@ntBre reviewed on 2025-08-28 14:07_

Looks great, thanks! Just a couple of minor things

---

_Comment by @amyreese on 2025-08-28 17:02_

@ntBre fixed the oversights and updated to use `match_builtin_expr`

---

_@ntBre approved on 2025-08-28 17:05_

Thank you!

---

_Merged by @amyreese on 2025-08-28 18:04_

---

_Closed by @amyreese on 2025-08-28 18:04_

---
