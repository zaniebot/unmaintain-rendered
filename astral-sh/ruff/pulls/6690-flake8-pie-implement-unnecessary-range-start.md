```yaml
number: 6690
title: "[`flake8-pie`] Implement `unnecessary-range-start` (`PIE808`) "
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: PIE808
created_at: 2023-08-19T05:47:57Z
updated_at: 2023-08-19T23:36:17Z
url: https://github.com/astral-sh/ruff/pull/6690
synced_at: 2026-01-12T15:55:22Z
```

# [`flake8-pie`] Implement `unnecessary-range-start` (`PIE808`) 

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Close #6662

## Test Plan

<!-- How was it tested? -->

Unit tests

---

_Comment by @github-actions[bot] on 2023-08-19 06:23_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+79, -0, 0 error(s))

<details><summary>airflow (+28, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/airflow/providers/google/cloud/hooks/mlengine.py#L59'>airflow/providers/google/cloud/hooks/mlengine.py:59:20:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/airflow/www/utils.py#L346'>airflow/www/utils.py:346:28:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/api_connexion/endpoints/test_mapped_task_instance_endpoint.py#L332'>tests/api_connexion/endpoints/test_mapped_task_instance_endpoint.py:332:27:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/api_connexion/endpoints/test_mapped_task_instance_endpoint.py#L356'>tests/api_connexion/endpoints/test_mapped_task_instance_endpoint.py:356:27:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/cli/commands/test_user_command.py#L209'>tests/cli/commands/test_user_command.py:209:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/cli/commands/test_user_command.py#L231'>tests/cli/commands/test_user_command.py:231:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/jobs/test_scheduler_job.py#L1538'>tests/jobs/test_scheduler_job.py:1538:28:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/jobs/test_scheduler_job.py#L1590'>tests/jobs/test_scheduler_job.py:1590:28:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/models/test_dag.py#L342'>tests/models/test_dag.py:342:96:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/models/test_dag.py#L343'>tests/models/test_dag.py:343:32:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/models/test_dag.py#L375'>tests/models/test_dag.py:375:36:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/models/test_dag.py#L377'>tests/models/test_dag.py:377:32:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/models/test_dag.py#L408'>tests/models/test_dag.py:408:36:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/models/test_dag.py#L410'>tests/models/test_dag.py:410:32:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/models/test_dag.py#L869'>tests/models/test_dag.py:869:102:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/providers/apache/hive/hooks/test_hive.py#L294'>tests/providers/apache/hive/hooks/test_hive.py:294:54:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/providers/google/cloud/transfers/test_sql_to_gcs.py#L143'>tests/providers/google/cloud/transfers/test_sql_to_gcs.py:143:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/providers/microsoft/azure/hooks/test_asb.py#L33'>tests/providers/microsoft/azure/hooks/test_asb.py:33:55:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/providers/microsoft/azure/operators/test_asb.py#L43'>tests/providers/microsoft/azure/operators/test_asb.py:43:55:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/sensors/test_base.py#L553'>tests/sensors/test_base.py:553:28:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/sensors/test_external_task_sensor.py#L1417'>tests/sensors/test_external_task_sensor.py:1417:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/sensors/test_external_task_sensor.py#L1500'>tests/sensors/test_external_task_sensor.py:1500:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/system/providers/cncf/kubernetes/example_kubernetes_decorator.py#L50'>tests/system/providers/cncf/kubernetes/example_kubernetes_decorator.py:50:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/system/providers/cncf/kubernetes/example_kubernetes_decorator.py#L53'>tests/system/providers/cncf/kubernetes/example_kubernetes_decorator.py:53:28:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/system/providers/microsoft/azure/example_azure_service_bus.py#L48'>tests/system/providers/microsoft/azure/example_azure_service_bus.py:48:55:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/system/providers/snowflake/example_snowflake.py#L37'>tests/system/providers/snowflake/example_snowflake.py:37:61:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/test_utils/perf/perf_kit/repeat_and_time.py#L120'>tests/test_utils/perf/perf_kit/repeat_and_time.py:120:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/apache/airflow/blob/bcefe6109bcabd9bd6daf8b44f7352adda1ed53d/tests/www/test_utils.py#L80'>tests/www/test_utils.py:80:36:</a> PIE808 [*] Unnecessary `start` argument in `range`
</pre>

</p>
</details>
<details><summary>bokeh (+16, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/docs/bokeh/source/docs/first_steps/examples/first_steps_3_box_annotation.py#L7'>docs/bokeh/source/docs/first_steps/examples/first_steps_3_box_annotation.py:7:16:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/docs/bokeh/source/docs/first_steps/examples/first_steps_3_box_annotation.py#L8'>docs/bokeh/source/docs/first_steps/examples/first_steps_3_box_annotation.py:8:25:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py#L11'>docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py:11:25:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py#L8'>docs/bokeh/source/docs/first_steps/examples/first_steps_4_datetime_axis.py:8:65:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/docs/bokeh/source/docs/first_steps/examples/first_steps_5_vectorize_color.py#L6'>docs/bokeh/source/docs/first_steps/examples/first_steps_5_vectorize_color.py:6:16:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/docs/bokeh/source/docs/first_steps/examples/first_steps_5_vectorize_color.py#L7'>docs/bokeh/source/docs/first_steps/examples/first_steps_5_vectorize_color.py:7:25:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/examples/integration/layout/multi_plot_layout.py#L21'>examples/integration/layout/multi_plot_layout.py:21:18:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/examples/integration/layout/multi_plot_layout.py#L22'>examples/integration/layout/multi_plot_layout.py:22:22:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/examples/interaction/js_callbacks/customjs_for_widgets.py#L5'>examples/interaction/js_callbacks/customjs_for_widgets.py:5:29:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/examples/interaction/js_callbacks/js_on_change.py#L5'>examples/interaction/js_callbacks/js_on_change.py:5:29:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/examples/reference/models/HSpan.py#L9'>examples/reference/models/HSpan.py:9:32:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/examples/reference/models/VSpan.py#L9'>examples/reference/models/VSpan.py:9:32:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/examples/server/app/hold_app.py#L17'>examples/server/app/hold_app.py:17:28:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/src/bokeh/embed/util.py#L257'>src/bokeh/embed/util.py:257:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/src/bokeh/util/serialization.py#L294'>src/bokeh/util/serialization.py:294:21:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/bokeh/bokeh/blob/962a34c27fdae5dbef0a76984edd048c3d458332/tests/unit/bokeh/core/test_serialization.py#L127'>tests/unit/bokeh/core/test_serialization.py:127:34:</a> PIE808 [*] Unnecessary `start` argument in `range`
</pre>

</p>
</details>
<details><summary>openpilot (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/commaai/openpilot/blob/012060ba328da69e7fd62531bd65e8d064b3f6c0/selfdrive/car/__init__.py#L40'>selfdrive/car/__init__.py:40:32:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/commaai/openpilot/blob/012060ba328da69e7fd62531bd65e8d064b3f6c0/selfdrive/controls/lib/longitudinal_mpc_lib/long_mpc.py#L293'>selfdrive/controls/lib/longitudinal_mpc_lib/long_mpc.py:293:22:</a> PIE808 [*] Unnecessary `start` argument in `range`
</pre>

</p>
</details>
<details><summary>securedrop (+3, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/7eb784e3355de3400336adadce4915273e004671/admin/securedrop_admin/__init__.py#L110'>admin/securedrop_admin/__init__.py:110:72:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/freedomofpress/securedrop/blob/7eb784e3355de3400336adadce4915273e004671/securedrop/tests/test_remove_pending_sources.py#L19'>securedrop/tests/test_remove_pending_sources.py:19:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/freedomofpress/securedrop/blob/7eb784e3355de3400336adadce4915273e004671/securedrop/tests/test_remove_pending_sources.py#L56'>securedrop/tests/test_remove_pending_sources.py:56:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
</pre>

</p>
</details>
<details><summary>ibis (+7, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/2cb1a55be576163c6fa39c7be2000462c6a8cbdd/ibis/backends/dask/tests/execution/test_window.py#L492'>ibis/backends/dask/tests/execution/test_window.py:492:53:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/ibis-project/ibis/blob/2cb1a55be576163c6fa39c7be2000462c6a8cbdd/ibis/backends/pandas/tests/execution/test_window.py#L439'>ibis/backends/pandas/tests/execution/test_window.py:439:53:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/ibis-project/ibis/blob/2cb1a55be576163c6fa39c7be2000462c6a8cbdd/ibis/backends/pyspark/tests/test_basic.py#L22'>ibis/backends/pyspark/tests/test_basic.py:22:42:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/ibis-project/ibis/blob/2cb1a55be576163c6fa39c7be2000462c6a8cbdd/ibis/backends/pyspark/tests/test_basic.py#L32'>ibis/backends/pyspark/tests/test_basic.py:32:22:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/ibis-project/ibis/blob/2cb1a55be576163c6fa39c7be2000462c6a8cbdd/ibis/backends/pyspark/tests/test_basic.py#L32'>ibis/backends/pyspark/tests/test_basic.py:32:61:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/ibis-project/ibis/blob/2cb1a55be576163c6fa39c7be2000462c6a8cbdd/ibis/backends/pyspark/tests/test_basic.py#L47'>ibis/backends/pyspark/tests/test_basic.py:47:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/ibis-project/ibis/blob/2cb1a55be576163c6fa39c7be2000462c6a8cbdd/ibis/backends/pyspark/tests/test_basic.py#L48'>ibis/backends/pyspark/tests/test_basic.py:48:25:</a> PIE808 [*] Unnecessary `start` argument in `range`
</pre>

</p>
</details>
<details><summary>zulip (+23, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/analytics/views/stats.py#L510'>analytics/views/stats.py:510:28:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/analytics/views/stats.py#L513'>analytics/views/stats.py:513:82:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/scripts/lib/supervisor.py#L12'>scripts/lib/supervisor.py:12:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/lib/upload/base.py#L101'>zerver/lib/upload/base.py:101:28:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_decorators.py#L892'>zerver/tests/test_decorators.py:892:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_drafts.py#L570'>zerver/tests/test_drafts.py:570:73:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_email_mirror.py#L1335'>zerver/tests/test_email_mirror.py:1335:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_email_mirror.py#L1589'>zerver/tests/test_email_mirror.py:1589:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_message_flags.py#L665'>zerver/tests/test_message_flags.py:665:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_message_notification_emails.py#L176'>zerver/tests/test_message_notification_emails.py:176:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_message_notification_emails.py#L390'>zerver/tests/test_message_notification_emails.py:390:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_message_notification_emails.py#L413'>zerver/tests/test_message_notification_emails.py:413:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_message_notification_emails.py#L435'>zerver/tests/test_message_notification_emails.py:435:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_retention.py#L1091'>zerver/tests/test_retention.py:1091:84:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_retention.py#L1097'>zerver/tests/test_retention.py:1097:82:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_retention.py#L1135'>zerver/tests/test_retention.py:1135:92:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_retention.py#L606'>zerver/tests/test_retention.py:606:91:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_retention.py#L618'>zerver/tests/test_retention.py:618:91:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_retention.py#L630'>zerver/tests/test_retention.py:630:83:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_retention.py#L641'>zerver/tests/test_retention.py:641:83:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_retention.py#L662'>zerver/tests/test_retention.py:662:83:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_retention.py#L663'>zerver/tests/test_retention.py:663:83:</a> PIE808 [*] Unnecessary `start` argument in `range`
+ <a href='https://github.com/zulip/zulip/blob/113ac6eb98ffe192580ba0e25a7958346daeebf8/zerver/tests/test_upload.py#L773'>zerver/tests/test_upload.py:773:24:</a> PIE808 [*] Unnecessary `start` argument in `range`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PIE808 | 79 | 79 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.9±0.03ms    10.4 MB/sec    1.05      4.1±0.21ms     9.9 MB/sec
formatter/numpy/ctypeslib.py               1.01    817.6±8.10µs    20.4 MB/sec    1.00    809.3±7.62µs    20.6 MB/sec
formatter/numpy/globals.py                 1.00     85.5±2.49µs    34.5 MB/sec    1.06     91.0±6.04µs    32.4 MB/sec
formatter/pydantic/types.py                1.00   1591.1±9.04µs    16.0 MB/sec    1.06  1690.4±90.34µs    15.1 MB/sec
linter/all-rules/large/dataset.py          1.01     12.6±0.61ms     3.2 MB/sec    1.00     12.4±0.12ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.03ms     5.1 MB/sec    1.01      3.3±0.02ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    461.9±3.75µs     6.4 MB/sec    1.01    467.1±1.88µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.06ms     4.0 MB/sec    1.01      6.3±0.05ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.05ms     6.3 MB/sec    1.00      6.5±0.03ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1429.2±11.41µs    11.7 MB/sec    1.00   1425.6±7.82µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.05    177.3±5.35µs    16.6 MB/sec    1.00    168.3±1.73µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.1±0.08ms     8.3 MB/sec    1.00      2.9±0.01ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.8±0.07ms    10.7 MB/sec    1.00      3.8±0.07ms    10.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   769.3±14.26µs    21.6 MB/sec    1.00   769.9±13.93µs    21.6 MB/sec
formatter/numpy/globals.py                 1.00     79.5±2.83µs    37.1 MB/sec    1.01     80.5±2.44µs    36.6 MB/sec
formatter/pydantic/types.py                1.00  1541.0±23.99µs    16.5 MB/sec    1.01  1560.3±23.36µs    16.3 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.11ms     3.2 MB/sec    1.03     13.0±0.15ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.04ms     4.8 MB/sec    1.02      3.5±0.05ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    438.6±6.25µs     6.7 MB/sec    1.02    448.8±9.69µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.10ms     3.8 MB/sec    1.01      6.7±0.09ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.08ms     5.7 MB/sec    1.01      7.2±0.08ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1506.0±25.17µs    11.1 MB/sec    1.01  1525.9±17.94µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.7±3.66µs    17.0 MB/sec    1.05    181.6±8.04µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.03ms     8.0 MB/sec    1.01      3.2±0.05ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "[flake8-pie] Implement `unnecessary-range-start` (`PIE808`) " to "[`flake8-pie`] Implement `unnecessary-range-start` (`PIE808`) " by @harupy on 2023-08-19 06:29_

---

_@tjkuson reviewed on 2023-08-19 19:35_

---

_Review comment by @tjkuson on `crates/ruff/src/rules/flake8_pie/rules/unnecessary_range_start.rs`:28 on 2023-08-19 19:35_

```suggestion
/// ```
///
/// ## References
/// - [Python documentation: `range`](https://docs.python.org/3/library/stdtypes.html#range)
```

Think it could be worth linking to the documentation, so people can read how `range` is defined.

---

_@charliermarsh approved on 2023-08-19 21:51_

---

_Merged by @charliermarsh on 2023-08-19 21:59_

---

_Closed by @charliermarsh on 2023-08-19 21:59_

---

_Branch deleted on 2023-08-19 23:36_

---
