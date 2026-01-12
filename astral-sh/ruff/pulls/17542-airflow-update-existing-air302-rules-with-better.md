```yaml
number: 17542
title: "[`airflow`] update existing `AIR302` rules with better suggestions"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: update-AIR302-with-better-suggestion
created_at: 2025-04-22T09:50:41Z
updated_at: 2025-04-25T16:44:29Z
url: https://github.com/astral-sh/ruff/pull/17542
synced_at: 2026-01-12T15:56:02Z
```

# [`airflow`] update existing `AIR302` rules with better suggestions

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

Even though the original suggestion works, they've been removed in later version and is no longer the best practices.

e.g., many sql realted operators have been removed and are now suggested to use SQLExecuteQueryOperator instead

## Test Plan

<!-- How was it tested? -->

The existing test fixtures have been updated


---

_Comment by @github-actions[bot] on 2025-04-22 09:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "feat(AIR302): update existing rules with better suggestions" to "[`airflow`] update existing `AIR302` rules with better suggestions" by @Lee-W on 2025-04-22 14:34_

---

_Label `rule` added by @ntBre on 2025-04-24 18:00_

---

_Label `preview` added by @ntBre on 2025-04-24 18:00_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:65 on 2025-04-24 18:02_

```suggestion
            ProviderReplacement::None => None,
```

I'm surprised clippy or rustfmt didn't catch this.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_move_to_provider_in_3.rs`:68 on 2025-04-24 18:05_

```suggestion
            ProviderReplacement::None => None,
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302.py.snap`:1240 on 2025-04-24 18:07_

Just double checking that this is the right version since it's lower than the old version.

---

_@ntBre approved on 2025-04-24 18:09_

Thanks! A couple of formatting suggestions and one question about a version, but this looks good to me.

---

_@Lee-W reviewed on 2025-04-24 23:53_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302.py.snap`:1240 on 2025-04-24 23:53_

Yep, this is expected even though much happens in 7.4.0, some were moved earlier

---

_@Lee-W reviewed on 2025-04-25 00:29_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:65 on 2025-04-25 00:29_

updated. thanks!

---

_@Lee-W reviewed on 2025-04-25 00:29_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_move_to_provider_in_3.rs`:68 on 2025-04-25 00:29_

updated. thanks!

---

_Merged by @ntBre on 2025-04-25 16:44_

---

_Closed by @ntBre on 2025-04-25 16:44_

---
