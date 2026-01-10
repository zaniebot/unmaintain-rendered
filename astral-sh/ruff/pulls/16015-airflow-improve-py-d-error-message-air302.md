```yaml
number: 16015
title: "[airflow] improve PY\\d+ error message (AIR302)"
type: pull_request
state: closed
author: Lee-W
labels: []
assignees: []
base: main
head: AIR302-enhancement
created_at: 2025-02-07T10:01:41Z
updated_at: 2025-02-08T04:08:10Z
url: https://github.com/astral-sh/ruff/pull/16015
synced_at: 2026-01-10T19:57:22Z
```

# [airflow] improve PY\d+ error message (AIR302)

---

_Pull request opened by @Lee-W on 2025-02-07 10:01_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

add more explicit instruction

## Test Plan

<!-- How was it tested? -->

update test fixture

---

_Comment by @github-actions[bot] on 2025-02-07 10:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2025-02-07 11:06_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:579 on 2025-02-07 11:06_

Are we sure that's the right recommendation? Can the deprecated Airflow symbols related to this be used with other comparison operators like `> PY36`, `<= PY310`, etc.?

---

_@Lee-W reviewed on 2025-02-08 04:08_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:579 on 2025-02-08 04:08_

ah, you're right. I guess that's why I did not do it this way at the first place. Let me close this one! Thanks for reminding me!

---

_Closed by @Lee-W on 2025-02-08 04:08_

---
