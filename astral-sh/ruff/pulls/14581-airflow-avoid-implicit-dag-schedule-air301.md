```yaml
number: 14581
title: "[airflow] Avoid implicit DAG schedule (AIR301)"
type: pull_request
state: merged
author: uranusjr
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: airflow-upgrade-dag-schedule-argument
created_at: 2024-11-25T07:31:37Z
updated_at: 2024-11-26T12:38:18Z
url: https://github.com/astral-sh/ruff/pull/14581
synced_at: 2026-01-10T20:50:57Z
```

# [airflow] Avoid implicit DAG schedule (AIR301)

---

_Pull request opened by @uranusjr on 2024-11-25 07:31_

## Summary

Airflow 3.0 changes the default of the 'schedule' argument. This causes incompatibility if a DAG previously does not specify a schedule explicitly. This is against best practice anyway---you should always prefer to set a schedule explicitly.

Due to backward compatibility, Airflow 2 also possesses multiple arguments to set the schedule. These are all being combined into 'schedule' in 3.0. Usages of those old arguments are also detected.

## Test Plan

A test fixture has been included for the rule.


---

_Renamed from "[airflow] Avoid implicit DAG schedule (AIR002)" to "[airflow] Avoid implicit DAG schedule (AIR301)" by @uranusjr on 2024-11-25 07:32_

---

_Comment by @github-actions[bot] on 2024-11-25 07:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+35 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+35 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/airflow/models/dag.py#L296'>airflow/models/dag.py:296:16:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/performance/src/performance_dags/performance_dag/performance_dag.py#L230'>performance/src/performance_dags/performance_dag/performance_dag.py:230:11:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/src/airflow/providers/arangodb/example_dags/example_arangodb.py#L25'>providers/src/airflow/providers/arangodb/example_dags/example_arangodb.py:25:7:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/src/airflow/providers/google/cloud/example_dags/example_cloud_task.py#L41'>providers/src/airflow/providers/google/cloud/example_dags/example_cloud_task.py:41:6:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/src/airflow/providers/google/cloud/example_dags/example_facebook_ads_to_gcs.py#L61'>providers/src/airflow/providers/google/cloud/example_dags/example_facebook_ads_to_gcs.py:61:6:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/src/airflow/providers/google/cloud/example_dags/example_looker.py#L31'>providers/src/airflow/providers/google/cloud/example_dags/example_looker.py:31:6:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/src/airflow/providers/google/cloud/example_dags/example_presto_to_gcs.py#L52'>providers/src/airflow/providers/google/cloud/example_dags/example_presto_to_gcs.py:52:6:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/src/airflow/providers/google/cloud/example_dags/example_salesforce_to_gcs.py#L47'>providers/src/airflow/providers/google/cloud/example_dags/example_salesforce_to_gcs.py:47:6:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/src/airflow/providers/google/marketing_platform/example_dags/example_display_video.py#L124'>providers/src/airflow/providers/google/marketing_platform/example_dags/example_display_video.py:124:6:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/src/airflow/providers/google/marketing_platform/example_dags/example_display_video.py#L172'>providers/src/airflow/providers/google/marketing_platform/example_dags/example_display_video.py:172:6:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/src/airflow/providers/google/marketing_platform/example_dags/example_display_video.py#L91'>providers/src/airflow/providers/google/marketing_platform/example_dags/example_display_video.py:91:6:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/src/airflow/providers/oracle/example_dags/example_oracle.py#L25'>providers/src/airflow/providers/oracle/example_dags/example_oracle.py:25:6:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/tests/google/cloud/operators/test_looker.py#L47'>providers/tests/google/cloud/operators/test_looker.py:47:19:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/tests/google/cloud/utils/airflow_util.py#L54'>providers/tests/google/cloud/utils/airflow_util.py:54:15:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/tests/google/cloud/utils/test_mlengine_operator_utils.py#L64'>providers/tests/google/cloud/utils/test_mlengine_operator_utils.py:64:12:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/tests/integration/mongo/sensors/test_mongo.py#L49'>providers/tests/integration/mongo/sensors/test_mongo.py:49:20:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/tests/integration/qdrant/operators/test_qdrant_ingest.py#L38'>providers/tests/integration/qdrant/operators/test_qdrant_ingest.py:38:20:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/tests/integration/redis/sensors/test_redis_key.py#L35'>providers/tests/integration/redis/sensors/test_redis_key.py:35:20:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/tests/integration/redis/sensors/test_redis_pub_sub.py#L38'>providers/tests/integration/redis/sensors/test_redis_pub_sub.py:38:20:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/tests/integration/ydb/operators/test_ydb.py#L51'>providers/tests/integration/ydb/operators/test_ydb.py:51:20:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/tests/system/apache/kafka/example_dag_event_listener.py#L85'>providers/tests/system/apache/kafka/example_dag_event_listener.py:85:6:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/tests/system/asana/example_asana.py#L49'>providers/tests/system/asana/example_asana.py:49:6:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/providers/tests/system/google/cloud/cloud_functions/example_functions.py#L81'>providers/tests/system/google/cloud/cloud_functions/example_functions.py:81:6:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/task_sdk/tests/dags/super_basic.py#L24'>task_sdk/tests/dags/super_basic.py:24:2:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/task_sdk/tests/dags/super_basic_run.py#L30'>task_sdk/tests/dags/super_basic_run.py:30:2:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/task_sdk/tests/defintions/test_baseoperator.py#L283'>task_sdk/tests/defintions/test_baseoperator.py:283:23:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/task_sdk/tests/defintions/test_baseoperator.py#L352'>task_sdk/tests/defintions/test_baseoperator.py:352:10:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/task_sdk/tests/defintions/test_baseoperator.py#L359'>task_sdk/tests/defintions/test_baseoperator.py:359:10:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/task_sdk/tests/defintions/test_dag.py#L296'>task_sdk/tests/defintions/test_dag.py:296:9:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/task_sdk/tests/defintions/test_dag.py#L339'>task_sdk/tests/defintions/test_dag.py:339:14:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/task_sdk/tests/defintions/test_dag.py#L345'>task_sdk/tests/defintions/test_dag.py:345:16:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/tests/jobs/test_scheduler_job.py#L6434'>tests/jobs/test_scheduler_job.py:6434:16:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/tests/models/test_dag.py#L2855'>tests/models/test_dag.py:2855:10:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/tests/serialization/test_serialized_objects.py#L218'>tests/serialization/test_serialized_objects.py:218:47:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/a832c418f351d201656d685f57d400eee65925a2/tests/utils/test_log_handlers.py#L273'>tests/utils/test_log_handlers.py:273:15:</a> AIR301 DAG should have an explicit `schedule` argument
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR301 | 35 | 35 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:11 on 2024-11-25 09:54_

```suggestion
/// Checks that the `DAG()` class or a `@dag()` decorator without an explicit
/// `schedule` parameter.
```

It then reads better with *Why is this bad*

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:17 on 2024-11-25 09:55_

Can we tell a bit more about *how* it breaks existing DAGS that use the implicit default. Will it fail with an exception or is it just that the default is different?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:20 on 2024-11-25 09:57_

I'm a bit torn about whether this is something that belongs to this rule or it is a better fit for the `Airflow3Deprecation` rule. I can see how it fits here because it is `DAG` related but it feels a bit out of scope reading through the rule's description (It isn't mentioned at all in the *What it does* section)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:94 on 2024-11-25 09:59_

Nit:
```suggestion
            .is_some_and(|arg| matches!(arg, "timetable" | "schedule_interval")
```

---

_@MichaReiser reviewed on 2024-11-25 09:59_

This overall looks good to me, but I think we have to figure out the right scoping for all the Airflow3 deprecation rules. 


---

_Label `rule` added by @MichaReiser on 2024-11-25 10:00_

---

_Label `preview` added by @MichaReiser on 2024-11-25 10:00_

---

_@uranusjr reviewed on 2024-11-25 10:55_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:20 on 2024-11-25 10:55_

I agree with the assessment TBH. No opinions from me, I can do either.

---

_@MichaReiser reviewed on 2024-11-25 12:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:20 on 2024-11-25 12:01_

I've a slight preference to move it into the other deprecation rule or make it its own rule. 

For my understanding, are `schedule_interval`, and `timetable` now deprecated or have they been deprecated and were removed as part of airflow 3? Depending on the answer, I think a separate rule makes more sense

---

_@uranusjr reviewed on 2024-11-26 08:32_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:20 on 2024-11-26 08:32_

They have been deprecated for a while, and removed as a part of 3.0.

---

_@uranusjr reviewed on 2024-11-26 08:51_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:17 on 2024-11-26 08:51_

Just the default is different—your DAGs will simply stop doing anything silently. I’ll add a paragraph to call this out.

---

_@uranusjr reviewed on 2024-11-26 08:56_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:94 on 2024-11-26 08:56_

I needed to do a conversion since `arg` is `&Identifier` and can’t be matched to `&str` as-is. Hope that’s good enough.

---

_@MichaReiser reviewed on 2024-11-26 09:20_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:20 on 2024-11-26 09:20_

I suggest moving them to a separate rule. Maybe one that catches all Airflow 3 removals? 

---

_@uranusjr reviewed on 2024-11-26 10:11_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:20 on 2024-11-26 10:11_

Got it, I’ll move them to #14582 (AIR302) then.

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/dag_schedule_argument.rs`:20 on 2024-11-26 10:23_

Alright done.

---

_@uranusjr reviewed on 2024-11-26 10:23_

---

_@MichaReiser approved on 2024-11-26 12:38_

This is great. Thank you :) 

---

_Merged by @MichaReiser on 2024-11-26 12:38_

---

_Closed by @MichaReiser on 2024-11-26 12:38_

---
