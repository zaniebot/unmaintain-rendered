```yaml
number: 10590
title: "[`DTZ`] Improve docs for `call-datetime-without-tzinfo` (`DTZ005`)"
type: pull_request
state: closed
author: cclauss
labels:
  - documentation
assignees: []
base: main
head: patch-1
created_at: 2024-03-25T21:21:39Z
updated_at: 2024-03-27T20:03:38Z
url: https://github.com/astral-sh/ruff/pull/10590
synced_at: 2026-01-12T15:55:32Z
```

# [`DTZ`] Improve docs for `call-datetime-without-tzinfo` (`DTZ005`)

---

_@cclauss_

Fixes #10251
* #10251

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-03-25 21:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+109 -109 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+100 -100 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/hooks/s3.py#L699'>airflow/providers/amazon/aws/hooks/s3.py:699:34:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/hooks/s3.py#L699'>airflow/providers/amazon/aws/hooks/s3.py:699:34:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/hooks/s3.py#L714'>airflow/providers/amazon/aws/hooks/s3.py:714:38:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/hooks/s3.py#L714'>airflow/providers/amazon/aws/hooks/s3.py:714:38:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/hooks/s3.py#L738'>airflow/providers/amazon/aws/hooks/s3.py:738:34:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/hooks/s3.py#L738'>airflow/providers/amazon/aws/hooks/s3.py:738:34:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L291'>airflow/providers/amazon/aws/sensors/s3.py:291:39:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L291'>airflow/providers/amazon/aws/sensors/s3.py:291:39:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L301'>airflow/providers/amazon/aws/sensors/s3.py:301:43:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L301'>airflow/providers/amazon/aws/sensors/s3.py:301:43:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L319'>airflow/providers/amazon/aws/sensors/s3.py:319:44:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L319'>airflow/providers/amazon/aws/sensors/s3.py:319:44:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L322'>airflow/providers/amazon/aws/sensors/s3.py:322:39:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L322'>airflow/providers/amazon/aws/sensors/s3.py:322:39:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py#L300'>airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py:300:25:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py#L300'>airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py:300:25:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py#L307'>airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py:307:25:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py#L307'>airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py:307:25:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/fab/auth_manager/security_manager/override.py#L1641'>airflow/providers/fab/auth_manager/security_manager/override.py:1641:31:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/fab/auth_manager/security_manager/override.py#L1641'>airflow/providers/fab/auth_manager/security_manager/override.py:1641:31:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/google/cloud/hooks/bigquery.py#L1554'>airflow/providers/google/cloud/hooks/bigquery.py:1554:14:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/google/cloud/hooks/bigquery.py#L1554'>airflow/providers/google/cloud/hooks/bigquery.py:1554:14:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/google/cloud/sensors/gcs.py#L383'>airflow/providers/google/cloud/sensors/gcs.py:383:12:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/google/cloud/sensors/gcs.py#L383'>airflow/providers/google/cloud/sensors/gcs.py:383:12:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/google/cloud/triggers/gcs.py#L385'>airflow/providers/google/cloud/triggers/gcs.py:385:16:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/google/cloud/triggers/gcs.py#L385'>airflow/providers/google/cloud/triggers/gcs.py:385:16:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/openlineage/plugins/listener.py#L110'>airflow/providers/openlineage/plugins/listener.py:110:84:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/openlineage/plugins/listener.py#L110'>airflow/providers/openlineage/plugins/listener.py:110:84:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/openlineage/plugins/listener.py#L178'>airflow/providers/openlineage/plugins/listener.py:178:78:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/openlineage/plugins/listener.py#L178'>airflow/providers/openlineage/plugins/listener.py:178:78:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/openlineage/plugins/listener.py#L233'>airflow/providers/openlineage/plugins/listener.py:233:78:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/openlineage/plugins/listener.py#L233'>airflow/providers/openlineage/plugins/listener.py:233:78:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/timetables/interval.py#L215'>airflow/timetables/interval.py:215:15:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/timetables/interval.py#L215'>airflow/timetables/interval.py:215:15:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2036'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2036:96:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2036'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2036:96:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2046'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2046:28:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2046'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2046:28:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2058'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2058:72:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2058'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2058:72:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/testing_commands.py#L223'>dev/breeze/src/airflow_breeze/commands/testing_commands.py:223:24:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/testing_commands.py#L223'>dev/breeze/src/airflow_breeze/commands/testing_commands.py:223:24:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/params/common_build_params.py#L131'>dev/breeze/src/airflow_breeze/params/common_build_params.py:131:15:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/params/common_build_params.py#L131'>dev/breeze/src/airflow_breeze/params/common_build_params.py:131:15:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/perf/dags/elastic_dag.py#L155'>dev/perf/dags/elastic_dag.py:155:14:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
... 155 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+9 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py#L8'>docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py:8:11:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py#L8'>docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py:8:11:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L55'>examples/basic/data/server_sent_events_source.py:55:17:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L55'>examples/basic/data/server_sent_events_source.py:55:17:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/faces/main.py#L53'>examples/server/app/faces/main.py:53:6:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/faces/main.py#L53'>examples/server/app/faces/main.py:53:6:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/faces/main.py#L82'>examples/server/app/faces/main.py:82:16:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/faces/main.py#L82'>examples/server/app/faces/main.py:82:16:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L242'>src/bokeh/events.py:242:26:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L242'>src/bokeh/events.py:242:26:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DTZ005 | 218 | 109 | 109 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+109 -109 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+100 -100 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/hooks/s3.py#L699'>airflow/providers/amazon/aws/hooks/s3.py:699:34:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/hooks/s3.py#L699'>airflow/providers/amazon/aws/hooks/s3.py:699:34:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/hooks/s3.py#L714'>airflow/providers/amazon/aws/hooks/s3.py:714:38:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/hooks/s3.py#L714'>airflow/providers/amazon/aws/hooks/s3.py:714:38:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/hooks/s3.py#L738'>airflow/providers/amazon/aws/hooks/s3.py:738:34:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/hooks/s3.py#L738'>airflow/providers/amazon/aws/hooks/s3.py:738:34:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L291'>airflow/providers/amazon/aws/sensors/s3.py:291:39:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L291'>airflow/providers/amazon/aws/sensors/s3.py:291:39:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L301'>airflow/providers/amazon/aws/sensors/s3.py:301:43:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L301'>airflow/providers/amazon/aws/sensors/s3.py:301:43:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L319'>airflow/providers/amazon/aws/sensors/s3.py:319:44:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L319'>airflow/providers/amazon/aws/sensors/s3.py:319:44:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L322'>airflow/providers/amazon/aws/sensors/s3.py:322:39:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/sensors/s3.py#L322'>airflow/providers/amazon/aws/sensors/s3.py:322:39:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py#L300'>airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py:300:25:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py#L300'>airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py:300:25:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py#L307'>airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py:307:25:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py#L307'>airflow/providers/cncf/kubernetes/operators/custom_object_launcher.py:307:25:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/fab/auth_manager/security_manager/override.py#L1641'>airflow/providers/fab/auth_manager/security_manager/override.py:1641:31:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/fab/auth_manager/security_manager/override.py#L1641'>airflow/providers/fab/auth_manager/security_manager/override.py:1641:31:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/google/cloud/hooks/bigquery.py#L1554'>airflow/providers/google/cloud/hooks/bigquery.py:1554:14:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/google/cloud/hooks/bigquery.py#L1554'>airflow/providers/google/cloud/hooks/bigquery.py:1554:14:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/google/cloud/sensors/gcs.py#L383'>airflow/providers/google/cloud/sensors/gcs.py:383:12:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/google/cloud/sensors/gcs.py#L383'>airflow/providers/google/cloud/sensors/gcs.py:383:12:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/google/cloud/triggers/gcs.py#L385'>airflow/providers/google/cloud/triggers/gcs.py:385:16:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/google/cloud/triggers/gcs.py#L385'>airflow/providers/google/cloud/triggers/gcs.py:385:16:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/openlineage/plugins/listener.py#L110'>airflow/providers/openlineage/plugins/listener.py:110:84:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/openlineage/plugins/listener.py#L110'>airflow/providers/openlineage/plugins/listener.py:110:84:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/openlineage/plugins/listener.py#L178'>airflow/providers/openlineage/plugins/listener.py:178:78:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/openlineage/plugins/listener.py#L178'>airflow/providers/openlineage/plugins/listener.py:178:78:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/openlineage/plugins/listener.py#L233'>airflow/providers/openlineage/plugins/listener.py:233:78:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/openlineage/plugins/listener.py#L233'>airflow/providers/openlineage/plugins/listener.py:233:78:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/timetables/interval.py#L215'>airflow/timetables/interval.py:215:15:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/timetables/interval.py#L215'>airflow/timetables/interval.py:215:15:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2036'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2036:96:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2036'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2036:96:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2046'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2046:28:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2046'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2046:28:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2058'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2058:72:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2058'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2058:72:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/testing_commands.py#L223'>dev/breeze/src/airflow_breeze/commands/testing_commands.py:223:24:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/testing_commands.py#L223'>dev/breeze/src/airflow_breeze/commands/testing_commands.py:223:24:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/params/common_build_params.py#L131'>dev/breeze/src/airflow_breeze/params/common_build_params.py:131:15:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/params/common_build_params.py#L131'>dev/breeze/src/airflow_breeze/params/common_build_params.py:131:15:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/perf/dags/elastic_dag.py#L155'>dev/perf/dags/elastic_dag.py:155:14:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
... 155 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+9 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py#L8'>docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py:8:11:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py#L8'>docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py:8:11:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L55'>examples/basic/data/server_sent_events_source.py:55:17:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L55'>examples/basic/data/server_sent_events_source.py:55:17:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/faces/main.py#L53'>examples/server/app/faces/main.py:53:6:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/faces/main.py#L53'>examples/server/app/faces/main.py:53:6:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/faces/main.py#L82'>examples/server/app/faces/main.py:82:16:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/faces/main.py#L82'>examples/server/app/faces/main.py:82:16:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L242'>src/bokeh/events.py:242:26:</a> DTZ005 The use of `datetime.datetime.now()` without `tz` argument is not allowed
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L242'>src/bokeh/events.py:242:26:</a> DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DTZ005 | 218 | 109 | 109 | 0 | 0 |

</p>
</details>




---

_Renamed from "Update call_datetime_now_without_tzinfo.rs" to "[`DTZ`] Update call_datetime_now_without_tzinfo.rs `DTZ005`" by @cclauss on 2024-03-26 02:37_

---

_Label `documentation` added by @MichaReiser on 2024-03-26 07:43_

---

_Assigned to @AlexWaygood by @MichaReiser on 2024-03-26 07:43_

---

_@MichaReiser approved on 2024-03-26 07:43_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-03-26 07:43_

---

_Comment by @AlexWaygood on 2024-03-26 10:32_

Hmm, I'm not sure about just adding a paragraph at the bottom. Possibly you could argue that the first sentence of the documentation for this rule is wrong:

> Checks for usage of `datetime.datetime.now()` without a `tz` argument.

In reality, it does more than that, since it also emits an error if you explicitly pass `tz=None`. Probably it would be more accurate to say:

> Checks for usages of `datetime.datetime.now()` that do not specify a timezone.

Then we could explain in the body of the rule's documentation why `tz=None` is, from the perspective of this rule, the same antipattern as failing to pass a `tz=` argument at all.

---

_@AlexWaygood reviewed on 2024-03-26 12:15_

Looks good! Could you also change the error message? It's currently

> "The use of `datetime.datetime.now()` without `tz` argument is not allowed"

It should be

> "Using `datetime.datetime.now()` without specifying a timezone is not allowed"

---

_@AlexWaygood approved on 2024-03-26 12:36_

Thanks!

---

_Renamed from "[`DTZ`] Update call_datetime_now_without_tzinfo.rs `DTZ005`" to "[`DTZ`] Improve docs for `call-datetime-without-tzinfo` (`DTZ005`)" by @AlexWaygood on 2024-03-26 12:37_

---

_Comment by @cclauss on 2024-03-26 12:46_

We have lost this helpful hint from the original commit...
> For a timezone-aware datetime for the machine's timezone, use:
> `datetime.datetime.now(tz=datetime.timezone.utc).astimezone()`

---

_Comment by @AlexWaygood on 2024-03-26 13:02_

> We have lost this helpful hint from the original commit...
> 
> > For a timezone-aware datetime for the machine's timezone, use:
> > `datetime.datetime.now(tz=datetime.timezone.utc).astimezone()`

Right. I think that could be worked into the docs somewhere; I agree that it's useful.

I'm also wondering if we could tailor the error message to the specific thing that the user is doing wrong. Something like this?

```diff
--- a/crates/ruff_linter/src/rules/flake8_datetimez/rules/call_datetime_now_without_tzinfo.rs
+++ b/crates/ruff_linter/src/rules/flake8_datetimez/rules/call_datetime_now_without_tzinfo.rs
@@ -1,3 +1,5 @@
+use std::fmt;
+
 use ruff_diagnostics::{Diagnostic, Violation};
 use ruff_macros::{derive_message_formats, violation};
 
@@ -6,7 +8,6 @@ use ruff_python_semantic::Modules;
 use ruff_text_size::Ranged;
 
 use crate::checkers::ast::Checker;
-use crate::rules::flake8_datetimez::rules::helpers::has_non_none_keyword;
 
 use super::helpers;
 
@@ -48,12 +49,28 @@ use super::helpers;
 /// ## References
 /// - [Python documentation: Aware and Naive Objects](https://docs.python.org/3/library/datetime.html#aware-and-naive-objects)
 #[violation]
-pub struct CallDatetimeNowWithoutTzinfo;
+pub struct CallDatetimeNowWithoutTzinfo(DatetimeNowAntipattern);
 
 impl Violation for CallDatetimeNowWithoutTzinfo {
     #[derive_message_formats]
     fn message(&self) -> String {
-        format!("Using `datetime.datetime.now()` without specifying a timezone is not allowed")
+        let CallDatetimeNowWithoutTzinfo(antipattern) = self;
+        format!("{antipattern} is not allowed")
+    }
+}
+
+#[derive(Debug, Clone, Copy, Eq, PartialEq)]
+enum DatetimeNowAntipattern {
+    NoTzArgumentPassed,
+    NonePassedToTzArgument,
+}
+
+impl fmt::Display for DatetimeNowAntipattern {
+    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
+        f.write_str(match self {
+            Self::NoTzArgumentPassed => "Using `datetime.datetime.now()` without a `tz=` argument",
+            Self::NonePassedToTzArgument => "Passing `tz=None` to `datetime.datetime.now()`",
+        })
     }
 }
 
@@ -78,9 +95,10 @@ pub(crate) fn call_datetime_now_without_tzinfo(checker: &mut Checker, call: &ast
 
     // no args / no args unqualified
     if call.arguments.args.is_empty() && call.arguments.keywords.is_empty() {
-        checker
-            .diagnostics
-            .push(Diagnostic::new(CallDatetimeNowWithoutTzinfo, call.range()));
+        checker.diagnostics.push(Diagnostic::new(
+            CallDatetimeNowWithoutTzinfo(DatetimeNowAntipattern::NoTzArgumentPassed),
+            call.range(),
+        ));
         return;
     }
 
@@ -91,16 +109,24 @@ pub(crate) fn call_datetime_now_without_tzinfo(checker: &mut Checker, call: &ast
         .first()
         .is_some_and(Expr::is_none_literal_expr)
     {
-        checker
-            .diagnostics
-            .push(Diagnostic::new(CallDatetimeNowWithoutTzinfo, call.range()));
+        checker.diagnostics.push(Diagnostic::new(
+            CallDatetimeNowWithoutTzinfo(DatetimeNowAntipattern::NonePassedToTzArgument),
+            call.range(),
+        ));
         return;
     }
 
-    // wrong keywords / none keyword
-    if !call.arguments.keywords.is_empty() && !has_non_none_keyword(&call.arguments, "tz") {
-        checker
-            .diagnostics
-            .push(Diagnostic::new(CallDatetimeNowWithoutTzinfo, call.range()));
+    if let Some(keyword_argument) = call.arguments.find_keyword("tz") {
+        if keyword_argument.value.is_none_literal_expr() {
+            checker.diagnostics.push(Diagnostic::new(
+                CallDatetimeNowWithoutTzinfo(DatetimeNowAntipattern::NonePassedToTzArgument),
+                call.range(),
+            ));
+        }
+    } else {
+        checker.diagnostics.push(Diagnostic::new(
+            CallDatetimeNowWithoutTzinfo(DatetimeNowAntipattern::NoTzArgumentPassed),
+            call.range(),
+        ));
     }
 }
diff --git a/crates/ruff_linter/src/rules/flake8_datetimez/snapshots/ruff_linter__rules__flake8_datetimez__tests__DTZ005_DTZ005.py.snap b/crates/ruff_linter/src/rules/flake8_datetimez/snapshots/ruff_linter__rules__flake8_datetimez__tests__DTZ005_DTZ005.py.snap
index 2f24cc4dc..d79264aad 100644
--- a/crates/ruff_linter/src/rules/flake8_datetimez/snapshots/ruff_linter__rules__flake8_datetimez__tests__DTZ005_DTZ005.py.snap
+++ b/crates/ruff_linter/src/rules/flake8_datetimez/snapshots/ruff_linter__rules__flake8_datetimez__tests__DTZ005_DTZ005.py.snap
@@ -1,7 +1,7 @@
 ---
 source: crates/ruff_linter/src/rules/flake8_datetimez/mod.rs
 ---
-DTZ005.py:4:1: DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
+DTZ005.py:4:1: DTZ005 Using `datetime.datetime.now()` without a `tz=` argument is not allowed
   |
 3 | # no args
 4 | datetime.datetime.now()
@@ -10,7 +10,7 @@ DTZ005.py:4:1: DTZ005 Using `datetime.datetime.now()` without specifying a timez
 6 | # wrong keywords
   |
 
-DTZ005.py:7:1: DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
+DTZ005.py:7:1: DTZ005 Using `datetime.datetime.now()` without a `tz=` argument is not allowed
   |
 6 | # wrong keywords
 7 | datetime.datetime.now(bad=datetime.timezone.utc)
@@ -19,7 +19,7 @@ DTZ005.py:7:1: DTZ005 Using `datetime.datetime.now()` without specifying a timez
 9 | # none args
   |
 
-DTZ005.py:10:1: DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
+DTZ005.py:10:1: DTZ005 Passing `tz=None` to `datetime.datetime.now()` is not allowed
    |
  9 | # none args
 10 | datetime.datetime.now(None)
@@ -28,7 +28,7 @@ DTZ005.py:10:1: DTZ005 Using `datetime.datetime.now()` without specifying a time
 12 | # none keywords
    |
 
-DTZ005.py:13:1: DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
+DTZ005.py:13:1: DTZ005 Passing `tz=None` to `datetime.datetime.now()` is not allowed
    |
 12 | # none keywords
 13 | datetime.datetime.now(tz=None)
@@ -37,7 +37,7 @@ DTZ005.py:13:1: DTZ005 Using `datetime.datetime.now()` without specifying a time
 15 | from datetime import datetime
    |
 
-DTZ005.py:18:1: DTZ005 Using `datetime.datetime.now()` without specifying a timezone is not allowed
+DTZ005.py:18:1: DTZ005 Using `datetime.datetime.now()` without a `tz=` argument is not allowed
    |
 17 | # no args unqualified
 18 | datetime.now()
```

(I'm happy to push that straight to your branch if you like it, since I've already tried it out locally!)

---

_Comment by @charliermarsh on 2024-03-26 13:18_

I'm gonna defer to Alex on the actual copy, but I'll note that similar changes may need to be applied to the other DTZ rules which enforce the ~same thing.

---

_Closed by @AlexWaygood on 2024-03-27 19:42_

---

_Branch deleted on 2024-03-27 20:03_

---
