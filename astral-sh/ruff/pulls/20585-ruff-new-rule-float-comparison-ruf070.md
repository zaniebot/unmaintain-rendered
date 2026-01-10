```yaml
number: 20585
title: "[`ruff`] New rule float-comparison (`RUF070`)"
type: pull_request
state: open
author: chirizxc
labels:
  - rule
  - preview
assignees: []
base: main
head: sq-S1244
created_at: 2025-09-25T21:12:22Z
updated_at: 2026-01-09T17:36:22Z
url: https://github.com/astral-sh/ruff/pull/20585
synced_at: 2026-01-10T15:56:07Z
```

# [`ruff`] New rule float-comparison (`RUF070`)

---

_Pull request opened by @chirizxc on 2025-09-25 21:12_

## Summary

Part of https://github.com/astral-sh/ruff/issues/14220

## Test Plan

`cargo nextest run ruf067`


---

_Comment by @chirizxc on 2025-09-25 21:14_

We need more tests and cases where the rule can give FP and so on

---

_Comment by @chirizxc on 2025-09-25 21:16_

Also, I randomly picked CODE for this rule

---

_Comment by @ntBre on 2025-09-25 21:26_

I haven't reviewed, but if there's not a corresponding E723 rule in pycodestyle, I would default to making this a ruff rule. We have a PR in progress for RUF066, so RUF067 would be the next free code.

---

_Label `rule` added by @ntBre on 2025-09-25 21:26_

---

_Label `preview` added by @ntBre on 2025-09-25 21:26_

---

_Comment by @github-actions[bot] on 2025-09-25 21:26_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1008 -0 violations, +0 -0 fixes in 16 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+35 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/.github/tests/test_mr_publish_results.py#L63'>.github/tests/test_mr_publish_results.py:63:12:</a> RUF070 Unreliable floating point equality comparison `87.0 == transform_to_seconds("1m27s")`
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/.github/tests/test_mr_publish_results.py#L64'>.github/tests/test_mr_publish_results.py:64:12:</a> RUF070 Unreliable floating point equality comparison `87.3 == transform_to_seconds("1m27.3s")`
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/.github/tests/test_mr_publish_results.py#L65'>.github/tests/test_mr_publish_results.py:65:12:</a> RUF070 Unreliable floating point equality comparison `27.0 == transform_to_seconds("27s")`
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/.github/tests/test_mr_publish_results.py#L66'>.github/tests/test_mr_publish_results.py:66:12:</a> RUF070 Unreliable floating point equality comparison `3627.0 == transform_to_seconds("1h27s")`
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/.github/tests/test_mr_publish_results.py#L67'>.github/tests/test_mr_publish_results.py:67:12:</a> RUF070 Unreliable floating point equality comparison `3687.0 == transform_to_seconds("1h1m27s")`
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/policies/ensemble.py#L57'>rasa/core/policies/ensemble.py:57:32:</a> RUF070 Unreliable floating point equality comparison `max_confidence == 0.0`
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/engine/caching.py#L301'>rasa/engine/caching.py:301:16:</a> RUF070 Unreliable floating point equality comparison `self._max_cache_size == 0.0`
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/slots.py#L257'>rasa/shared/core/slots.py:257:16:</a> RUF070 Unreliable floating point equality comparison `x == 1.0`
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/shared/core/slots.py#L260'>rasa/shared/core/slots.py:260:20:</a> RUF070 Unreliable floating point equality comparison `float(x) == 1.0`
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/utils/tensorflow/layers.py#L291'>rasa/utils/tensorflow/layers.py:291:12:</a> RUF070 Unreliable floating point equality comparison `self.density == 1.0`
... 25 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+29 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3f0885e79244a69aca25b9be97eb6bbc2e92443b/airflow-core/src/airflow/models/taskinstance.py#L943'>airflow-core/src/airflow/models/taskinstance.py:943:12:</a> RUF070 Unreliable floating point equality comparison `multiplier != 1.0`
+ <a href='https://github.com/apache/airflow/blob/3f0885e79244a69aca25b9be97eb6bbc2e92443b/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1425'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1425:16:</a> RUF070 Unreliable floating point equality comparison `ti.duration == 3600.00`
+ <a href='https://github.com/apache/airflow/blob/3f0885e79244a69aca25b9be97eb6bbc2e92443b/airflow-core/tests/unit/cli/commands/test_variable_command.py#L205'>airflow-core/tests/unit/cli/commands/test_variable_command.py:205:16:</a> RUF070 Unreliable floating point equality comparison `Variable.get("float", deserialize_json=True) == 42.0`
+ <a href='https://github.com/apache/airflow/blob/3f0885e79244a69aca25b9be97eb6bbc2e92443b/airflow-core/tests/unit/core/test_configuration.py#L376'>airflow-core/tests/unit/core/test_configuration.py:376:16:</a> RUF070 Unreliable floating point equality comparison `test_conf.getfloat("another", "key8_float", fallback="1") == 1.0`
+ <a href='https://github.com/apache/airflow/blob/3f0885e79244a69aca25b9be97eb6bbc2e92443b/airflow-core/tests/unit/jobs/test_base_job.py#L113'>airflow-core/tests/unit/jobs/test_base_job.py:113:20:</a> RUF070 Unreliable floating point equality comparison `most_recent.heartrate == float(job_heartbeat_sec)`
+ <a href='https://github.com/apache/airflow/blob/3f0885e79244a69aca25b9be97eb6bbc2e92443b/airflow-core/tests/unit/jobs/test_base_job.py#L298'>airflow-core/tests/unit/jobs/test_base_job.py:298:16:</a> RUF070 Unreliable floating point equality comparison `health_check_threshold("UnknownJob", 30) == 30 * 2.1`
+ <a href='https://github.com/apache/airflow/blob/3f0885e79244a69aca25b9be97eb6bbc2e92443b/airflow-core/tests/unit/jobs/test_scheduler_job.py#L6700'>airflow-core/tests/unit/jobs/test_scheduler_job.py:6700:24:</a> RUF070 Unreliable floating point equality comparison `duration == 0.0`
+ <a href='https://github.com/apache/airflow/blob/3f0885e79244a69aca25b9be97eb6bbc2e92443b/airflow-core/tests/unit/jobs/test_scheduler_job.py#L6726'>airflow-core/tests/unit/jobs/test_scheduler_job.py:6726:24:</a> RUF070 Unreliable floating point equality comparison `duration == 0.0`
+ <a href='https://github.com/apache/airflow/blob/3f0885e79244a69aca25b9be97eb6bbc2e92443b/airflow-core/tests/unit/lineage/test_hook.py#L719'>airflow-core/tests/unit/lineage/test_hook.py:719:16:</a> RUF070 Unreliable floating point equality comparison `lineage.extra[2].value == 3.14`
+ <a href='https://github.com/apache/airflow/blob/3f0885e79244a69aca25b9be97eb6bbc2e92443b/airflow-core/tests/unit/models/test_pool.py#L205'>airflow-core/tests/unit/models/test_pool.py:205:16:</a> RUF070 Unreliable floating point equality comparison `float("inf") == pool.open_slots()`
... 19 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+27 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/ea90d1f14169f9d48f059e95fab21ca42946e160/tests/integration_tests/utils_tests.py#L406'>tests/integration_tests/utils_tests.py:406:16:</a> RUF070 Unreliable floating point equality comparison `cast_to_num("5.2") == 5.2`
+ <a href='https://github.com/apache/superset/blob/ea90d1f14169f9d48f059e95fab21ca42946e160/tests/integration_tests/utils_tests.py#L408'>tests/integration_tests/utils_tests.py:408:16:</a> RUF070 Unreliable floating point equality comparison `cast_to_num(10.1) == 10.1`
+ <a href='https://github.com/apache/superset/blob/ea90d1f14169f9d48f059e95fab21ca42946e160/tests/unit_tests/commands/databases/csv_reader_test.py#L853'>tests/unit_tests/commands/databases/csv_reader_test.py:853:12:</a> RUF070 Unreliable floating point equality comparison `df.iloc[0]["Score"] == 95.5`
+ <a href='https://github.com/apache/superset/blob/ea90d1f14169f9d48f059e95fab21ca42946e160/tests/unit_tests/commands/databases/csv_reader_test.py#L950'>tests/unit_tests/commands/databases/csv_reader_test.py:950:12:</a> RUF070 Unreliable floating point equality comparison `result_df.iloc[0]["score"] == 95.5`
+ <a href='https://github.com/apache/superset/blob/ea90d1f14169f9d48f059e95fab21ca42946e160/tests/unit_tests/commands/report/alert_test.py#L226'>tests/unit_tests/commands/report/alert_test.py:226:12:</a> RUF070 Unreliable floating point equality comparison `command._result == 0.0`
+ <a href='https://github.com/apache/superset/blob/ea90d1f14169f9d48f059e95fab21ca42946e160/tests/unit_tests/commands/report/alert_test.py#L229'>tests/unit_tests/commands/report/alert_test.py:229:12:</a> RUF070 Unreliable floating point equality comparison `report_schedule_mock.last_value == 0.0`
+ <a href='https://github.com/apache/superset/blob/ea90d1f14169f9d48f059e95fab21ca42946e160/tests/unit_tests/db_engine_specs/test_trino.py#L1163'>tests/unit_tests/db_engine_specs/test_trino.py:1163:12:</a> RUF070 Unreliable floating point equality comparison `query.progress == 0.0`
+ <a href='https://github.com/apache/superset/blob/ea90d1f14169f9d48f059e95fab21ca42946e160/tests/unit_tests/db_engine_specs/test_trino.py#L971'>tests/unit_tests/db_engine_specs/test_trino.py:971:12:</a> RUF070 Unreliable floating point equality comparison `query.progress == 100.0`
+ <a href='https://github.com/apache/superset/blob/ea90d1f14169f9d48f059e95fab21ca42946e160/tests/unit_tests/extensions/ssh_test.py#L35'>tests/unit_tests/extensions/ssh_test.py:35:12:</a> RUF070 Unreliable floating point equality comparison `sshtunnel.TUNNEL_TIMEOUT == 123.0`
+ <a href='https://github.com/apache/superset/blob/ea90d1f14169f9d48f059e95fab21ca42946e160/tests/unit_tests/extensions/ssh_test.py#L36'>tests/unit_tests/extensions/ssh_test.py:36:12:</a> RUF070 Unreliable floating point equality comparison `sshtunnel.SSH_TIMEOUT == 321.0`
... 17 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+199 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/slope_graph.py#L29'>examples/topics/categorical/slope_graph.py:29:10:</a> RUF070 Unreliable floating point equality comparison `df["year"] == 2000.0`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/slope_graph.py#L29'>examples/topics/categorical/slope_graph.py:29:35:</a> RUF070 Unreliable floating point equality comparison `df["year"] == 2010.0`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L321'>src/bokeh/colors/color.py:321:12:</a> RUF070 Unreliable floating point equality comparison `self.a == 1.0`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L460'>src/bokeh/colors/color.py:460:12:</a> RUF070 Unreliable floating point equality comparison `self.a == 1.0`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/tools/test_box_zoom_tool.py#L106'>tests/integration/tools/test_box_zoom_tool.py:106:16:</a> RUF070 Unreliable floating point equality comparison `(results['xrstart'] + results['xrend'])/2.0 == pytest.approx(0.25)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/tools/test_box_zoom_tool.py#L107'>tests/integration/tools/test_box_zoom_tool.py:107:16:</a> RUF070 Unreliable floating point equality comparison `(results['yrstart'] + results['yrend'])/2.0 == pytest.approx(0.75)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/tools/test_range_tool.py#L103'>tests/integration/tools/test_range_tool.py:103:16:</a> RUF070 Unreliable floating point equality comparison `results['start'] == 0.4`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/tools/test_range_tool.py#L104'>tests/integration/tools/test_range_tool.py:104:16:</a> RUF070 Unreliable floating point equality comparison `results['end'] == 0.6`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/tools/test_range_tool.py#L118'>tests/integration/tools/test_range_tool.py:118:16:</a> RUF070 Unreliable floating point equality comparison `results['start'] == 0.5`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/tools/test_range_tool.py#L119'>tests/integration/tools/test_range_tool.py:119:16:</a> RUF070 Unreliable floating point equality comparison `results['end'] == 0.7`
... 189 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+30 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/736b59817a5e9be9990f99a6be9a8ddfd78a6fcf/ibis/backends/bigquery/tests/system/test_client.py#L434'>ibis/backends/bigquery/tests/system/test_client.py:434:48:</a> RUF070 Unreliable floating point equality comparison `t.max != 9999.9`
+ <a href='https://github.com/ibis-project/ibis/blob/736b59817a5e9be9990f99a6be9a8ddfd78a6fcf/ibis/backends/bigquery/tests/system/udf/test_udf_execute.py#L102'>ibis/backends/bigquery/tests/system/udf/test_udf_execute.py:102:12:</a> RUF070 Unreliable floating point equality comparison `con.execute(expr) == 8.0`
+ <a href='https://github.com/ibis-project/ibis/blob/736b59817a5e9be9990f99a6be9a8ddfd78a6fcf/ibis/backends/clickhouse/tests/test_operators.py#L175'>ibis/backends/clickhouse/tests/test_operators.py:175:12:</a> RUF070 Unreliable floating point equality comparison `round(con.execute(expr), 3) == -5.245`
+ <a href='https://github.com/ibis-project/ibis/blob/736b59817a5e9be9990f99a6be9a8ddfd78a6fcf/ibis/backends/druid/tests/conftest.py#L58'>ibis/backends/druid/tests/conftest.py:58:31:</a> RUF070 Unreliable floating point equality comparison `js[datasource] == 100.0`
+ <a href='https://github.com/ibis-project/ibis/blob/736b59817a5e9be9990f99a6be9a8ddfd78a6fcf/ibis/backends/duckdb/tests/test_udf.py#L180'>ibis/backends/duckdb/tests/test_udf.py:180:12:</a> RUF070 Unreliable floating point equality comparison `result.iat[0] == 1.0`
+ <a href='https://github.com/ibis-project/ibis/blob/736b59817a5e9be9990f99a6be9a8ddfd78a6fcf/ibis/backends/postgres/tests/test_client.py#L515'>ibis/backends/postgres/tests/test_client.py:515:12:</a> RUF070 Unreliable floating point equality comparison `value[0].as_py() == 1.0`
+ <a href='https://github.com/ibis-project/ibis/blob/736b59817a5e9be9990f99a6be9a8ddfd78a6fcf/ibis/backends/sqlite/tests/test_client.py#L55'>ibis/backends/sqlite/tests/test_client.py:55:12:</a> RUF070 Unreliable floating point equality comparison `result == 0.0`
+ <a href='https://github.com/ibis-project/ibis/blob/736b59817a5e9be9990f99a6be9a8ddfd78a6fcf/ibis/backends/tests/test_map.py#L504'>ibis/backends/tests/test_map.py:504:12:</a> RUF070 Unreliable floating point equality comparison `con.execute(expr) == 3.0`
+ <a href='https://github.com/ibis-project/ibis/blob/736b59817a5e9be9990f99a6be9a8ddfd78a6fcf/ibis/backends/tests/test_numeric.py#L1126'>ibis/backends/tests/test_numeric.py:1126:12:</a> RUF070 Unreliable floating point equality comparison `result == 0.5`
+ <a href='https://github.com/ibis-project/ibis/blob/736b59817a5e9be9990f99a6be9a8ddfd78a6fcf/ibis/common/tests/test_annotations.py#L371'>ibis/common/tests/test_annotations.py:371:12:</a> RUF070 Unreliable floating point equality comparison `test(2, 3.0) == 6.0`
... 20 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+71 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/d972d00b3a4c2ffef2a616e7473085c7a279caaf/libs/core/tests/unit_tests/language_models/chat_models/test_base.py#L1207'>libs/core/tests/unit_tests/language_models/chat_models/test_base.py:1207:12:</a> RUF070 Unreliable floating point equality comparison `ls_params["ls_temperature"] == 0.2`
+ <a href='https://github.com/langchain-ai/langchain/blob/d972d00b3a4c2ffef2a616e7473085c7a279caaf/libs/core/tests/unit_tests/language_models/llms/test_base.py#L273'>libs/core/tests/unit_tests/language_models/llms/test_base.py:273:12:</a> RUF070 Unreliable floating point equality comparison `ls_params["ls_temperature"] == 0.2`
+ <a href='https://github.com/langchain-ai/langchain/blob/d972d00b3a4c2ffef2a616e7473085c7a279caaf/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L1149'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:1149:12:</a> RUF070 Unreliable floating point equality comparison `result_v2[0].coordinates.latitude == 48.8584`
+ <a href='https://github.com/langchain-ai/langchain/blob/d972d00b3a4c2ffef2a616e7473085c7a279caaf/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L1150'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:1150:12:</a> RUF070 Unreliable floating point equality comparison `result_v2[0].coordinates.longitude == 2.2945`
+ <a href='https://github.com/langchain-ai/langchain/blob/d972d00b3a4c2ffef2a616e7473085c7a279caaf/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L1189'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:1189:12:</a> RUF070 Unreliable floating point equality comparison `result_mixed[1].coordinates.latitude == 37.8199`
+ <a href='https://github.com/langchain-ai/langchain/blob/d972d00b3a4c2ffef2a616e7473085c7a279caaf/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L1245'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:1245:12:</a> RUF070 Unreliable floating point equality comparison `result_v1_full[0].price == 999.99`
+ <a href='https://github.com/langchain-ai/langchain/blob/d972d00b3a4c2ffef2a616e7473085c7a279caaf/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L1267'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:1267:12:</a> RUF070 Unreliable floating point equality comparison `result_v1_minimal[0].price == 29.99`
+ <a href='https://github.com/langchain-ai/langchain/blob/d972d00b3a4c2ffef2a616e7473085c7a279caaf/libs/core/tests/unit_tests/rate_limiters/test_in_memory_rate_limiter.py#L104'>libs/core/tests/unit_tests/rate_limiters/test_in_memory_rate_limiter.py:104:16:</a> RUF070 Unreliable floating point equality comparison `rate_limiter.available_tokens == 199.0`
+ <a href='https://github.com/langchain-ai/langchain/blob/d972d00b3a4c2ffef2a616e7473085c7a279caaf/libs/core/tests/unit_tests/rate_limiters/test_in_memory_rate_limiter.py#L107'>libs/core/tests/unit_tests/rate_limiters/test_in_memory_rate_limiter.py:107:16:</a> RUF070 Unreliable floating point equality comparison `rate_limiter.available_tokens == 499.0`
+ <a href='https://github.com/langchain-ai/langchain/blob/d972d00b3a4c2ffef2a616e7473085c7a279caaf/libs/core/tests/unit_tests/rate_limiters/test_in_memory_rate_limiter.py#L21'>libs/core/tests/unit_tests/rate_limiters/test_in_memory_rate_limiter.py:21:12:</a> RUF070 Unreliable floating point equality comparison `rate_limiter.available_tokens == 0.0`
... 61 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/cb3fd566478502a9579389c40e21b8eafba41bad/tests/unit/test_services_fees.py#L61'>tests/unit/test_services_fees.py:61:12:</a> RUF070 Unreliable floating point equality comparison `fee / 1000 == 199`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/5d377b2850ab27badc92c014e500898d0d29e553/examples/orm_deprecated/bulk_import/example_bulkwriter_with_nullable.py#L300'>examples/orm_deprecated/bulk_import/example_bulkwriter_with_nullable.py:300:16:</a> RUF070 Unreliable floating point equality comparison `len(b[0]) == DIM/8`
+ <a href='https://github.com/milvus-io/pymilvus/blob/5d377b2850ab27badc92c014e500898d0d29e553/pymilvus/orm/iterator.py#L557'>pymilvus/orm/iterator.py:557:12:</a> RUF070 Unreliable floating point equality comparison `self._width == 0.0`
+ <a href='https://github.com/milvus-io/pymilvus/blob/5d377b2850ab27badc92c014e500898d0d29e553/tests/test_search_result.py#L105'>tests/test_search_result.py:105:16:</a> RUF070 Unreliable floating point equality comparison `first_hit["distance"] == 0.`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+110 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/17b66cc0ad8c26292bfee55e019e1ea38ef6177b/pandas/core/indexes/range.py#L963'>pandas/core/indexes/range.py:963:30:</a> RUF070 Unreliable floating point equality comparison `abs(start_s - start_o) == step_s / 2`
+ <a href='https://github.com/pandas-dev/pandas/blob/17b66cc0ad8c26292bfee55e019e1ea38ef6177b/pandas/core/indexes/range.py#L964'>pandas/core/indexes/range.py:964:30:</a> RUF070 Unreliable floating point equality comparison `abs(end_s - end_o) == step_s / 2`
+ <a href='https://github.com/pandas-dev/pandas/blob/17b66cc0ad8c26292bfee55e019e1ea38ef6177b/pandas/core/window/rolling.py#L1818'>pandas/core/window/rolling.py:1818:12:</a> RUF070 Unreliable floating point equality comparison `q == 1.0`
+ <a href='https://github.com/pandas-dev/pandas/blob/17b66cc0ad8c26292bfee55e019e1ea38ef6177b/pandas/core/window/rolling.py#L1820'>pandas/core/window/rolling.py:1820:14:</a> RUF070 Unreliable floating point equality comparison `q == 0.0`
+ <a href='https://github.com/pandas-dev/pandas/blob/17b66cc0ad8c26292bfee55e019e1ea38ef6177b/pandas/tests/arrays/floating/test_function.py#L159'>pandas/tests/arrays/floating/test_function.py:159:16:</a> RUF070 Unreliable floating point equality comparison `result == 6.0`
+ <a href='https://github.com/pandas-dev/pandas/blob/17b66cc0ad8c26292bfee55e019e1ea38ef6177b/pandas/tests/arrays/sparse/test_array.py#L345'>pandas/tests/arrays/sparse/test_array.py:345:16:</a> RUF070 Unreliable floating point equality comparison `arr.density == 0.5`
+ <a href='https://github.com/pandas-dev/pandas/blob/17b66cc0ad8c26292bfee55e019e1ea38ef6177b/pandas/tests/arrays/sparse/test_reductions.py#L118'>pandas/tests/arrays/sparse/test_reductions.py:118:16:</a> RUF070 Unreliable floating point equality comparison `out == 45.0`
+ <a href='https://github.com/pandas-dev/pandas/blob/17b66cc0ad8c26292bfee55e019e1ea38ef6177b/pandas/tests/arrays/sparse/test_reductions.py#L122'>pandas/tests/arrays/sparse/test_reductions.py:122:16:</a> RUF070 Unreliable floating point equality comparison `out == 40.0`
+ <a href='https://github.com/pandas-dev/pandas/blob/17b66cc0ad8c26292bfee55e019e1ea38ef6177b/pandas/tests/arrays/sparse/test_reductions.py#L125'>pandas/tests/arrays/sparse/test_reductions.py:125:16:</a> RUF070 Unreliable floating point equality comparison `out == 40.0`
+ <a href='https://github.com/pandas-dev/pandas/blob/17b66cc0ad8c26292bfee55e019e1ea38ef6177b/pandas/tests/arrays/sparse/test_reductions.py#L152'>pandas/tests/arrays/sparse/test_reductions.py:152:16:</a> RUF070 Unreliable floating point equality comparison `out == 45.0`
... 100 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF070 | 1008 | 1008 | 0 | 0 | 0 |

</p>
</details>





---

_Renamed from "[`pycodestyle`] New rule float-comparison (`E723`)" to "[`ruff`] New rule float-comparison (`RUF067`)" by @chirizxc on 2025-09-25 22:43_

---

_Comment by @chirizxc on 2025-09-25 23:13_

<img width="1150" height="483" alt="изображение" src="https://github.com/user-attachments/assets/643195bf-bfc4-4527-b000-971fd9b1de31" />

+ [examples/topics/categorical/slope_graph.py:29:10:](https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/slope_graph.py#L29) RUF067 Comparison `df["year"] == 2000.0` should be replaced by `math.isclose(df["year"], 2000.0, rel_tol=1e-09, abs_tol=1e-09)`
+ [examples/topics/categorical/slope_graph.py:29:35:](https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/categorical/slope_graph.py#L29) RUF067 Comparison `df["year"] == 2010.0` should be replaced by `math.isclose(df["year"], 2010.0, rel_tol=1e-09, abs_tol=1e-09)`


This breaks the behavior of code and results in:
```
TypeError: cannot convert the series to <class 'float'>
````

---

_Comment by @chirizxc on 2025-09-25 23:16_

What should we do in such a case? Should we somehow mark this fact in the documentation or what should we do, because we will not be able to check in general that such calls will not lead to some code breakages

---

_Comment by @chirizxc on 2025-09-26 18:43_

The rule detects potentially problematic float comparisons correctly, but we should not suggest replacing only `math.isclose()` but also `numpy.isclose()`

---

_Comment by @ntBre on 2025-10-27 18:44_

Would it help with the false positives, and maybe with the numpy cases, to apply the rule only in boolean contexts? I think we have other rules that apply only to expressions used as `if` conditions or in negated `not` expressions, for example. We even have a semantic model helper for it:

https://github.com/astral-sh/ruff/blob/5f136c2e35257e55dfbfd8afc7093aaded63a5b6/crates/ruff_python_semantic/src/model.rs#L1974

This rule is going to require type inference to get exactly right anyway, so this might be a good way to be more conservative in the meantime.

I also only gave the implementation a quick skim and could have overlooked this, but does this rule apply to comparisons like `<=` and `>` too? I would probably expect those to be allowed since something like `delta < 1e-8` should work as expected, I'm assuming that's what `math.isclose` does under the hood. Along those lines, we may want to rename the rule to `float-equality-comparison`, if it only applies to equality.

---

_Comment by @chirizxc on 2025-10-29 08:54_

> Would it help with the false positives, and maybe with the numpy cases, to apply the rule only in boolean contexts? I think we have other rules that apply only to expressions used as `if` conditions or in negated `not` expressions, for example. We even have a semantic model helper for it:
> 
> https://github.com/astral-sh/ruff/blob/5f136c2e35257e55dfbfd8afc7093aaded63a5b6/crates/ruff_python_semantic/src/model.rs#L1974
> 
> This rule is going to require type inference to get exactly right anyway, so this might be a good way to be more conservative in the meantime.
> 
> I also only gave the implementation a quick skim and could have overlooked this, but does this rule apply to comparisons like `<=` and `>` too? I would probably expect those to be allowed since something like `delta < 1e-8` should work as expected, I'm assuming that's what `math.isclose` does under the hood. Along those lines, we may want to rename the rule to `float-equality-comparison`, if it only applies to equality.

Yes, this only applies to `==` and `!=`, just as it works in SonarQube.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/float_equality_comparison.rs`:139 on 2026-01-01 15:32_

Could we use `ResolvedPythonType` for this check?

https://github.com/astral-sh/ruff/blob/b2b9d91859f2abc017e35d22f01696f141f6f5b1/crates/ruff_python_semantic/src/analyze/type_inference.rs#L68

It looks roughly similar, but I'm not sure if it covers exactly the same cases.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/float_equality_comparison.rs`:27 on 2026-01-01 15:34_

Does the rule currently apply to numpy/pandas values? I think I would kind of prefer only to cover floats and only recommend the standard library `math.isclose` for now. That gives a cleaner suggestion instead of offering two alternatives in the diagnostic message and hopefully avoids some false positives.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/float_equality_comparison.rs`:80 on 2026-01-01 15:36_

I think we should be able to avoid allocating a string here and just make `FloatEqualityComparison` hold `&'a str`s.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/float_equality_comparison.rs`:59 on 2026-01-01 15:38_

Let's unpack the struct here so that the variable names render in the documentation. We extract this `format!` argument for the description in the rules table.


```suggestion
    fn message(&self) -> String {
        let FloatEqualityComparison { left, right, operand } = self;
        format!(
            "Comparison `{left} {operand} {right}` should be replaced by `math.isclose()` or `numpy.isclose()`"
        )
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/float_equality_comparison.rs`:52 on 2026-01-01 15:39_

There's no fix yet right? We should set this to `Never` or just delete this line since `Never` is the default.


```suggestion
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/codes.rs`:1064 on 2026-01-01 15:49_

Sorry for the trouble, but we landed another PR using RUF067 to avoid introducing gaps in the rule codes, so you may run into conflicts here. I'm happy to renumber the rule if you want, I know it can be a pain.

---

_@ntBre reviewed on 2026-01-01 15:53_

Thank you! And sorry for the delay on the review.

I think this will be a really helpful rule, but I'm a bit wary of recommending `np.isclose`. What do you think about trying to restrict the rule to plain floats for now? We could always expand the scope later.

My other comments were just small suggestions.

---

_@chirizxc reviewed on 2026-01-01 17:32_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/codes.rs`:1064 on 2026-01-01 17:32_

There's no problem with that, we just need to find out what the next free rule code.

---

_Comment by @chirizxc on 2026-01-01 17:40_

> Thank you! And sorry for the delay on the review.
> 
> I think this will be a really helpful rule, but I'm a bit wary of recommending `np.isclose`. What do you think about trying to restrict the rule to plain floats for now? We could always expand the scope later.
> 
> My other comments were just small suggestions.

Then we will severely restrict the rule, since comparing two floats is less common

---

_Comment by @chirizxc on 2026-01-01 17:41_

~Essentially, `math.isclose()` / `numpy.isclose()` should cover 100% of cases.~

---

_Renamed from "[`ruff`] New rule float-comparison (`RUF067`)" to "[`ruff`] New rule float-comparison (`RUF070`)" by @chirizxc on 2026-01-01 17:58_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/ruff/rules/float_equality_comparison.rs`:27 on 2026-01-01 18:19_

I think we won't be able to avoid false positives, or we'll have to compare only floats. There is a note about this in the [SonarQube documentation](https://rules.sonarsource.com/python/RSPEC-1244/):

Whenever attempting to compare float values, it is important to consider the inherent imprecision of floating-point arithmetic.

One common solution to this problem is to use a tolerance value (also called epsilon) to define an acceptable range of difference between two floats. A tolerance value may be relative (based on the magnitude of the numbers being compared) or absolute. Note that comparing a value to 0 is a special case: as it has no magnitude, it is important to use an absolute tolerance value.

The math.isclose function allows to compare floats with a relative and absolute tolerance. One should however be careful when comparing values to 0, as by default, the absolute tolerance of `math.isclose` is 0.0 (this case is covered by rule [S6727](https://rules.sonarsource.com/python/RSPEC-6727)) . Depending on the library you’re using, equivalent functions exist, with possibly different default tolerances (e.g `numpy.isclose` or `torch.isclose` which are respectively designed to work with numpy arrays and pytorch tensors).

If precise decimal arithmetic is needed, another option is to use the Decimal class of the decimal module, which allows for exact decimal arithmetic.

---

_@chirizxc reviewed on 2026-01-01 18:20_

---

_@chirizxc reviewed on 2026-01-01 18:20_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/ruff/rules/float_equality_comparison.rs`:27 on 2026-01-01 18:20_

Should we also include such a note?

---

_Comment by @ntBre on 2026-01-02 15:20_

> Then we will severely restrict the rule, since comparing two floats is less common

I think you might be convincing me. cc @amyreese since you and I talked about this a bit too.

It feels slightly awkward to me to recommend both options in the error message. If we do want to keep flagging both types of issues, which will make the rule more useful, two other options would be:
- only recommend `math.isclose` and move the numpy suggestion (and maybe pytorch too, like sonar) into the rule documentation
- try to infer the types and recommend `math.isclose` or `numpy.isclose` depending on the type

The first of these is probably more practical.

I guess a third option would be to change the main error message to something like "Unreliable floating point equality comparison" and move the replacement suggestion into the `fix_title`, making it a bit less prominent. I kind of like this idea anyway.

But I'm curious to hear other thoughts from you and Amy!

---

_Comment by @chirizxc on 2026-01-02 15:39_

> * try to infer the types and recommend `math.isclose` or `numpy.isclose` depending on the type

I think it's impossible to do that without `ty` types for now.

> * only recommend `math.isclose` and move the numpy suggestion (and maybe pytorch too, like sonar) into the rule documentation

Then the error message should be clearer, so that people look at the documentation about it

I like the third option overall, but then the error message will become less clear. I mean that people will have to refer to the documentation to understand it. (understand more precisely how to replace the current code in a more correct way)


---

_Comment by @chirizxc on 2026-01-02 15:47_

> * only recommend `math.isclose` and move the numpy suggestion (and maybe pytorch too, like sonar) into the rule documentation

Here, it would be better to quote the entire sentence from the `SonarQube` documentation, to describe cases where the library itself provides such an opportunity: 
> Depending on the library you’re using, equivalent functions exist, with possibly different default tolerances (e.g numpy.isclose or torch.isclose which are respectively designed to work with numpy arrays and pytorch tensors).


---

_Review comment by @sobolevn on `crates/ruff_linter/resources/test/fixtures/ruff/RUF070.py`:27 on 2026-01-02 16:27_

```suggestion
assert -x == 1.0
assert -x == -1.0
```

these two checks are identical right now. let's use `UnaryOp` with a `float` in one of them.

---

_@sobolevn reviewed on 2026-01-02 16:28_

---

_Comment by @amyreese on 2026-01-09 00:26_

I'm in favor of just having the documentation clear about `math.isclose()` being a baseline recommendation, and point to docs for ecosystem-specific alternatives.

---

_Comment by @chirizxc on 2026-01-09 12:47_

So, we choose the option to rename error message to "Unreliable floating point equality comparison" and update the documentation accordingly?

---

_Comment by @chirizxc on 2026-01-09 16:32_

@ntBre Are there any restrictions on the length of documentation for the rule🤔? It has become longer in this commit.

---

_Comment by @MichaReiser on 2026-01-09 16:39_

> @ntBre Are there any restrictions on the length of documentation for the rule🤔? It has become longer in this commit.

Not really. It should be concise, but there's no upper limit on how long the documentation is allowed to be

---
