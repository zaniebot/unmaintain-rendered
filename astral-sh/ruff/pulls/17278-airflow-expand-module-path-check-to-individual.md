```yaml
number: 17278
title: "[`airflow`] Expand module path check to individual symbols (`AIR302`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: get-rid-of-ImportPathMoved
created_at: 2025-04-07T15:01:32Z
updated_at: 2025-04-08T13:03:28Z
url: https://github.com/astral-sh/ruff/pull/17278
synced_at: 2026-01-10T19:40:37Z
```

# [`airflow`] Expand module path check to individual symbols (`AIR302`)

---

_Pull request opened by @Lee-W on 2025-04-07 15:01_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->


### Improvement
Expand the following moved module into individual symbols.

* airflow.triggers.temporal
* airflow.triggers.file
* airflow.triggers.external_task
* airflow.hooks.subprocess
* airflow.hooks.package_index
* airflow.hooks.filesystem
* airflow.sensors.weekday
* airflow.sensors.time_delta
* airflow.sensors.time_sensor
* airflow.sensors.date_time
* airflow.operators.weekday
* airflow.operators.datetime
* airflow.operators.bash 

This removes `Replacement::ImportPathMoved`.

## Fix
During the expansion, the following paths were also fixed

* airflow.sensors.s3_key_sensor.S3KeySensor → airflow.providers.amazon.aws.sensors.S3KeySensor
* airflow.operators.sql.SQLThresholdCheckOperator → airflow.providers.common.sql.operators.sql.SQLThresholdCheckOperator
* airflow.hooks.druid_hook.DruidDbApiHook → airflow.providers.apache.druid.hooks.druid.DruidDbApiHook
* airflow.hooks.druid_hook.DruidHook → airflow.providers.apache.druid.hooks.druid.DruidHook
* airflow.kubernetes.pod_generator.extend_object_field → airflow.providers.cncf.kubernetes.pod_generator.extend_object_field
* airflow.kubernetes.pod_launcher.PodLauncher → airflow.providers.cncf.kubernetes.pod_launcher_deprecated.PodLauncher
* airflow.kubernetes.pod_launcher.PodStatus → airflow.providers.cncf.kubernetes.pod_launcher_deprecated.PodStatus
* airflow.kubernetes.pod_generator.PodDefaults → airflow.providers.cncf.kubernetes.pod_generator.PodDefaults
* airflow.kubernetes.pod_launcher_deprecated.PodDefaults → airflow.providers.cncf.kubernetes.pod_launcher_deprecated.PodDefaults

### Refactor
As many symbols are moved into the same module, `SourceModuleMovedToProvider` is introduced for grouping similar logic

## Test Plan

<!-- How was it tested? -->


---

_Review comment by @Skylion007 on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302.py`:201 on 2025-04-07 15:04_

Merge conflict! Do we not have any merge conflict checks in our pre-commit hook? There should be a builtin one.

---

_@Skylion007 reviewed on 2025-04-07 15:04_

---

_Comment by @github-actions[bot] on 2025-04-07 15:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/0b375e08a023263e95e80f720afa49d40da2e88f/performance/src/performance_dags/performance_dag/performance_dag.py#L95'>performance/src/performance_dags/performance_dag/performance_dag.py:95:13:</a> AIR302 Import path `airflow.operators.bash` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/0b375e08a023263e95e80f720afa49d40da2e88f/performance/src/performance_dags/performance_dag/performance_dag.py#L95'>performance/src/performance_dags/performance_dag/performance_dag.py:95:13:</a> AIR302 `airflow.operators.bash.BashOperator` is moved into `standard` provider in Airflow 3.0;
- <a href='https://github.com/apache/airflow/blob/0b375e08a023263e95e80f720afa49d40da2e88f/providers/edge/src/airflow/providers/edge/example_dags/integration_test.py#L116'>providers/edge/src/airflow/providers/edge/example_dags/integration_test.py:116:24:</a> AIR302 Import path `airflow.operators.bash` is moved into `standard` provider in Airflow 3.0;
+ <a href='https://github.com/apache/airflow/blob/0b375e08a023263e95e80f720afa49d40da2e88f/providers/edge/src/airflow/providers/edge/example_dags/integration_test.py#L116'>providers/edge/src/airflow/providers/edge/example_dags/integration_test.py:116:24:</a> AIR302 `airflow.operators.bash.BashOperator` is moved into `standard` provider in Airflow 3.0;
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR302 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>




---

_@Lee-W reviewed on 2025-04-07 15:08_

---

_Review comment by @Lee-W on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302.py`:201 on 2025-04-07 15:08_

I just checked. it doesn't seem to be used 🤔 

---

Hmmm... I checked during the 20 commits' rebasing, and it's still missing. 😭 Let me check it again

---

_Converted to draft by @Lee-W on 2025-04-07 15:08_

---

_@Lee-W reviewed on 2025-04-07 15:11_

---

_Review comment by @Lee-W on `crates/ruff_linter/resources/test/fixtures/airflow/AIR302.py`:201 on 2025-04-07 15:11_

ah... it's the last one... just got it resolved. Thanks for reminder!

---

_Renamed from "Get rid of import path moved" to "[airflow] Expand module path check to individual symbols (AIR302)" by @Lee-W on 2025-04-07 15:12_

---

_Marked ready for review by @Lee-W on 2025-04-07 15:12_

---

_Comment by @Lee-W on 2025-04-07 15:22_

@ntBre Hey, it would be nice if we could get this one reviewed. 🙏 The amount of change is enormous, but I tried to split the PR into 20 separate commits, so I hope it will be easier to review that way  

---

_Label `rule` added by @ntBre on 2025-04-07 16:20_

---

_Label `preview` added by @ntBre on 2025-04-07 16:20_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:130 on 2025-04-07 21:03_

We *could* merge these nested `match`es with something like this, but you have to parenthesized the last part of the pattern to bind to `rest`. The main advantage would be getting rid of extra `_ => return` cases, but I don't think it looks *that* much better (and it would be a lot more changes), so I'm also happy with the current version.

```suggestion
        ["airflow", "hooks", "S3_hook", rest @ ("S3Hook" | "provide_bucket_name")] => 
            Replacement::SourceModuleMovedToProvider {
                name: (*rest).to_string(),
                module: "airflow.providers.amazon.aws.hooks.s3",
                provider: "amazon",
                version: "1.0.0"
            },
```

---

_@ntBre approved on 2025-04-07 21:04_

Thanks! This was easy to review commit-by-commit. I had one suggestion, but I'm happy enough with the current version if you are.

---

_@Lee-W reviewed on 2025-04-08 03:21_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:130 on 2025-04-08 03:21_

This is super nice! Just refactor it a bit. To unify the `|` usage, I add break lines to them, which might make it easier to extent in the future

---

_@ntBre approved on 2025-04-08 13:01_

---

_Renamed from "[airflow] Expand module path check to individual symbols (AIR302)" to "[`airflow`] Expand module path check to individual symbols (`AIR302`)" by @ntBre on 2025-04-08 13:02_

---

_Merged by @ntBre on 2025-04-08 13:03_

---

_Closed by @ntBre on 2025-04-08 13:03_

---
