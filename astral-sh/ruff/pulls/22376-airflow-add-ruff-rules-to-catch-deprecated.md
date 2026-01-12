```yaml
number: 22376
title: "[`airflow`] Add ruff rules to catch deprecated Airflow imports for Airflow 3.1 (`AIR321`)"
type: pull_request
state: open
author: sjyangkevin
labels:
  - rule
assignees: []
draft: true
base: main
head: catch-deprecated-imports-airflow-3_1
created_at: 2026-01-04T21:44:45Z
updated_at: 2026-01-07T04:19:40Z
url: https://github.com/astral-sh/ruff/pull/22376
synced_at: 2026-01-12T15:57:48Z
```

# [`airflow`] Add ruff rules to catch deprecated Airflow imports for Airflow 3.1 (`AIR321`)

---

_@sjyangkevin_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR is related to the discussion: https://github.com/apache/airflow/issues/54714

This change creates a new code (`AIR321`) and implement ruff rules to catch, and/or fix deprecated imports in Airflow for **Airflow 3.1**. The rules are implemented by following the structure of `AIR301`. The rules check whether a removed Airflow name is used, and match on `Expr::Name` and `Expr::Attribute`.

## Test Plan

The following two test files are added:
1. crates/ruff_linter/resources/test/fixtures/airflow/AIR321_names.py
2. crates/ruff_linter/resources/test/fixtures/airflow/AIR321_names_fix.py 

`AIR321_names.py`
All the test cases in this file should raise violations and fixes should be suggested when running the test with `--unsafe-fixes`. The test results shown in the snapshot are expected.
```bash
cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/airflow/AIR321_names.py --no-cache --preview --select AIR321 --unsafe-fixes
```
`AIR321_names_fix.py `
All the test cases in this file raise NO violation (i.e., all checks should pass). The snapshot file is empty.

<img width="1330" height="384" alt="Screenshot from 2026-01-04 16-37-51" src="https://github.com/user-attachments/assets/8a1d6dca-dc69-41f8-ba9f-822e0bcd04a7" />

## Document Update

<img width="942" height="56" alt="Screenshot from 2026-01-04 16-37-02" src="https://github.com/user-attachments/assets/e678837d-895f-483b-94a0-58dde7c60032" />



---

_Comment by @astral-sh-bot[bot] on 2026-01-04 21:53_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+469 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+469 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/airflow-core/tests/unit/ti_deps/deps/test_task_concurrency.py#L35'>airflow-core/tests/unit/ti_deps/deps/test_task_concurrency.py:35:16:</a> AIR321 `airflow.models.baseoperator.BaseOperator` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/dev/airflow_perf/scheduler_dag_execution_timing.py#L181'>dev/airflow_perf/scheduler_dag_execution_timing.py:181:24:</a> AIR321 [*] `airflow.utils.timezone.utcnow` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/kubernetes-tests/tests/kubernetes_tests/test_kubernetes_pod_operator.py#L59'>kubernetes-tests/tests/kubernetes_tests/test_kubernetes_pod_operator.py:59:20:</a> AIR321 [*] `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L83'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:83:27:</a> AIR321 [*] `airflow.utils.timezone.utcnow` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/alibaba/tests/unit/alibaba/cloud/log/test_oss_task_handler.py#L59'>providers/alibaba/tests/unit/alibaba/cloud/log/test_oss_task_handler.py:59:26:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/alibaba/tests/unit/alibaba/cloud/sensors/test_analyticdb_spark.py#L26'>providers/alibaba/tests/unit/alibaba/cloud/sensors/test_analyticdb_spark.py:26:16:</a> AIR321 [*] `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/src/airflow/providers/amazon/aws/executors/utils/exponential_backoff_retry.py#L72'>providers/amazon/src/airflow/providers/amazon/aws/executors/utils/exponential_backoff_retry.py:72:20:</a> AIR321 [*] `airflow.utils.timezone.utcnow` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/system/amazon/aws/example_mongo_to_s3.py#L45'>providers/amazon/tests/system/amazon/aws/example_mongo_to_s3.py:45:16:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/system/amazon/aws/example_mwaa.py#L121'>providers/amazon/tests/system/amazon/aws/example_mwaa.py:121:35:</a> AIR321 [*] `airflow.utils.timezone.utc` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/system/amazon/aws/example_mwaa.py#L151'>providers/amazon/tests/system/amazon/aws/example_mwaa.py:151:35:</a> AIR321 [*] `airflow.utils.timezone.utc` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py#L878'>providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py:878:77:</a> AIR321 `airflow.utils.timezone.utcnow` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L325'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:325:25:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L326'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:326:23:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L585'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:585:74:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L586'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:586:75:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L601'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:601:66:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L602'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:602:67:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L694'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:694:74:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L695'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:695:75:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L725'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:725:74:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L726'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:726:75:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L762'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:762:41:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L800'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:800:74:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L801'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:801:75:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L928'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:928:41:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L934'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:934:41:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L945'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:945:41:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L951'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:951:41:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L995'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:995:41:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py#L143'>providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py:143:26:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py#L197'>providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py:197:16:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py#L263'>providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py:263:26:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py#L336'>providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py:336:17:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py#L337'>providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py:337:42:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py#L391'>providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py:391:33:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L651'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:651:24:</a> AIR321 `airflow.utils.timezone.utcnow` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L652'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:652:42:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L676'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:676:27:</a> AIR321 `airflow.utils.timezone.utcnow` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L707'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:707:15:</a> AIR321 `airflow.utils.timezone.utcnow` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L796'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:796:17:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L803'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:803:17:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L804'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:804:17:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L832'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:832:17:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L839'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:839:17:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L840'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:840:17:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/transfers/test_s3_to_sftp.py#L47'>providers/amazon/tests/unit/amazon/aws/transfers/test_s3_to_sftp.py:47:16:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/amazon/tests/unit/amazon/aws/transfers/test_sftp_to_s3.py#L45'>providers/amazon/tests/unit/amazon/aws/transfers/test_sftp_to_s3.py:45:16:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/apache/flink/tests/unit/apache/flink/operators/test_flink_kubernetes.py#L200'>providers/apache/flink/tests/unit/apache/flink/operators/test_flink_kubernetes.py:200:51:</a> AIR321 [*] `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/apache/flink/tests/unit/apache/flink/sensors/test_flink_kubernetes.py#L886'>providers/apache/flink/tests/unit/apache/flink/sensors/test_flink_kubernetes.py:886:51:</a> AIR321 [*] `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/02a254df3dc98367856ad210ef9e77be810e9c09/providers/apache/hdfs/tests/unit/apache/hdfs/log/test_hdfs_task_handler.py#L37'>providers/apache/hdfs/tests/unit/apache/hdfs/log/test_hdfs_task_handler.py:37:16:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
... 419 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR321 | 469 | 469 | 0 | 0 | 0 |

</p>
</details>





---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:36 on 2026-01-04 21:55_

wondering if it is the right version to set.

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR321_AIR321_names_fix.py.snap`:1 on 2026-01-04 21:56_

The test cases in this snapshot do not raise any violation. The `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR321_AIR321_names.py.snap` is the "unhappy path", and this file is the "happy path". Wondering if there is a better way to implement these tests, would appreciate if there is any feedback.

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:91 on 2026-01-04 21:57_

At the moment, we are only checking for deprecated imports, but in future there could be more cases come in.

---

_@sjyangkevin reviewed on 2026-01-04 21:57_

---

_Comment by @MichaReiser on 2026-01-05 10:23_

@Lee-W could you take a look at the PR if it catches the semantics you want.

---

_Label `rule` added by @MichaReiser on 2026-01-05 10:23_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:124 on 2026-01-05 10:32_

```suggestion
        ] => Replacement::SourceModuleMoved {
```

I think it makes more sense to use `SourceModuleMoved`. Same for the following ones without rename

---

@amoghrajesh should we use these `_internal` thing?

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:158 on 2026-01-05 10:35_

```suggestion
        ["airflow", "utils", "timezone", rest @ ("convert_to_utc", "datetime", "make_naive")] => Replacement::SourceModuleMoved {
            module: "airflow.sdk.timezone",
            name: format!("conf.{rest}"),
        },
```

we can try something like this in many places

---

_@Lee-W reviewed on 2026-01-05 10:36_

---

_Comment by @Lee-W on 2026-01-05 10:36_

> @Lee-W could you take a look at the PR if it catches the semantics you want.

At a high level, yes, but some details will need some polish. Will let you know when it's at least ok from my side. Thanks!

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:158 on 2026-01-05 13:27_

Thanks for the feedback. I will group things if they are under the same submodules.

---

_@sjyangkevin reviewed on 2026-01-05 13:27_

---

_@sjyangkevin reviewed on 2026-01-06 03:57_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:124 on 2026-01-06 03:57_

> I think it makes more sense to use `SourceModuleMoved`. Same for the following ones without rename

updated to use `SourceModuleMoved` instead of `Rename` as only the locations are changed. Thanks for the feedback.



---

_@sjyangkevin reviewed on 2026-01-06 03:57_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:158 on 2026-01-06 03:57_

related imports have been grouped together.

---

_Review requested from @Lee-W by @sjyangkevin on 2026-01-06 04:05_

---

_Review comment by @amoghrajesh on `crates/ruff_linter/resources/test/fixtures/airflow/AIR321_names_fix.py`:7 on 2026-01-06 07:10_

I wonder if there is even a need to have this in the public API? If so, we should not be importing it from `_internal` ever and add it to lazy imports here: https://github.com/apache/airflow/blob/main/task-sdk/src/airflow/sdk/__init__.py#L113

---

_Review comment by @amoghrajesh on `crates/ruff_linter/resources/test/fixtures/airflow/AIR321_names_fix.py`:15 on 2026-01-06 07:11_

This is true if the usage is in `airflow-core` but otherwise its inside `BaseXcom` now: https://github.com/apache/airflow/blob/main/task-sdk/src/airflow/sdk/bases/xcom.py#L50

Not sure what you can do about it

---

_Review comment by @amoghrajesh on `crates/ruff_linter/resources/test/fixtures/airflow/AIR321_names_fix.py`:48 on 2026-01-06 07:13_

We should not reference internal modules. We do not need to maintain compat for these I'd say

---

_Review comment by @amoghrajesh on `crates/ruff_linter/resources/test/fixtures/airflow/AIR321_names_fix.py`:65 on 2026-01-06 07:14_

```suggestion
from airflow.sdk import BaseOperator
```

---

_@amoghrajesh reviewed on 2026-01-06 07:16_

---

_@Lee-W reviewed on 2026-01-06 08:58_

---

_Review comment by @Lee-W on `crates/ruff_linter/resources/test/fixtures/airflow/AIR321_names_fix.py`:15 on 2026-01-06 08:58_

We can add a warning message instead of a fix, but isn't that a breaking change?

---

_Comment by @Lee-W on 2026-01-06 09:03_

The logic looks solid, but we’ll need to revisit the list. @sjyangkevin, could you please make this a draft? We can discuss the list further in relation to the Airflow issue. Thanks!

---

_Converted to draft by @sjyangkevin on 2026-01-06 13:16_

---

_Comment by @sjyangkevin on 2026-01-06 13:38_

@Lee-W and @amoghrajesh , thanks for the feedback! I have converted it into a draft. I will also happy to work on the fix after the further discussion.

---
