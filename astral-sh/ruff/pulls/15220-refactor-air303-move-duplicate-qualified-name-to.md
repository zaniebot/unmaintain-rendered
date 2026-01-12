```yaml
number: 15220
title: "refactor(AIR303): move duplicate qualified_name.to_string() to Diagnostic argument"
type: pull_request
state: merged
author: Lee-W
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor-AIR303
created_at: 2025-01-02T12:42:53Z
updated_at: 2025-01-23T08:50:11Z
url: https://github.com/astral-sh/ruff/pull/15220
synced_at: 2026-01-12T15:55:50Z
```

# refactor(AIR303): move duplicate qualified_name.to_string() to Diagnostic argument

---

_@Lee-W_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Refactor airflow rule logic like https://github.com/astral-sh/ruff/commit/86bdc2e7b1b3b6b8630a898fdcfc1071d5f7cb7d

## Test Plan

<!-- How was it tested? -->

No functionality change. Existing test cases work as it was


---

_Comment by @github-actions[bot] on 2025-01-02 12:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2025-01-03 03:08_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:114 on 2025-01-03 03:08_

I think we should just pass in the `TextRange` here to avoid the monomorphization of this large function.

---

_@dhruvmanila approved on 2025-01-03 03:08_

Thank you for doing this.

---

_Label `internal` added by @dhruvmanila on 2025-01-03 03:12_

---

_@Lee-W reviewed on 2025-01-03 03:29_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:114 on 2025-01-03 03:29_

Just updated it!

---

_Merged by @dhruvmanila on 2025-01-03 03:40_

---

_Closed by @dhruvmanila on 2025-01-03 03:40_

---

_Branch deleted on 2025-01-23 08:50_

---
