```yaml
number: 17293
title: "[`airflow`] Refactor `AIR301` logic and fix typos (`AIR301`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor-AIR301
created_at: 2025-04-08T11:18:50Z
updated_at: 2025-04-09T14:46:17Z
url: https://github.com/astral-sh/ruff/pull/17293
synced_at: 2026-01-12T15:56:01Z
```

# [`airflow`] Refactor `AIR301` logic and fix typos (`AIR301`)

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

* Simplify match conditions in AIR301
* Fix
    * `airflow.datasets.manager.DatasetManager` → `airflow.assets.manager.AssetManager`
    * `airflow.www.auth.has_access_dataset` → `airflow.www.auth.has_access_dataset` 

## Test Plan

<!-- How was it tested? -->
The test fixture has been updated accordingly

---

_Comment by @github-actions[bot] on 2025-04-08 11:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -19 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -19 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/amazon/src/airflow/providers/amazon/aws/links/base_aws.py#L41'>providers/amazon/src/airflow/providers/amazon/aws/links/base_aws.py:41:19:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/databricks/src/airflow/providers/databricks/operators/databricks.py#L251'>providers/databricks/src/airflow/providers/databricks/operators/databricks.py:251:28:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/databricks/src/airflow/providers/databricks/plugins/databricks_workflow.py#L238'>providers/databricks/src/airflow/providers/databricks/plugins/databricks_workflow.py:238:26:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/databricks/src/airflow/providers/databricks/plugins/databricks_workflow.py#L272'>providers/databricks/src/airflow/providers/databricks/plugins/databricks_workflow.py:272:38:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/databricks/src/airflow/providers/databricks/plugins/databricks_workflow.py#L366'>providers/databricks/src/airflow/providers/databricks/plugins/databricks_workflow.py:366:39:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/dbt/cloud/src/airflow/providers/dbt/cloud/operators/dbt.py#L50'>providers/dbt/cloud/src/airflow/providers/dbt/cloud/operators/dbt.py:50:34:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/google/src/airflow/providers/google/cloud/links/base.py#L38'>providers/google/src/airflow/providers/google/cloud/links/base.py:38:22:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/google/src/airflow/providers/google/cloud/links/datafusion.py#L44'>providers/google/src/airflow/providers/google/cloud/links/datafusion.py:44:22:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/google/src/airflow/providers/google/cloud/links/dataproc.py#L136'>providers/google/src/airflow/providers/google/cloud/links/dataproc.py:136:24:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/google/src/airflow/providers/google/cloud/links/dataproc.py#L83'>providers/google/src/airflow/providers/google/cloud/links/dataproc.py:83:20:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/google/src/airflow/providers/google/cloud/operators/dataproc_metastore.py#L112'>providers/google/src/airflow/providers/google/cloud/operators/dataproc_metastore.py:112:37:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/google/src/airflow/providers/google/cloud/operators/dataproc_metastore.py#L64'>providers/google/src/airflow/providers/google/cloud/operators/dataproc_metastore.py:64:29:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/google/src/airflow/providers/google/marketing_platform/links/analytics_admin.py#L39'>providers/google/src/airflow/providers/google/marketing_platform/links/analytics_admin.py:39:31:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/microsoft/azure/src/airflow/providers/microsoft/azure/operators/data_factory.py#L52'>providers/microsoft/azure/src/airflow/providers/microsoft/azure/operators/data_factory.py:52:53:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/microsoft/azure/src/airflow/providers/microsoft/azure/operators/powerbi.py#L42'>providers/microsoft/azure/src/airflow/providers/microsoft/azure/operators/powerbi.py:42:19:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/microsoft/azure/src/airflow/providers/microsoft/azure/operators/synapse.py#L133'>providers/microsoft/azure/src/airflow/providers/microsoft/azure/operators/synapse.py:133:35:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/standard/src/airflow/providers/standard/operators/trigger_dagrun.py#L71'>providers/standard/src/airflow/providers/standard/operators/trigger_dagrun.py:71:25:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/standard/src/airflow/providers/standard/sensors/external_task.py#L63'>providers/standard/src/airflow/providers/standard/sensors/external_task.py:63:23:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/c874e8dbe61e376d2415c98ae9374db8a9621e5e/providers/yandex/src/airflow/providers/yandex/links/yq.py#L43'>providers/yandex/src/airflow/providers/yandex/links/yq.py:43:14:</a> AIR301 `airflow.models.baseoperatorlink.BaseOperatorLink` is removed in Airflow 3.0
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR301 | 19 | 0 | 19 | 0 | 0 |

</p>
</details>




---

_Renamed from "Refactor air301" to "[Airflow] Refactor AIR301 logic and fix typos (AIR301)" by @Lee-W on 2025-04-08 13:18_

---

_Marked ready for review by @Lee-W on 2025-04-08 13:20_

---

_Comment by @Lee-W on 2025-04-08 13:32_

The TODOs are intentionally kept. They're things I think might be outdated but I'll need to confirm with the airflow community. Also, I would like to keep this PR simple

---

_Assigned to @ntBre by @ntBre on 2025-04-08 18:55_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:78 on 2025-04-08 18:57_

These can't be combined because `name` has a different type in the two arms? I think that's right but just double-checking.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:822 on 2025-04-08 19:03_

Is the comment just above this outdated or are they both covered here now?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:850 on 2025-04-08 19:03_

Same here

---

_@ntBre approved on 2025-04-08 19:04_

Thanks! A couple of questions, but this looks good otherwise.

---

_@Lee-W reviewed on 2025-04-09 01:49_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:78 on 2025-04-09 01:49_

Yep, just tired it a bit

---

_@Lee-W reviewed on 2025-04-09 01:51_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:822 on 2025-04-09 01:51_

`airflow.providers.google` is a package installable from PyPI. Even though we only need to check airflow.providers.google.`datasets`, I think it might be better to keep comments this way to align airflow logic 

```rust
// airflow.providers.google
// airflow.providers.google.datasets
some logic
// airflow.providers.google.some_other_module
some other logic

```

---

_@Lee-W reviewed on 2025-04-09 01:51_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:850 on 2025-04-09 01:51_

Same as https://github.com/astral-sh/ruff/pull/17293#discussion_r2034312971

---

_Comment by @ntBre on 2025-04-09 14:36_

Is this ready to merge?

---

_Comment by @Lee-W on 2025-04-09 14:43_

> Is this ready to merge?

Yep, this is ready 🙂 

---

_@ntBre approved on 2025-04-09 14:45_

---

_Renamed from "[Airflow] Refactor AIR301 logic and fix typos (AIR301)" to "[`airflow`] Refactor `AIR301` logic and fix typos (`AIR301`)" by @ntBre on 2025-04-09 14:45_

---

_Label `documentation` added by @ntBre on 2025-04-09 14:45_

---

_Label `preview` added by @ntBre on 2025-04-09 14:45_

---

_Label `internal` added by @ntBre on 2025-04-09 14:45_

---

_Label `documentation` removed by @ntBre on 2025-04-09 14:46_

---

_Label `preview` removed by @ntBre on 2025-04-09 14:46_

---

_Merged by @ntBre on 2025-04-09 14:46_

---

_Closed by @ntBre on 2025-04-09 14:46_

---
