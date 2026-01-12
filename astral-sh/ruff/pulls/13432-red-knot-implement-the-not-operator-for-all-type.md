```yaml
number: 13432
title: "red-knot: Implement the `not` operator for all `Type` variants"
type: pull_request
state: merged
author: haarisr
labels:
  - ty
assignees: []
merged: true
base: main
head: hr/not_int
created_at: 2024-09-21T00:34:41Z
updated_at: 2024-09-25T20:44:19Z
url: https://github.com/astral-sh/ruff/pull/13432
synced_at: 2026-01-12T15:55:44Z
```

# red-knot: Implement the `not` operator for all `Type` variants

---

_@haarisr_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Adds unary not operator (not) for integer literals.

Not sure if this is the best way to do it. Feedback would be helpful

Contributes to astral-sh/ty#244
## Test Plan

<!-- How was it tested? -->

Added new test basing of the test values in the `unary sub operator`. Small and large value for both positive and negative integers as well as for zero (which is the only True) test case


---

_Review requested from @carljm by @haarisr on 2024-09-21 00:34_

---

_Review requested from @MichaReiser by @haarisr on 2024-09-21 00:34_

---

_Review requested from @AlexWaygood by @haarisr on 2024-09-21 00:34_

---

_Comment by @github-actions[bot] on 2024-09-21 00:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+49 -0 violations, +4 -0 fixes in 7 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/79a8882d2f60eea5d189e07dcbe778b403cc968d/airflow/providers/amazon/aws/secrets/secrets_manager.py#L173'>airflow/providers/amazon/aws/secrets/secrets_manager.py:173:29:</a> SIM910 [*] Use `kwargs.get("profile_name")` instead of `kwargs.get("profile_name", None)`
+ <a href='https://github.com/apache/airflow/blob/79a8882d2f60eea5d189e07dcbe778b403cc968d/airflow/providers/amazon/aws/secrets/systems_manager.py#L107'>airflow/providers/amazon/aws/secrets/systems_manager.py:107:29:</a> SIM910 [*] Use `kwargs.get("profile_name")` instead of `kwargs.get("profile_name", None)`
+ <a href='https://github.com/apache/airflow/blob/79a8882d2f60eea5d189e07dcbe778b403cc968d/tests/conftest.py#L954'>tests/conftest.py:954:28:</a> SIM910 [*] Use `kwargs.get("default_args")` instead of `kwargs.get("default_args", None)`
+ <a href='https://github.com/apache/airflow/blob/79a8882d2f60eea5d189e07dcbe778b403cc968d/tests/providers/elasticsearch/log/elasticmock/utilities/__init__.py#L88'>tests/providers/elasticsearch/log/elasticmock/utilities/__init__.py:88:32:</a> SIM910 [*] Use `kwargs.get("body")` instead of `kwargs.get("body", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/ad2998598f0802f81815214cc3cc0b9ee9196938/superset/db_engine_specs/presto.py#L650'>superset/db_engine_specs/presto.py:650:13:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/apache/superset/blob/ad2998598f0802f81815214cc3cc0b9ee9196938/superset/db_engine_specs/presto.py#L650'>superset/db_engine_specs/presto.py:650:13:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L193'>src/bokeh/plotting/_figure.py:193:13:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L193'>src/bokeh/plotting/_figure.py:193:13:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L205'>src/bokeh/plotting/contour.py:205:17:</a> SIM910 [*] Use `visuals.get("line_color")` instead of `visuals.get("line_color", None)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L222'>src/bokeh/plotting/contour.py:222:17:</a> SIM910 [*] Use `visuals.get("fill_color")` instead of `visuals.get("fill_color", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+37 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/bulk_writer/local_bulk_writer.py#L113'>pymilvus/bulk_writer/local_bulk_writer.py:113:21:</a> SIM910 [*] Use `kwargs.get("call_back")` instead of `kwargs.get("call_back", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/asynch.py#L108'>pymilvus/client/asynch.py:108:18:</a> SIM910 [*] Use `kwargs.get("timeout")` instead of `kwargs.get("timeout", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L1255'>pymilvus/client/grpc_handler.py:1255:23:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L128'>pymilvus/client/grpc_handler.py:128:13:</a> SIM910 [*] Use `kwargs.get("user")` instead of `kwargs.get("user", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L129'>pymilvus/client/grpc_handler.py:129:13:</a> SIM910 [*] Use `kwargs.get("password")` instead of `kwargs.get("password", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L130'>pymilvus/client/grpc_handler.py:130:13:</a> SIM910 [*] Use `kwargs.get("token")` instead of `kwargs.get("token", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L1464'>pymilvus/client/grpc_handler.py:1464:23:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L1991'>pymilvus/client/grpc_handler.py:1991:23:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L473'>pymilvus/client/grpc_handler.py:473:18:</a> SIM910 [*] Use `kwargs.get("schema")` instead of `kwargs.get("schema", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L570'>pymilvus/client/grpc_handler.py:570:22:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L602'>pymilvus/client/grpc_handler.py:602:28:</a> SIM910 [*] Use `kwargs.get("param_name")` instead of `kwargs.get("param_name", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L607'>pymilvus/client/grpc_handler.py:607:22:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L667'>pymilvus/client/grpc_handler.py:667:22:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L730'>pymilvus/client/grpc_handler.py:730:24:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L749'>pymilvus/client/grpc_handler.py:749:24:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L784'>pymilvus/client/grpc_handler.py:784:33:</a> SIM910 [*] Use `kwargs.get("guarantee_timestamp")` instead of `kwargs.get("guarantee_timestamp", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L820'>pymilvus/client/grpc_handler.py:820:33:</a> SIM910 [*] Use `kwargs.get("guarantee_timestamp")` instead of `kwargs.get("guarantee_timestamp", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L90'>pymilvus/client/grpc_handler.py:90:22:</a> SIM910 [*] Use `kwargs.get("user")` instead of `kwargs.get("user", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L92'>pymilvus/client/grpc_handler.py:92:36:</a> SIM910 [*] Use `kwargs.get("db_name")` instead of `kwargs.get("db_name", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L980'>pymilvus/client/grpc_handler.py:980:23:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L1052'>pymilvus/client/prepare.py:1052:17:</a> SIM910 [*] Use `kwargs.get("limit")` instead of `kwargs.get("limit", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L1056'>pymilvus/client/prepare.py:1056:18:</a> SIM910 [*] Use `kwargs.get("offset")` instead of `kwargs.get("offset", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L1135'>pymilvus/client/prepare.py:1135:25:</a> SIM910 [*] Use `kwargs.get("channel_names")` instead of `kwargs.get("channel_names", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L813'>pymilvus/client/prepare.py:813:12:</a> SIM910 [*] Use `kwargs.get(RANK_GROUP_SCORER)` instead of `kwargs.get(RANK_GROUP_SCORER, None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L822'>pymilvus/client/prepare.py:822:12:</a> SIM910 [*] Use `kwargs.get(GROUP_BY_FIELD)` instead of `kwargs.get(GROUP_BY_FIELD, None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L831'>pymilvus/client/prepare.py:831:12:</a> SIM910 [*] Use `kwargs.get(GROUP_SIZE)` instead of `kwargs.get(GROUP_SIZE, None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L840'>pymilvus/client/prepare.py:840:12:</a> SIM910 [*] Use `kwargs.get(GROUP_STRICT_SIZE)` instead of `kwargs.get(GROUP_STRICT_SIZE, None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L92'>pymilvus/client/prepare.py:92:26:</a> SIM910 [*] Use `kwargs.get("num_partitions")` instead of `kwargs.get("num_partitions", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/decorators.py#L170'>pymilvus/decorators.py:170:21:</a> SIM910 [*] Use `kwargs.get("log_level")` instead of `kwargs.get("log_level", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/decorators.py#L171'>pymilvus/decorators.py:171:22:</a> SIM910 [*] Use `kwargs.get("client_request_id")` instead of `kwargs.get("client_request_id", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/decorators.py#L56'>pymilvus/decorators.py:56:24:</a> SIM910 [*] Use `kwargs.get("timeout")` instead of `kwargs.get("timeout", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/decorators.py#L57'>pymilvus/decorators.py:57:28:</a> SIM910 [*] Use `kwargs.get("retry_times")` instead of `kwargs.get("retry_times", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/orm/collection.py#L188'>pymilvus/orm/collection.py:188:51:</a> SIM910 [*] Use `kwargs.get("auto_id")` instead of `kwargs.get("auto_id", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/orm/schema.py#L324'>pymilvus/orm/schema.py:324:30:</a> SIM910 [*] Use `kwargs.get("default_value")` instead of `kwargs.get("default_value", None)`
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/7eaffb05e7cc3b6a0a256e08d9118c71a3b31ffa/mlflow/openai/_openai_autolog.py#L203'>mlflow/openai/_openai_autolog.py:203:25:</a> SIM910 [*] Use `kwargs.get("model")` instead of `kwargs.get("model", None)`
+ <a href='https://github.com/mlflow/mlflow/blob/7eaffb05e7cc3b6a0a256e08d9118c71a3b31ffa/mlflow/pytorch/_pytorch_autolog.py#L52'>mlflow/pytorch/_pytorch_autolog.py:52:53:</a> SIM910 [*] Use `kwargs.get("global_step")` instead of `kwargs.get("global_step", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/e1538b75f8ab5639fe609176e9c26ae218e35485/reflex/components/datadisplay/dataeditor.py#L332'>reflex/components/datadisplay/dataeditor.py:332:16:</a> SIM910 [*] Use `props.get("rows")` instead of `props.get("rows", None)`
+ <a href='https://github.com/reflex-dev/reflex/blob/e1538b75f8ab5639fe609176e9c26ae218e35485/reflex/vars/base.py#L242'>reflex/vars/base.py:242:24:</a> SIM910 [*] Use `kwargs.get("_js_expr")` instead of `kwargs.get("_js_expr", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/b0ca32c95525f8056985a9ccf9c901be28e8256e/corporate/tests/test_stripe.py#L652'>corporate/tests/test_stripe.py:652:41:</a> SIM910 [*] Use `kwargs.get("remote_server_plan_start_date")` instead of `kwargs.get("remote_server_plan_start_date", None)`
+ <a href='https://github.com/zulip/zulip/blob/b0ca32c95525f8056985a9ccf9c901be28e8256e/zerver/lib/test_classes.py#L1897'>zerver/lib/test_classes.py:1897:37:</a> SIM910 [*] Use `kwargs.get("mentioned_user_group_id")` instead of `kwargs.get("mentioned_user_group_id", None)`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM910 | 49 | 49 | 0 | 0 | 0 |
| SIM118 | 4 | 0 | 0 | 4 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+49 -0 violations, +4 -0 fixes in 7 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/79a8882d2f60eea5d189e07dcbe778b403cc968d/airflow/providers/amazon/aws/secrets/secrets_manager.py#L173'>airflow/providers/amazon/aws/secrets/secrets_manager.py:173:29:</a> SIM910 [*] Use `kwargs.get("profile_name")` instead of `kwargs.get("profile_name", None)`
+ <a href='https://github.com/apache/airflow/blob/79a8882d2f60eea5d189e07dcbe778b403cc968d/airflow/providers/amazon/aws/secrets/systems_manager.py#L107'>airflow/providers/amazon/aws/secrets/systems_manager.py:107:29:</a> SIM910 [*] Use `kwargs.get("profile_name")` instead of `kwargs.get("profile_name", None)`
+ <a href='https://github.com/apache/airflow/blob/79a8882d2f60eea5d189e07dcbe778b403cc968d/tests/conftest.py#L954'>tests/conftest.py:954:28:</a> SIM910 [*] Use `kwargs.get("default_args")` instead of `kwargs.get("default_args", None)`
+ <a href='https://github.com/apache/airflow/blob/79a8882d2f60eea5d189e07dcbe778b403cc968d/tests/providers/elasticsearch/log/elasticmock/utilities/__init__.py#L88'>tests/providers/elasticsearch/log/elasticmock/utilities/__init__.py:88:32:</a> SIM910 [*] Use `kwargs.get("body")` instead of `kwargs.get("body", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/ad2998598f0802f81815214cc3cc0b9ee9196938/superset/db_engine_specs/presto.py#L650'>superset/db_engine_specs/presto.py:650:13:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/apache/superset/blob/ad2998598f0802f81815214cc3cc0b9ee9196938/superset/db_engine_specs/presto.py#L650'>superset/db_engine_specs/presto.py:650:13:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L193'>src/bokeh/plotting/_figure.py:193:13:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L193'>src/bokeh/plotting/_figure.py:193:13:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L205'>src/bokeh/plotting/contour.py:205:17:</a> SIM910 [*] Use `visuals.get("line_color")` instead of `visuals.get("line_color", None)`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/contour.py#L222'>src/bokeh/plotting/contour.py:222:17:</a> SIM910 [*] Use `visuals.get("fill_color")` instead of `visuals.get("fill_color", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+37 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/bulk_writer/local_bulk_writer.py#L113'>pymilvus/bulk_writer/local_bulk_writer.py:113:21:</a> SIM910 [*] Use `kwargs.get("call_back")` instead of `kwargs.get("call_back", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/asynch.py#L108'>pymilvus/client/asynch.py:108:18:</a> SIM910 [*] Use `kwargs.get("timeout")` instead of `kwargs.get("timeout", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L1255'>pymilvus/client/grpc_handler.py:1255:23:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L128'>pymilvus/client/grpc_handler.py:128:13:</a> SIM910 [*] Use `kwargs.get("user")` instead of `kwargs.get("user", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L129'>pymilvus/client/grpc_handler.py:129:13:</a> SIM910 [*] Use `kwargs.get("password")` instead of `kwargs.get("password", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L130'>pymilvus/client/grpc_handler.py:130:13:</a> SIM910 [*] Use `kwargs.get("token")` instead of `kwargs.get("token", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L1464'>pymilvus/client/grpc_handler.py:1464:23:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L1991'>pymilvus/client/grpc_handler.py:1991:23:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L473'>pymilvus/client/grpc_handler.py:473:18:</a> SIM910 [*] Use `kwargs.get("schema")` instead of `kwargs.get("schema", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L570'>pymilvus/client/grpc_handler.py:570:22:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L602'>pymilvus/client/grpc_handler.py:602:28:</a> SIM910 [*] Use `kwargs.get("param_name")` instead of `kwargs.get("param_name", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L607'>pymilvus/client/grpc_handler.py:607:22:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L667'>pymilvus/client/grpc_handler.py:667:22:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L730'>pymilvus/client/grpc_handler.py:730:24:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L749'>pymilvus/client/grpc_handler.py:749:24:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L784'>pymilvus/client/grpc_handler.py:784:33:</a> SIM910 [*] Use `kwargs.get("guarantee_timestamp")` instead of `kwargs.get("guarantee_timestamp", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L820'>pymilvus/client/grpc_handler.py:820:33:</a> SIM910 [*] Use `kwargs.get("guarantee_timestamp")` instead of `kwargs.get("guarantee_timestamp", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L90'>pymilvus/client/grpc_handler.py:90:22:</a> SIM910 [*] Use `kwargs.get("user")` instead of `kwargs.get("user", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L92'>pymilvus/client/grpc_handler.py:92:36:</a> SIM910 [*] Use `kwargs.get("db_name")` instead of `kwargs.get("db_name", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/grpc_handler.py#L980'>pymilvus/client/grpc_handler.py:980:23:</a> SIM910 [*] Use `kwargs.get("_callback")` instead of `kwargs.get("_callback", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L1052'>pymilvus/client/prepare.py:1052:17:</a> SIM910 [*] Use `kwargs.get("limit")` instead of `kwargs.get("limit", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L1056'>pymilvus/client/prepare.py:1056:18:</a> SIM910 [*] Use `kwargs.get("offset")` instead of `kwargs.get("offset", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L1135'>pymilvus/client/prepare.py:1135:25:</a> SIM910 [*] Use `kwargs.get("channel_names")` instead of `kwargs.get("channel_names", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L813'>pymilvus/client/prepare.py:813:12:</a> SIM910 [*] Use `kwargs.get(RANK_GROUP_SCORER)` instead of `kwargs.get(RANK_GROUP_SCORER, None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L822'>pymilvus/client/prepare.py:822:12:</a> SIM910 [*] Use `kwargs.get(GROUP_BY_FIELD)` instead of `kwargs.get(GROUP_BY_FIELD, None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L831'>pymilvus/client/prepare.py:831:12:</a> SIM910 [*] Use `kwargs.get(GROUP_SIZE)` instead of `kwargs.get(GROUP_SIZE, None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L840'>pymilvus/client/prepare.py:840:12:</a> SIM910 [*] Use `kwargs.get(GROUP_STRICT_SIZE)` instead of `kwargs.get(GROUP_STRICT_SIZE, None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/client/prepare.py#L92'>pymilvus/client/prepare.py:92:26:</a> SIM910 [*] Use `kwargs.get("num_partitions")` instead of `kwargs.get("num_partitions", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/decorators.py#L170'>pymilvus/decorators.py:170:21:</a> SIM910 [*] Use `kwargs.get("log_level")` instead of `kwargs.get("log_level", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/decorators.py#L171'>pymilvus/decorators.py:171:22:</a> SIM910 [*] Use `kwargs.get("client_request_id")` instead of `kwargs.get("client_request_id", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/decorators.py#L56'>pymilvus/decorators.py:56:24:</a> SIM910 [*] Use `kwargs.get("timeout")` instead of `kwargs.get("timeout", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/decorators.py#L57'>pymilvus/decorators.py:57:28:</a> SIM910 [*] Use `kwargs.get("retry_times")` instead of `kwargs.get("retry_times", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/orm/collection.py#L188'>pymilvus/orm/collection.py:188:51:</a> SIM910 [*] Use `kwargs.get("auto_id")` instead of `kwargs.get("auto_id", None)`
+ <a href='https://github.com/milvus-io/pymilvus/blob/da51ba14edde3c67b682f2d0ae1b85a095815692/pymilvus/orm/schema.py#L324'>pymilvus/orm/schema.py:324:30:</a> SIM910 [*] Use `kwargs.get("default_value")` instead of `kwargs.get("default_value", None)`
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/7eaffb05e7cc3b6a0a256e08d9118c71a3b31ffa/mlflow/openai/_openai_autolog.py#L203'>mlflow/openai/_openai_autolog.py:203:25:</a> SIM910 [*] Use `kwargs.get("model")` instead of `kwargs.get("model", None)`
+ <a href='https://github.com/mlflow/mlflow/blob/7eaffb05e7cc3b6a0a256e08d9118c71a3b31ffa/mlflow/pytorch/_pytorch_autolog.py#L52'>mlflow/pytorch/_pytorch_autolog.py:52:53:</a> SIM910 [*] Use `kwargs.get("global_step")` instead of `kwargs.get("global_step", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/e1538b75f8ab5639fe609176e9c26ae218e35485/reflex/components/datadisplay/dataeditor.py#L332'>reflex/components/datadisplay/dataeditor.py:332:16:</a> SIM910 [*] Use `props.get("rows")` instead of `props.get("rows", None)`
+ <a href='https://github.com/reflex-dev/reflex/blob/e1538b75f8ab5639fe609176e9c26ae218e35485/reflex/vars/base.py#L242'>reflex/vars/base.py:242:24:</a> SIM910 [*] Use `kwargs.get("_js_expr")` instead of `kwargs.get("_js_expr", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/b0ca32c95525f8056985a9ccf9c901be28e8256e/corporate/tests/test_stripe.py#L652'>corporate/tests/test_stripe.py:652:41:</a> SIM910 [*] Use `kwargs.get("remote_server_plan_start_date")` instead of `kwargs.get("remote_server_plan_start_date", None)`
+ <a href='https://github.com/zulip/zulip/blob/b0ca32c95525f8056985a9ccf9c901be28e8256e/zerver/lib/test_classes.py#L1897'>zerver/lib/test_classes.py:1897:37:</a> SIM910 [*] Use `kwargs.get("mentioned_user_group_id")` instead of `kwargs.get("mentioned_user_group_id", None)`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM910 | 49 | 49 | 0 | 0 | 0 |
| SIM118 | 4 | 0 | 0 | 4 | 0 |

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2218 on 2024-09-21 04:07_

you can simplify this slightly:

```suggestion
            (UnaryOp::Not, Type::IntLiteral(value)) => Type::BooleanLiteral(value == 0),
```

---

_@AlexWaygood reviewed on 2024-09-21 04:11_

Thanks!

---

_@carljm reviewed on 2024-09-21 04:26_

Thank you!

I think for these other types what we should do is create a `Type::bool` method (corresponding to the `__bool__` magic method at runtime) that returns either BooleanLiteral (for those types whose Boolean value is statically known) or the `bool` builtin type. And then unary Not operator on most types will just call `bool` on the type and reverse it (if it's a BooleanLiteral.) Because we'll have other needs for the bool method outside unary Not. 

---

_Label `red-knot` added by @AlexWaygood on 2024-09-25 00:28_

---

_@AlexWaygood requested changes on 2024-09-25 00:33_

Following https://github.com/astral-sh/ruff/pull/13449 being merged, if you rebase this PR on `main` I think you can simplify this PR like so:

```diff
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index d388fa8b2..33a22a378 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -936,7 +936,6 @@ impl Truthiness {
         matches!(self, Truthiness::Ambiguous)
     }
 
-    #[allow(unused)]
     const fn negate(self) -> Self {
         match self {
             Self::AlwaysTrue => Self::AlwaysFalse,
@@ -945,7 +944,6 @@ impl Truthiness {
         }
     }
 
-    #[allow(unused)]
     fn into_type(self, db: &dyn Db) -> Type {
         match self {
             Self::AlwaysTrue => Type::BooleanLiteral(true),
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 23ad71f3c..9b8362000 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -2210,11 +2210,7 @@ impl<'db> TypeInferenceBuilder<'db> {
 
         match (op, self.infer_expression(operand)) {
             (UnaryOp::USub, Type::IntLiteral(value)) => Type::IntLiteral(-value),
-            (UnaryOp::Not, Type::BooleanLiteral(value)) => Type::BooleanLiteral(!value),
-            (UnaryOp::Not, Type::IntLiteral(value)) => match value {
-                0 => Type::BooleanLiteral(true),
-                _ => Type::BooleanLiteral(false),
-            },
+            (UnaryOp::Not, ty) => ty.bool(self.db).negate().into_type(self.db),
             _ => Type::Unknown, // TODO other unary op types
         }
     }
```

but we'll also want to add a fair few tests, because we'll now be implementing `not` for _all_ variants of `Type` rather than just one ;)

---

_Review comment by @haarisr on `crates/red_knot_python_semantic/src/types/infer.rs`:3198 on 2024-09-25 04:38_

The test for `b` fails. Not sure why the returned type is `bool`. Any help here would be appreciated

---

_@haarisr reviewed on 2024-09-25 04:40_

Did most of the types that I could.

Would like help on tests for `Type::Any | Type::Never | Type::Unknown | Type::Unbound`

---

_Review requested from @AlexWaygood by @haarisr on 2024-09-25 04:41_

---

_Review requested from @carljm by @haarisr on 2024-09-25 04:41_

---

_Renamed from "red-knot: Add not unary operator for integer literals" to "red-knot: Implement the `not` operator for all `Type` variants" by @AlexWaygood on 2024-09-25 04:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3198 on 2024-09-25 18:45_

The problem here is that `reveal_type` is only conditionally defined in `typing.pyi` (by version, and we don't yet support special handling of `sys.version_info` checks), so the type of `typing.reveal_type` is thus `Unknown | Literal[reveal_type]`, and the `Unknown` constituent has `Truthiness::Ambiguous`.

Tbh I'm not sure this behavior of adding `Unknown` to the type is correct; if we can see that in one path a name simply doesn't exist, we might want to issue a diagnostic, but it doesn't make sense to add `Unknown` to the union; it's not like there might be some other random definition of the name we don't know about (ok, yes, technically there could be something dynamic happening with the global namespace that we don't know about, but that's always true and not especially relevant here). Either you'll get the name imported, or you'll get an error, you won't get some random other type.

Anyway, that's all for resolution in a different PR. I think the right answer here for now is just a TODO comment:
```suggestion
            b = not reveal_type
            "#,
        )?;

        assert_public_ty(&db, "src/a.py", "a", "Literal[False]");
        // TODO Unknown should not be part of the type of typing.reveal_type
        // assert_public_ty(&db, "src/a.py", "b", "Literal[False]");
```

---

_@carljm reviewed on 2024-09-25 19:01_

Looks good to me! Just needs the failing test assertion commented out for now, with TODO comment.

---

_@carljm approved on 2024-09-25 20:43_

---

_Merged by @carljm on 2024-09-25 20:44_

---

_Closed by @carljm on 2024-09-25 20:44_

---
