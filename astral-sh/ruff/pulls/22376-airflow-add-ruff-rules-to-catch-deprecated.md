```yaml
number: 22376
title: "[`airflow`] Add ruff rules to catch deprecated Airflow imports for Airflow 3.1 (`AIR321`)"
type: pull_request
state: open
author: sjyangkevin
labels:
  - rule
assignees: []
base: main
head: catch-deprecated-imports-airflow-3_1
created_at: 2026-01-04T21:44:45Z
updated_at: 2026-01-19T09:18:41Z
url: https://github.com/astral-sh/ruff/pull/22376
synced_at: 2026-01-19T09:27:02Z
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
ℹ️ ecosystem check **detected linter changes**. (+450 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+450 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/airflow-core/tests/unit/ti_deps/deps/test_task_concurrency.py#L35'>airflow-core/tests/unit/ti_deps/deps/test_task_concurrency.py:35:16:</a> AIR321 `airflow.models.baseoperator.BaseOperator` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/dev/airflow_perf/scheduler_dag_execution_timing.py#L181'>dev/airflow_perf/scheduler_dag_execution_timing.py:181:24:</a> AIR321 [*] `airflow.utils.timezone.utcnow` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/kubernetes-tests/tests/kubernetes_tests/test_kubernetes_pod_operator.py#L59'>kubernetes-tests/tests/kubernetes_tests/test_kubernetes_pod_operator.py:59:20:</a> AIR321 [*] `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L83'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:83:27:</a> AIR321 [*] `airflow.utils.timezone.utcnow` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/alibaba/tests/unit/alibaba/cloud/log/test_oss_task_handler.py#L59'>providers/alibaba/tests/unit/alibaba/cloud/log/test_oss_task_handler.py:59:26:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/alibaba/tests/unit/alibaba/cloud/sensors/test_analyticdb_spark.py#L26'>providers/alibaba/tests/unit/alibaba/cloud/sensors/test_analyticdb_spark.py:26:16:</a> AIR321 [*] `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/src/airflow/providers/amazon/aws/executors/utils/exponential_backoff_retry.py#L72'>providers/amazon/src/airflow/providers/amazon/aws/executors/utils/exponential_backoff_retry.py:72:20:</a> AIR321 [*] `airflow.utils.timezone.utcnow` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/system/amazon/aws/example_mongo_to_s3.py#L45'>providers/amazon/tests/system/amazon/aws/example_mongo_to_s3.py:45:16:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/system/amazon/aws/example_mwaa.py#L121'>providers/amazon/tests/system/amazon/aws/example_mwaa.py:121:35:</a> AIR321 [*] `airflow.utils.timezone.utc` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/system/amazon/aws/example_mwaa.py#L151'>providers/amazon/tests/system/amazon/aws/example_mwaa.py:151:35:</a> AIR321 [*] `airflow.utils.timezone.utc` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py#L878'>providers/amazon/tests/unit/amazon/aws/executors/ecs/test_ecs_executor.py:878:77:</a> AIR321 `airflow.utils.timezone.utcnow` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L325'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:325:25:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L326'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:326:23:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L585'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:585:74:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L586'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:586:75:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L601'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:601:66:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L602'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:602:67:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L694'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:694:74:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L695'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:695:75:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L725'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:725:74:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L726'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:726:75:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L762'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:762:41:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L800'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:800:74:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L801'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:801:75:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L928'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:928:41:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L934'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:934:41:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L945'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:945:41:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L951'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:951:41:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py#L995'>providers/amazon/tests/unit/amazon/aws/hooks/test_s3.py:995:41:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py#L143'>providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py:143:26:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py#L197'>providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py:197:16:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py#L263'>providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py:263:26:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py#L336'>providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py:336:17:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py#L337'>providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py:337:42:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py#L391'>providers/amazon/tests/unit/amazon/aws/log/test_cloudwatch_task_handler.py:391:33:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L651'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:651:24:</a> AIR321 `airflow.utils.timezone.utcnow` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L652'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:652:42:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L676'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:676:27:</a> AIR321 `airflow.utils.timezone.utcnow` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L707'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:707:15:</a> AIR321 `airflow.utils.timezone.utcnow` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L796'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:796:17:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L803'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:803:17:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L804'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:804:17:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L832'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:832:17:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L839'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:839:17:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L840'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:840:17:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/transfers/test_s3_to_sftp.py#L47'>providers/amazon/tests/unit/amazon/aws/transfers/test_s3_to_sftp.py:47:16:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/amazon/tests/unit/amazon/aws/transfers/test_sftp_to_s3.py#L45'>providers/amazon/tests/unit/amazon/aws/transfers/test_sftp_to_s3.py:45:16:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/apache/flink/tests/unit/apache/flink/operators/test_flink_kubernetes.py#L200'>providers/apache/flink/tests/unit/apache/flink/operators/test_flink_kubernetes.py:200:51:</a> AIR321 [*] `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/apache/flink/tests/unit/apache/flink/sensors/test_flink_kubernetes.py#L886'>providers/apache/flink/tests/unit/apache/flink/sensors/test_flink_kubernetes.py:886:51:</a> AIR321 [*] `airflow.utils.timezone.datetime` is removed in Airflow 3.1
+ <a href='https://github.com/apache/airflow/blob/477d53a53a9f86e5410f90ff0cb198d46a43efba/providers/apache/hdfs/tests/unit/apache/hdfs/log/test_hdfs_task_handler.py#L37'>providers/apache/hdfs/tests/unit/apache/hdfs/log/test_hdfs_task_handler.py:37:16:</a> AIR321 `airflow.utils.timezone.datetime` is removed in Airflow 3.1
... 400 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR321 | 450 | 450 | 0 | 0 | 0 |

</p>
</details>





---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:36 on 2026-01-04 21:55_

wondering if it is the right version to set.

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR321_AIR321_names_fix.py.snap`:1 on 2026-01-04 21:56_

The test cases in this snapshot do not raise any violation. The `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR321_AIR321_names.py.snap` is the "unhappy path", and this file is the "happy path". Wondering if there is a better way to implement these tests, would appreciate if there is any feedback.

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:112 on 2026-01-04 21:57_

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

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/suggested_to_update_3_0.rs`:1 on 2026-01-14 04:16_

I noticed we also have some rules for module moved to task sdk. So, we can also refine those messages here using the new `SourceModuleMovedToSDK` Replacement. We might need to check which task sdk version.

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR301_AIR301_names.py.snap`:1 on 2026-01-14 04:16_

This is moved to AIR321.

---

_@sjyangkevin reviewed on 2026-01-14 04:23_

Hi @Lee-W and @amoghrajesh ,

I've refined the rules based on the feedback. I not sure if we can make a rule a "warning" in ruff; currently, I introduce a new Replacement `SourceModuleMovedToSDK` which we can use to embed a warning message. Let me know if it is the right approach.

Below is the summary of changes.
1. Exclude the followings
> airflow.utils.task_group.get_task_group_children_getter
> airflow.utils.task_group.task_group_to_dict
2. Fix Import Path in AIR311
> airflow.sensors.base.poke_mode_only → airflow.sdk.bases.sensor.poke_mode_only
3. Move from AIR301 to AIR321
> airflow.secrets.cache.SecretCache → from airflow.sdk import SecretCache
4. Keep `_internal` modules and show a warning message to indicate these are internal API that may change without notice.
5. Update the other rules to adapt for the new Replacement `SourceModuleMovedToSDK`

Thanks!!

---

_Review requested from @amoghrajesh by @sjyangkevin on 2026-01-14 04:27_

---

_Comment by @Lee-W on 2026-01-14 09:28_

>  I not sure if we can make a rule a "warning" in ruff

A Diagnotic without fix is basically a warning

---

_@Lee-W reviewed on 2026-01-14 09:28_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR301_AIR301_names.py.snap`:1 on 2026-01-14 09:28_

It might be worth checking how it works in Airflow 3.0.x. If the original path works fine, we're safe to move it to AIR321

---

_@sjyangkevin reviewed on 2026-01-14 16:09_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR301_AIR301_names.py.snap`:1 on 2026-01-14 16:09_

checked in 3.0.6, and the original path still working.

<img width="1054" height="197" alt="Screenshot from 2026-01-14 11-08-38" src="https://github.com/user-attachments/assets/20331dba-541e-43da-b0a7-ccb507150c8d" />


---

_Comment by @sjyangkevin on 2026-01-14 16:20_

> > I not sure if we can make a rule a "warning" in ruff
> 
> A Diagnotic without fix is basically a warning

Thanks! The current implementation proposes a fix and include a warning message. I can refactor this to use a simple message to notify the module move, and include a message to tell these APIs are subject to change (don't encourage using these). So, we don't suggest fix for it. Which one do you think could be a better idea?

Current Implementation suggest fixes and show warning message.
<img width="1083" height="383" alt="Screenshot from 2026-01-14 11-17-28" src="https://github.com/user-attachments/assets/1d5b1bb5-5818-4048-aa6a-1071b46f534d" />


---

_@sjyangkevin reviewed on 2026-01-15 15:06_

Hi @Lee-W and @amoghrajesh , I made a few adjustments to the PR and I think it is almost ready for open it again for review. Let me know if you have any feedback before opening it again. Below is the summary of changes made.

1. Move `SecretCache` from AIR301 to AIR321 (tested in Airflow 3.0.6 and the original import is still valid)
2. Move the test case for `SecretCache` from AIR301 to AIR321
3. For `_internal` modules, the rule is updated to use `Message` to show a warning. Hence, the `warning_message` field is removed from the `SourceModuleMovedToSDK`. This field is only used for the internal module cases, but will be `None` for majority.
4. Update the `fix_title` for `SourceModuleMovedToSDK` to **"`{name}` has been moved to `{module}` since Airflow 3.x (with apache-airflow-task-sdk>={version})."**, where version is `1.0.6` for rules in AIR301, and `1.1.6` for rules in AIR321.
5. For AIR321, update `preview_since` to `0.14.12`
6. Test snapshots are all updated.
7. Comments in https://github.com/apache/airflow/issues/54714#issuecomment-3741920944 are resolved.

Thanks!

<img width="988" height="224" alt="Screenshot from 2026-01-15 10-22-16" src="https://github.com/user-attachments/assets/fb11db9f-83b5-4be5-b36e-45ef568b29d1" />

<img width="985" height="295" alt="Screenshot from 2026-01-15 10-21-47" src="https://github.com/user-attachments/assets/a71fddfb-fdf1-4dc0-97bf-827bf54de7a7" />


---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:141 on 2026-01-16 08:30_

```suggestion
        [
            "airflow",
            "utils",
            "setup_teardown",
            rest @ ("BaseSetupTeardownContext", "SetupTeardownContext")
        ] => Replacement::Message(
            format!(
                "`{rest}` has been moved to `airflow.sdk.definitions._internal.setup_teardown` \
                since Airflow 3.1. This is an internal module and is subject to change without notice."
            ),
        )
```

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:151 on 2026-01-16 08:31_

I think `airflow.models` is not supposed to be used. We should make it a warning message.

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:184 on 2026-01-16 08:32_

```suggestion
        [
            "airflow",
            "utils",
            "decorators",
            rest @ ("fixup_decorator_warning_stack", "remove_task_decorator"),
        ] => Replacement::Message(
            "`{rest}` has been moved to `airflow.sdk.definitions._internal.decorators` \
            since Airflow 3.1. This is an internal module and is subject to change without notice.",
        ),
```

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:184 on 2026-01-16 08:33_

we can apply ths to elsewhere as well

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:184 on 2026-01-16 08:33_

or even make it a new enum item? the message are similair

---

_@Lee-W reviewed on 2026-01-16 08:33_

---

_@sjyangkevin reviewed on 2026-01-16 14:30_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:184 on 2026-01-16 14:30_

Thanks for the feedback! I attempted to use `format!` to template the message and pass the `rest`, but it returns a `String` type. However, `Message` is static string and has type error. I will play around with it and see if there is a way to use `Message`, if not, we can create a new enum item.

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/helpers.rs`:50 on 2026-01-16 20:44_

add a new item in the enum which used to show warning message for internal module

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:151 on 2026-01-16 20:45_

updated this to show a warning. `["airflow", "utils", "xcom", "XCOM_RETURN_KEY"] => Replacement::InternalModule { ...`

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR321_AIR321_names.py.snap`:24 on 2026-01-16 20:46_

updated warning message.

---

_@sjyangkevin reviewed on 2026-01-16 20:46_

---

_Review requested from @Lee-W by @sjyangkevin on 2026-01-16 20:48_

---

_@sjyangkevin reviewed on 2026-01-19 04:16_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:1031 on 2026-01-19 04:16_

Here, we leveraged the `suggest_fix` parameter from `SourceModuleMovedWithMessage` to handle when we would like to show a warning and when we would like to suggest a fix with custom message.

---

_@sjyangkevin reviewed on 2026-01-19 04:20_

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/helpers.rs`:49 on 2026-01-19 04:20_

Create this new enum item which can be used as follow:
1. The `fix_title` format is: "`{name}` has been moved to `{module}` since Airflow 3.1. `{message}`"; we can use the `message` parameter to include a static string as additional message that we want to show to users.
2. The `suggest_fix` parameter is used to optionally report diagnostics. In some cases, we might want to just show a warning (e.g., internal module usage), but in some case, we might want to report diagnostic and suggest fix (e.g., `BaseHook` is moved from one module to another)

Why `message` is a static string. Use the following example to explain. Making `message` a `String` allow us to use `format!` to templating the string and render it at runtime. However, only `rest` or variables defined in the accessible scope can be used to render the string. As `module` and `name` will be handled in `fix_title`. It might not be necessary to use those values to render the `message`. So, here use static string.

```rust
[
            "airflow",
            "utils",
            "setup_teardown",
            rest @ ("BaseSetupTeardownContext" | "SetupTeardownContext"),
        ] => Replacement::SourceModuleMovedWithMessage {
            module: "airflow.sdk.definitions._internal.setup_teardown",
            name: rest.to_string(),
            message: "This is an internal module which is not suggested to be used and is subject to change without notice.",
            suggest_fix: false,
        },
```

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3_1.rs`:148 on 2026-01-19 04:21_

Even though we've parameterized the message, this message can repeat multiple times in the code. Wonder if there is a good way to handle it.

---

_Review comment by @sjyangkevin on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR301_AIR301_names_fix.py.snap`:620 on 2026-01-19 04:23_

the `BaseHook` message is updated.

---

_@sjyangkevin reviewed on 2026-01-19 04:26_

Hi @Lee-W , I've made some updates to the way showing custom message. Let me know if you have further feedback. Thanks again for your time to review!

I also included the fix for https://github.com/apache/airflow/issues/52962

---

_Marked ready for review by @sjyangkevin on 2026-01-19 04:30_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:698 on 2026-01-19 08:46_

```suggestion
                message: "Import `BaseHook` from `airflow.hooks.base` is suggested in Airflow 3.0, but it is deprecated in Airflow 3.1. Follow the instructions of AIR321 if you're using 3.1+.",
```

We can improve the message to something like this

---

_@Lee-W reviewed on 2026-01-19 08:55_

most good, one nit

---

_Comment by @amoghrajesh on 2026-01-19 09:18_

Checked: https://github.com/sjyangkevin/ruff/blob/b1141bd88163d629c8022c9c0673251b0124829d/crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR321_AIR321_names.py.snap#L298 and I have no objections, just two qns:

1. [https://github.com/sjyangkevin/ruff/blob/b1141bd88163d629c8022c9c0673251b0124829d/[…]ruff_linter__rules__airflow__tests__AIR321_AIR321_names.py.snap](https://github.com/sjyangkevin/ruff/blob/b1141bd88163d629c8022c9c0673251b0124829d/crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR321_AIR321_names.py.snap#L298) (why the 1.1.6?)
2. I do not love the: airflow.sdk.execution_time.macros. Better to have it be: airflow.sdk.macros yes? (this one's on me though)

---
