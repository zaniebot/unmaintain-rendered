```yaml
number: 20118
title: "[`flake8-use-pathlib`] Make `PTH119` and `PTH120` fixes unsafe because they can change behavior"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: pth_119_and_120
created_at: 2025-08-27T17:49:02Z
updated_at: 2025-09-02T16:22:13Z
url: https://github.com/astral-sh/ruff/pull/20118
synced_at: 2026-01-12T15:56:54Z
```

# [`flake8-use-pathlib`] Make `PTH119` and `PTH120` fixes unsafe because they can change behavior

---

_@chirizxc_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/20112

## Test Plan
`cargo nextest run flake8_use_pathlib`


---

_Comment by @github-actions[bot] on 2025-08-27 17:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -780 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -284 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/src/airflow/config_templates/default_webserver_config.py#L32'>airflow-core/src/airflow/config_templates/default_webserver_config.py:32:27:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/src/airflow/config_templates/default_webserver_config.py#L32'>airflow-core/src/airflow/config_templates/default_webserver_config.py:32:27:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
- <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/src/airflow/configuration.py#L175'>airflow-core/src/airflow/configuration.py:175:34:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/src/airflow/configuration.py#L175'>airflow-core/src/airflow/configuration.py:175:34:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
- <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/src/airflow/configuration.py#L2259'>airflow-core/src/airflow/configuration.py:2259:21:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/src/airflow/configuration.py#L2259'>airflow-core/src/airflow/configuration.py:2259:21:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
- <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/src/airflow/configuration.py#L2259'>airflow-core/src/airflow/configuration.py:2259:5:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/src/airflow/configuration.py#L2259'>airflow-core/src/airflow/configuration.py:2259:5:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
- <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/src/airflow/configuration.py#L2268'>airflow-core/src/airflow/configuration.py:2268:21:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/src/airflow/configuration.py#L2268'>airflow-core/src/airflow/configuration.py:2268:21:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
... 195 additional changes omitted for rule PTH120
- <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/src/airflow/utils/email.py#L208'>airflow-core/src/airflow/utils/email.py:208:20:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/src/airflow/utils/email.py#L208'>airflow-core/src/airflow/utils/email.py:208:20:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
- <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/tests/unit/models/test_dagbag.py#L313'>airflow-core/tests/unit/models/test_dagbag.py:313:40:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/tests/unit/models/test_dagbag.py#L313'>airflow-core/tests/unit/models/test_dagbag.py:313:40:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
- <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/tests/unit/models/test_dagbag.py#L377'>airflow-core/tests/unit/models/test_dagbag.py:377:20:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/tests/unit/models/test_dagbag.py#L377'>airflow-core/tests/unit/models/test_dagbag.py:377:20:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
- <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/tests/unit/utils/log/test_file_processor_handler.py#L44'>airflow-core/tests/unit/utils/log/test_file_processor_handler.py:44:16:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/apache/airflow/blob/f4daefc35980b28e9da33e45852f756316bd6940/airflow-core/tests/unit/utils/log/test_file_processor_handler.py#L44'>airflow-core/tests/unit/utils/log/test_file_processor_handler.py:44:16:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
... 266 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +0 -26 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/d1839697442157ace4beba2eaff45558a8d2ba08/docker/pythonpath_dev/superset_config.py#L123'>docker/pythonpath_dev/superset_config.py:123:16:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/apache/superset/blob/d1839697442157ace4beba2eaff45558a8d2ba08/docker/pythonpath_dev/superset_config.py#L123'>docker/pythonpath_dev/superset_config.py:123:16:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
- <a href='https://github.com/apache/superset/blob/d1839697442157ace4beba2eaff45558a8d2ba08/scripts/cypress_run.py#L135'>scripts/cypress_run.py:135:18:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/apache/superset/blob/d1839697442157ace4beba2eaff45558a8d2ba08/scripts/cypress_run.py#L135'>scripts/cypress_run.py:135:18:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
- <a href='https://github.com/apache/superset/blob/d1839697442157ace4beba2eaff45558a8d2ba08/scripts/erd/erd.py#L173'>scripts/erd/erd.py:173:22:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/apache/superset/blob/d1839697442157ace4beba2eaff45558a8d2ba08/scripts/erd/erd.py#L173'>scripts/erd/erd.py:173:22:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
... 17 additional changes omitted for rule PTH120
- <a href='https://github.com/apache/superset/blob/d1839697442157ace4beba2eaff45558a8d2ba08/superset/db_engine_specs/hive.py#L82'>superset/db_engine_specs/hive.py:82:50:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/apache/superset/blob/d1839697442157ace4beba2eaff45558a8d2ba08/superset/db_engine_specs/hive.py#L82'>superset/db_engine_specs/hive.py:82:50:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
- <a href='https://github.com/apache/superset/blob/d1839697442157ace4beba2eaff45558a8d2ba08/superset/utils/core.py#L776'>superset/utils/core.py:776:20:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/apache/superset/blob/d1839697442157ace4beba2eaff45558a8d2ba08/superset/utils/core.py#L776'>superset/utils/core.py:776:20:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +0 -142 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/export_csv/main.py#L15'>examples/server/app/export_csv/main.py:15:23:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/export_csv/main.py#L15'>examples/server/app/export_csv/main.py:15:23:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/movies/main.py#L28'>examples/server/app/movies/main.py:28:16:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/movies/main.py#L28'>examples/server/app/movies/main.py:28:16:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/simple_hdf5/main.py#L17'>examples/server/app/simple_hdf5/main.py:17:11:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/simple_hdf5/main.py#L17'>examples/server/app/simple_hdf5/main.py:17:11:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
... 81 additional changes omitted for rule PTH120
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/sri.py#L30'>scripts/sri.py:30:24:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/sri.py#L30'>scripts/sri.py:30:24:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L169'>src/bokeh/application/handlers/code.py:169:31:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L169'>src/bokeh/application/handlers/code.py:169:31:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
... 132 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -0 violations, +0 -8 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/a8f1ddf414209fd61b447c4d93f4eb32db4a9508/examples/bulk_import/example_bulkinsert_csv.py#L305'>examples/bulk_import/example_bulkinsert_csv.py:305:74:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/milvus-io/pymilvus/blob/a8f1ddf414209fd61b447c4d93f4eb32db4a9508/examples/bulk_import/example_bulkinsert_csv.py#L305'>examples/bulk_import/example_bulkinsert_csv.py:305:74:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
- <a href='https://github.com/milvus-io/pymilvus/blob/a8f1ddf414209fd61b447c4d93f4eb32db4a9508/examples/bulk_import/example_bulkinsert_json.py#L347'>examples/bulk_import/example_bulkinsert_json.py:347:74:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/milvus-io/pymilvus/blob/a8f1ddf414209fd61b447c4d93f4eb32db4a9508/examples/bulk_import/example_bulkinsert_json.py#L347'>examples/bulk_import/example_bulkinsert_json.py:347:74:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
- <a href='https://github.com/milvus-io/pymilvus/blob/a8f1ddf414209fd61b447c4d93f4eb32db4a9508/examples/bulk_import/example_bulkinsert_numpy.py#L349'>examples/bulk_import/example_bulkinsert_numpy.py:349:74:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/milvus-io/pymilvus/blob/a8f1ddf414209fd61b447c4d93f4eb32db4a9508/examples/bulk_import/example_bulkinsert_numpy.py#L349'>examples/bulk_import/example_bulkinsert_numpy.py:349:74:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
- <a href='https://github.com/milvus-io/pymilvus/blob/a8f1ddf414209fd61b447c4d93f4eb32db4a9508/examples/bulk_import/example_bulkinsert_parquet.py#L207'>examples/bulk_import/example_bulkinsert_parquet.py:207:20:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/milvus-io/pymilvus/blob/a8f1ddf414209fd61b447c4d93f4eb32db4a9508/examples/bulk_import/example_bulkinsert_parquet.py#L207'>examples/bulk_import/example_bulkinsert_parquet.py:207:20:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +0 -320 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/docs/conf.py#L11'>docs/conf.py:11:49:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/docs/conf.py#L11'>docs/conf.py:11:49:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/manage.py#L7'>manage.py:7:12:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/manage.py#L7'>manage.py:7:12:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/scripts/lib/check_rabbitmq_queue.py#L10'>scripts/lib/check_rabbitmq_queue.py:10:14:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/scripts/lib/check_rabbitmq_queue.py#L10'>scripts/lib/check_rabbitmq_queue.py:10:14:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/scripts/lib/check_rabbitmq_queue.py#L10'>scripts/lib/check_rabbitmq_queue.py:10:30:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/scripts/lib/check_rabbitmq_queue.py#L10'>scripts/lib/check_rabbitmq_queue.py:10:30:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/scripts/lib/check_rabbitmq_queue.py#L10'>scripts/lib/check_rabbitmq_queue.py:10:46:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
+ <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/scripts/lib/check_rabbitmq_queue.py#L10'>scripts/lib/check_rabbitmq_queue.py:10:46:</a> PTH120 `os.path.dirname()` should be replaced by `Path.parent`
- <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/scripts/lib/clean_emoji_cache.py#L27'>scripts/lib/clean_emoji_cache.py:27:25:</a> PTH120 [*] `os.path.dirname()` should be replaced by `Path.parent`
... 248 additional changes omitted for rule PTH120
- <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/scripts/lib/zulip_tools.py#L566'>scripts/lib/zulip_tools.py:566:16:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/scripts/lib/zulip_tools.py#L566'>scripts/lib/zulip_tools.py:566:16:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
- <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/tools/setup/generate_bots_integrations_static_files.py#L64'>tools/setup/generate_bots_integrations_static_files.py:64:65:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/tools/setup/generate_bots_integrations_static_files.py#L64'>tools/setup/generate_bots_integrations_static_files.py:64:65:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
- <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/tools/setup/generate_bots_integrations_static_files.py#L69'>tools/setup/generate_bots_integrations_static_files.py:69:60:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/tools/setup/generate_bots_integrations_static_files.py#L69'>tools/setup/generate_bots_integrations_static_files.py:69:60:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
- <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/zerver/data_import/slack.py#L885'>zerver/data_import/slack.py:885:24:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
+ <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/zerver/data_import/slack.py#L885'>zerver/data_import/slack.py:885:24:</a> PTH119 `os.path.basename()` should be replaced by `Path.name`
- <a href='https://github.com/zulip/zulip/blob/62093ee37d61b862bd25e0ba5b119dee1107aacb/zerver/lib/compatibility.py#L15'>zerver/lib/compatibility.py:15:17:</a> PTH119 [*] `os.path.basename()` should be replaced by `Path.name`
... 300 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH120 | 570 | 0 | 0 | 0 | 570 |
| PTH119 | 210 | 0 | 0 | 0 | 210 |

</p>
</details>




---

_Comment by @dscorbett on 2025-08-27 19:32_

Another normalization that could be worth mentioning is that `pathlib` eliminates `"."` components, as in `"a/./b"` → `"a/b"`.

---

_Review requested from @dylwil3 by @dylwil3 on 2025-08-29 19:24_

---

_Label `fixes` added by @dylwil3 on 2025-09-01 20:47_

---

_Label `preview` added by @dylwil3 on 2025-09-01 20:47_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_dirname.rs`:40 on 2025-09-01 20:48_

```suggestion
/// which can alter the result compared to `os.path.dirname`. For example, this normalization:
```


---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_basename.rs`:100 on 2025-09-01 20:51_

Can we move this shared code somewhere? Could this logic be moved to the body of `check_os_pathlib_single_arg_calls`?

---

_@dylwil3 requested changes on 2025-09-01 20:51_

Thank you! Small tweak in wording and a question about factoring out some shared code

---

_@chirizxc reviewed on 2025-09-02 08:12_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_basename.rs`:100 on 2025-09-02 08:12_

yes

---

_Comment by @chirizxc on 2025-09-02 12:31_

@dylwil3 idk if we could use `#[allow(clippy::too_many_arguments)]`, but this way we don't duplicate the fn`s for safe and unsafe fixes

---

_Comment by @dylwil3 on 2025-09-02 12:58_

> @dylwil3 idk if we could use `#[allow(clippy::too_many_arguments)]`, but this way we don't duplicate the fn`s for safe and unsafe fixes

Clippy doesn't seem to complain locally if I remove the `allow` - we'll see if CI passes 😄 

---

_Comment by @chirizxc on 2025-09-02 13:09_

hmm, it's weird, i had something like 7>6, but if CI passes, it's okay!

---

_@dylwil3 approved on 2025-09-02 15:54_

Thank you!

---

_Merged by @dylwil3 on 2025-09-02 15:55_

---

_Closed by @dylwil3 on 2025-09-02 15:55_

---

_Branch deleted on 2025-09-02 16:22_

---
