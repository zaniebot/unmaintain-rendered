```yaml
number: 16968
title: "[airflow] fix missing or wrong test cases (AIR302)"
type: pull_request
state: merged
author: Lee-W
labels:
  - internal
assignees: []
merged: true
base: main
head: add-missing-test-case
created_at: 2025-03-25T13:40:26Z
updated_at: 2025-03-31T08:23:47Z
url: https://github.com/astral-sh/ruff/pull/16968
synced_at: 2026-01-10T19:40:36Z
```

# [airflow] fix missing or wrong test cases (AIR302)

---

_Pull request opened by @Lee-W on 2025-03-25 13:40_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Improve AIR302 test cases

## Test Plan

<!-- How was it tested? -->
test fixtures have been updated accordingly


---

_Renamed from "[airflow ]test(AIR302): fix missing or wrong test cases" to "[airflow] fix missing or wrong test cases (AIR302)" by @Lee-W on 2025-03-25 13:40_

---

_Comment by @github-actions[bot] on 2025-03-25 13:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @sunank200 on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_names.py`:82 on 2025-03-28 06:10_

Should we also add a import for the following in the test and it shouldn't raise any error:
```from airflow.providers.standard.sensors.external_task import ExternalTaskMarker```

---

_@sunank200 reviewed on 2025-03-28 06:11_

---

_@Lee-W reviewed on 2025-03-28 06:38_

---

_Review comment by @Lee-W on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302_names.py`:82 on 2025-03-28 06:38_

this one will be moved to AIR303 instead. but I want to keep the existing rules as it is and then fix the test cases later 

---

_@sunank200 reviewed on 2025-03-28 06:55_

I don't have option to approve it but this change LGTM.

---

_Label `internal` added by @MichaReiser on 2025-03-31 08:22_

---

_Merged by @MichaReiser on 2025-03-31 08:22_

---

_Closed by @MichaReiser on 2025-03-31 08:22_

---
