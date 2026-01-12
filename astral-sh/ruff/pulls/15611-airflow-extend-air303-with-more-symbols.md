```yaml
number: 15611
title: "[`airflow`] Extend `AIR303` with more symbols"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-AIR303
created_at: 2025-01-20T09:23:47Z
updated_at: 2025-01-23T08:50:02Z
url: https://github.com/astral-sh/ruff/pull/15611
synced_at: 2026-01-12T15:55:51Z
```

# [`airflow`] Extend `AIR303` with more symbols

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

Extend AIR303 with the following rules

* `airflow.operators.datetime.*` → `airflow.providers.standard.time.operators.datetime.*`
* `airflow.operators.weekday.*` → `airflow.providers.standard.time.operators.weekday.*`
* `airflow.sensors.date_time.*` → `airflow.providers.standard.time.sensors.date_time.*`
* `airflow.sensors.time_sensor.*` → `airflow.providers.standard.time.sensors.time.*`
* `airflow.sensors.time_delta.*` → `airflow.providers.standard.time.sensors.time_delta.*`
* `airflow.sensors.weekday.*` → `airflow.providers.standard.time.sensors.weekday.*`
* `airflow.hooks.filesystem.*` → `airflow.providers.standard.hooks.filesystem.*`
* `airflow.hooks.package_index.*` → `airflow.providers.standard.hooks.package_index.*`
* `airflow.hooks.subprocess.*` → `airflow.providers.standard.hooks.subprocess.*`
* `airflow.triggers.external_task.*` → `airflow.providers.standard.triggers.external_task.*`
* `airflow.triggers.file.*` → `airflow.providers.standard.triggers.file.*`
* `airflow.triggers.temporal.*` → `airflow.providers.standard.triggers.temporal.*`
* `airflow.sensors.filesystem.FileSensor` → `airflow.providers.standard.sensors.filesystem.FileSensor`
* `airflow.operators.trigger_dagrun.TriggerDagRunOperator` → `airflow.providers.standard.operators.trigger_dagrun.TriggerDagRunOperator`
* `airflow.sensors.external_task.ExternalTaskMarker` → `airflow.providers.standard.sensors.external_task.ExternalTaskMarker`
* `airflow.sensors.external_task.ExternalTaskSensor` → `airflow.providers.standard.sensors.external_task.ExternalTaskSensor`

## Test Plan

<!-- How was it tested? -->
a test fixture has been updated

---

_Renamed from "feat(AIR303): extend the following rules" to "[airflow] extend AIR303 rules" by @Lee-W on 2025-01-20 09:26_

---

_Label `rule` added by @dhruvmanila on 2025-01-20 09:26_

---

_Label `preview` added by @dhruvmanila on 2025-01-20 09:26_

---

_@dhruvmanila approved on 2025-01-20 09:29_

---

_Renamed from "[airflow] extend AIR303 rules" to "[`airflow`] Extend `AIR303` with more symbols" by @dhruvmanila on 2025-01-20 09:29_

---

_Comment by @github-actions[bot] on 2025-01-20 09:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dhruvmanila on 2025-01-20 09:30_

---

_Closed by @dhruvmanila on 2025-01-20 09:30_

---

_Branch deleted on 2025-01-23 08:50_

---
