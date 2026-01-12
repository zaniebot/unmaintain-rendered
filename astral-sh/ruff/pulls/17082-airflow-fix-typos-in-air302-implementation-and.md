```yaml
number: 17082
title: "[airflow] fix typos in AIR302 implementation and test cases"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-AIR302-wrong-paths
created_at: 2025-03-31T08:35:05Z
updated_at: 2025-03-31T08:42:05Z
url: https://github.com/astral-sh/ruff/pull/17082
synced_at: 2026-01-12T15:56:00Z
```

# [airflow] fix typos in AIR302 implementation and test cases

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

* The following paths are wrong 
    * `airflow.providers.amazon.auth_manager.avp.entities` should be `airflow.providers.amazon.aws.auth_manager.avp.entities`
    * `["airflow", "datasets", "manager", "dataset_manager"]` should be fixed as `airflow.assets.manager` but not `airflow.assets.manager.asset_manager`
   * `["airflow", "datasets.manager", "DatasetManager"]` should be ` ["airflow", "datasets", "manager", "DatasetManager"]` instead

## Test Plan

<!-- How was it tested? -->

the test fixture is updated accordingly

---

_Comment by @github-actions[bot] on 2025-03-31 08:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @MichaReiser on 2025-03-31 08:41_

---

_Label `preview` added by @MichaReiser on 2025-03-31 08:41_

---

_@MichaReiser approved on 2025-03-31 08:42_

---

_Merged by @MichaReiser on 2025-03-31 08:42_

---

_Closed by @MichaReiser on 2025-03-31 08:42_

---
