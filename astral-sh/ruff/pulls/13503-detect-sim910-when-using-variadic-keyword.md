```yaml
number: 13503
title: "Detect SIM910 when using variadic keyword arguments, i.e., `**kwargs`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/sim910-dict
created_at: 2024-09-24T14:10:21Z
updated_at: 2024-09-25T15:03:02Z
url: https://github.com/astral-sh/ruff/pull/13503
synced_at: 2026-01-12T15:55:44Z
```

# Detect SIM910 when using variadic keyword arguments, i.e., `**kwargs`

---

_@zanieb_

Closes https://github.com/astral-sh/ruff/issues/13493


---

_Label `bug` added by @zanieb on 2024-09-24 14:10_

---

_Comment by @github-actions[bot] on 2024-09-24 14:26_

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
+ <a href='https://github.com/apache/airflow/blob/f2775bf9f1062e71d1f7796cde4da92018c6b57a/airflow/providers/amazon/aws/secrets/secrets_manager.py#L173'>airflow/providers/amazon/aws/secrets/secrets_manager.py:173:29:</a> SIM910 [*] Use `kwargs.get("profile_name")` instead of `kwargs.get("profile_name", None)`
+ <a href='https://github.com/apache/airflow/blob/f2775bf9f1062e71d1f7796cde4da92018c6b57a/airflow/providers/amazon/aws/secrets/systems_manager.py#L107'>airflow/providers/amazon/aws/secrets/systems_manager.py:107:29:</a> SIM910 [*] Use `kwargs.get("profile_name")` instead of `kwargs.get("profile_name", None)`
+ <a href='https://github.com/apache/airflow/blob/f2775bf9f1062e71d1f7796cde4da92018c6b57a/tests/conftest.py#L954'>tests/conftest.py:954:28:</a> SIM910 [*] Use `kwargs.get("default_args")` instead of `kwargs.get("default_args", None)`
+ <a href='https://github.com/apache/airflow/blob/f2775bf9f1062e71d1f7796cde4da92018c6b57a/tests/providers/elasticsearch/log/elasticmock/utilities/__init__.py#L88'>tests/providers/elasticsearch/log/elasticmock/utilities/__init__.py:88:32:</a> SIM910 [*] Use `kwargs.get("body")` instead of `kwargs.get("body", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/046c096d86b0051eea00862f3d0291c457187ef6/superset/db_engine_specs/presto.py#L650'>superset/db_engine_specs/presto.py:650:13:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/apache/superset/blob/046c096d86b0051eea00862f3d0291c457187ef6/superset/db_engine_specs/presto.py#L650'>superset/db_engine_specs/presto.py:650:13:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
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
+ <a href='https://github.com/reflex-dev/reflex/blob/5e738fd62bbac46467d52558d1742d94e0d8a9e1/reflex/components/datadisplay/dataeditor.py#L332'>reflex/components/datadisplay/dataeditor.py:332:16:</a> SIM910 [*] Use `props.get("rows")` instead of `props.get("rows", None)`
+ <a href='https://github.com/reflex-dev/reflex/blob/5e738fd62bbac46467d52558d1742d94e0d8a9e1/reflex/vars/base.py#L242'>reflex/vars/base.py:242:24:</a> SIM910 [*] Use `kwargs.get("_js_expr")` instead of `kwargs.get("_js_expr", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/5a978edf40f5b55cd794899602a21d6e9004d124/corporate/tests/test_stripe.py#L652'>corporate/tests/test_stripe.py:652:41:</a> SIM910 [*] Use `kwargs.get("remote_server_plan_start_date")` instead of `kwargs.get("remote_server_plan_start_date", None)`
+ <a href='https://github.com/zulip/zulip/blob/5a978edf40f5b55cd794899602a21d6e9004d124/zerver/lib/test_classes.py#L1897'>zerver/lib/test_classes.py:1897:37:</a> SIM910 [*] Use `kwargs.get("mentioned_user_group_id")` instead of `kwargs.get("mentioned_user_group_id", None)`
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
+ <a href='https://github.com/apache/airflow/blob/f2775bf9f1062e71d1f7796cde4da92018c6b57a/airflow/providers/amazon/aws/secrets/secrets_manager.py#L173'>airflow/providers/amazon/aws/secrets/secrets_manager.py:173:29:</a> SIM910 [*] Use `kwargs.get("profile_name")` instead of `kwargs.get("profile_name", None)`
+ <a href='https://github.com/apache/airflow/blob/f2775bf9f1062e71d1f7796cde4da92018c6b57a/airflow/providers/amazon/aws/secrets/systems_manager.py#L107'>airflow/providers/amazon/aws/secrets/systems_manager.py:107:29:</a> SIM910 [*] Use `kwargs.get("profile_name")` instead of `kwargs.get("profile_name", None)`
+ <a href='https://github.com/apache/airflow/blob/f2775bf9f1062e71d1f7796cde4da92018c6b57a/tests/conftest.py#L954'>tests/conftest.py:954:28:</a> SIM910 [*] Use `kwargs.get("default_args")` instead of `kwargs.get("default_args", None)`
+ <a href='https://github.com/apache/airflow/blob/f2775bf9f1062e71d1f7796cde4da92018c6b57a/tests/providers/elasticsearch/log/elasticmock/utilities/__init__.py#L88'>tests/providers/elasticsearch/log/elasticmock/utilities/__init__.py:88:32:</a> SIM910 [*] Use `kwargs.get("body")` instead of `kwargs.get("body", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/046c096d86b0051eea00862f3d0291c457187ef6/superset/db_engine_specs/presto.py#L650'>superset/db_engine_specs/presto.py:650:13:</a> SIM118 Use `key in dict` instead of `key in dict.keys()`
+ <a href='https://github.com/apache/superset/blob/046c096d86b0051eea00862f3d0291c457187ef6/superset/db_engine_specs/presto.py#L650'>superset/db_engine_specs/presto.py:650:13:</a> SIM118 [*] Use `key in dict` instead of `key in dict.keys()`
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
+ <a href='https://github.com/reflex-dev/reflex/blob/5e738fd62bbac46467d52558d1742d94e0d8a9e1/reflex/components/datadisplay/dataeditor.py#L332'>reflex/components/datadisplay/dataeditor.py:332:16:</a> SIM910 [*] Use `props.get("rows")` instead of `props.get("rows", None)`
+ <a href='https://github.com/reflex-dev/reflex/blob/5e738fd62bbac46467d52558d1742d94e0d8a9e1/reflex/vars/base.py#L242'>reflex/vars/base.py:242:24:</a> SIM910 [*] Use `kwargs.get("_js_expr")` instead of `kwargs.get("_js_expr", None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/5a978edf40f5b55cd794899602a21d6e9004d124/corporate/tests/test_stripe.py#L652'>corporate/tests/test_stripe.py:652:41:</a> SIM910 [*] Use `kwargs.get("remote_server_plan_start_date")` instead of `kwargs.get("remote_server_plan_start_date", None)`
+ <a href='https://github.com/zulip/zulip/blob/5a978edf40f5b55cd794899602a21d6e9004d124/zerver/lib/test_classes.py#L1897'>zerver/lib/test_classes.py:1897:37:</a> SIM910 [*] Use `kwargs.get("mentioned_user_group_id")` instead of `kwargs.get("mentioned_user_group_id", None)`
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

_Review requested from @charliermarsh by @zanieb on 2024-09-24 16:17_

---

_Comment by @zanieb on 2024-09-24 16:18_

There are some ecosystem hits here but I don't think this needs to be gated by preview.

---

_@MichaReiser reviewed on 2024-09-25 07:15_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/typing.rs`:546 on 2024-09-25 07:15_

I don't know anything about the type checking code so I might be wrong here but doesn't this now return true for all type checkers? 

Looking at the definition of `is_dict` it returns whatever `check_types` returns and so do all other `is_*` methods:

https://github.com/astral-sh/ruff/blob/aa0db338d935aa40fafaa05feff7343c42491d8e/crates/ruff_python_semantic/src/analyze/typing.rs#L743-L745

But we could move this check into `is_dict` (and add support for `*args` to `is_list`)... but I don't know if this is the idiomatic way to do this. An alternative is to add a new `T::match_kwarg` that returns `true` or false. But it feels a bit overkill.


---

_@zanieb reviewed on 2024-09-25 13:36_

---

_Review comment by @zanieb on `crates/ruff_python_semantic/src/analyze/typing.rs`:546 on 2024-09-25 13:36_

Oh I see what you're saying... eek..

---

_@zanieb reviewed on 2024-09-25 13:47_

---

_Review comment by @zanieb on `crates/ruff_python_semantic/src/analyze/typing.rs`:546 on 2024-09-25 13:47_

I moved it into `is_dict` — I didn't want to touch the trait for this. We perform an extra `statement` lookup now but I don't think that's expensive.

---

_@zanieb reviewed on 2024-09-25 14:02_

---

_Review comment by @zanieb on `crates/ruff_python_semantic/src/analyze/typing.rs`:546 on 2024-09-25 14:02_

And opened a corresponding `is_tuple` fix https://github.com/astral-sh/ruff/pull/13512

---

_@MichaReiser approved on 2024-09-25 14:59_

---

_Merged by @zanieb on 2024-09-25 15:02_

---

_Closed by @zanieb on 2024-09-25 15:02_

---

_Branch deleted on 2024-09-25 15:03_

---
