```yaml
number: 17940
title: "[`airflow`] Move rules from `AIR312` to `AIR302`"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: move-wrongly-categorized-AIR312-rules
created_at: 2025-05-08T10:11:46Z
updated_at: 2025-05-19T17:20:22Z
url: https://github.com/astral-sh/ruff/pull/17940
synced_at: 2026-01-12T15:56:08Z
```

# [`airflow`] Move rules from `AIR312` to `AIR302`

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

In the later development of Airflow 3.0, backward compatibility was not added for some cases. Thus, the following rules are moved back to AIR302

* airflow.hooks.subprocess.SubprocessResult → airflow.providers.standard.hooks.subprocess.SubprocessResult
* airflow.hooks.subprocess.working_directory → airflow.providers.standard.hooks.subprocess.working_directory
* airflow.operators.datetime.target_times_as_dates → airflow.providers.standard.operators.datetime.target_times_as_dates
* airflow.operators.trigger_dagrun.TriggerDagRunLink → airflow.providers.standard.operators.trigger_dagrun.TriggerDagRunLink
* airflow.sensors.external_task.ExternalTaskSensorLink → airflow.providers.standard.sensors.external_task.ExternalDagLink (**This one contains a minor change**)
* airflow.sensors.time_delta.WaitSensor → airflow.providers.standard.sensors.time_delta.WaitSensor

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-05-08 10:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "refactor(AIR302,-AIR312): get rid of ProviderReplacement::ProviderNam…" to "[`airflow`] Move rules from `AIR302` to `AIR312`" by @Lee-W on 2025-05-08 10:33_

---

_Renamed from "[`airflow`] Move rules from `AIR302` to `AIR312`" to "[`airflow`] Move rules from `AIR312` to `AIR302`" by @Lee-W on 2025-05-08 10:33_

---

_Comment by @Lee-W on 2025-05-08 11:53_

It depends on https://github.com/astral-sh/ruff/pull/17942

---

_Review requested from @ntBre by @ntBre on 2025-05-13 01:39_

---

_Marked ready for review by @Lee-W on 2025-05-16 10:32_

---

_Label `rule` added by @ntBre on 2025-05-19 17:16_

---

_Label `preview` added by @ntBre on 2025-05-19 17:16_

---

_@ntBre approved on 2025-05-19 17:20_

LGTM

---

_Merged by @ntBre on 2025-05-19 17:20_

---

_Closed by @ntBre on 2025-05-19 17:20_

---
