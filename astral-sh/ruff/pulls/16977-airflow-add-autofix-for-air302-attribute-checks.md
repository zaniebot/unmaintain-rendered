```yaml
number: 16977
title: "[`airflow`] Add autofix for `AIR302` attribute checks"
type: pull_request
state: merged
author: Lee-W
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: autofix-AIR302-check-class-attribute
created_at: 2025-03-26T08:17:35Z
updated_at: 2025-04-02T15:21:47Z
url: https://github.com/astral-sh/ruff/pull/16977
synced_at: 2026-01-12T15:55:59Z
```

# [`airflow`] Add autofix for `AIR302` attribute checks

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
Add autofix logic to AIR302 check_method

## Test Plan

<!-- How was it tested? -->
test fixtures have been updated accordingly

---

_Comment by @github-actions[bot] on 2025-03-26 08:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @Lee-W on 2025-03-27 19:35_

---

_Label `fixes` added by @dhruvmanila on 2025-03-31 21:04_

---

_Label `preview` added by @dhruvmanila on 2025-03-31 21:04_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:295 on 2025-03-31 21:05_

Same as https://github.com/astral-sh/ruff/pull/16976#discussion_r2021757788

---

_@dhruvmanila approved on 2025-03-31 21:05_

Looks good, once the clone is removed I'll merge this and the other PR (#16976).

---

_Renamed from "[airflow] add autofix to AIR302 check_class_attribute" to "[`airflow`] Add autofix `AIR302` attribute checks" by @dhruvmanila on 2025-03-31 21:06_

---

_@Lee-W reviewed on 2025-04-01 07:29_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:295 on 2025-04-01 07:29_

Just updated. Thanks!

---

_Comment by @dhruvmanila on 2025-04-02 15:06_

@Lee-W You'd need to resolve the merge conflicts, I can't push to this branch directly as it's on the fork is under the Astronomer organization.

---

_Comment by @Lee-W on 2025-04-02 15:08_

> @Lee-W You'd need to resolve the merge conflicts, I can't push to this branch directly as it's on the fork is under the Astronomer organization.

Yep, I will. Sorry, I don't have the permission to change it either 😢 But I'll rebase it ASAP!

---

_Comment by @dhruvmanila on 2025-04-02 15:16_

Thank you!

---

_Renamed from "[`airflow`] Add autofix `AIR302` attribute checks" to "[`airflow`] Add autofix for `AIR302` attribute checks" by @dhruvmanila on 2025-04-02 15:16_

---

_Merged by @dhruvmanila on 2025-04-02 15:18_

---

_Closed by @dhruvmanila on 2025-04-02 15:18_

---
