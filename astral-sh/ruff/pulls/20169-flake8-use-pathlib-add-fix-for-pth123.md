```yaml
number: 20169
title: "[`flake8-use-pathlib`] Add fix for `PTH123`"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: pth123
created_at: 2025-08-30T20:28:42Z
updated_at: 2025-09-17T20:27:40Z
url: https://github.com/astral-sh/ruff/pull/20169
synced_at: 2026-01-12T15:56:56Z
```

# [`flake8-use-pathlib`] Add fix for `PTH123`

---

_@chirizxc_

## Summary
Part of https://github.com/astral-sh/ruff/issues/2331

## Test Plan
`cargo nextest run flake8_use_pathlib`


---

_Comment by @github-actions[bot] on 2025-08-30 20:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +1684 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +812 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/api_fastapi/auth/managers/simple/simple_auth_manager.py#L107'>airflow-core/src/airflow/api_fastapi/auth/managers/simple/simple_auth_manager.py:107:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/api_fastapi/auth/managers/simple/simple_auth_manager.py#L107'>airflow-core/src/airflow/api_fastapi/auth/managers/simple/simple_auth_manager.py:107:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/api_fastapi/auth/managers/simple/simple_auth_manager.py#L118'>airflow-core/src/airflow/api_fastapi/auth/managers/simple/simple_auth_manager.py:118:18:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/api_fastapi/auth/managers/simple/simple_auth_manager.py#L118'>airflow-core/src/airflow/api_fastapi/auth/managers/simple/simple_auth_manager.py:118:18:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/api_fastapi/auth/tokens.py#L166'>airflow-core/src/airflow/api_fastapi/auth/tokens.py:166:18:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/api_fastapi/auth/tokens.py#L166'>airflow-core/src/airflow/api_fastapi/auth/tokens.py:166:18:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/api_fastapi/auth/tokens.py#L370'>airflow-core/src/airflow/api_fastapi/auth/tokens.py:370:10:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/api_fastapi/auth/tokens.py#L370'>airflow-core/src/airflow/api_fastapi/auth/tokens.py:370:10:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/cli/commands/config_command.py#L1063'>airflow-core/src/airflow/cli/commands/config_command.py:1063:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/cli/commands/config_command.py#L1063'>airflow-core/src/airflow/cli/commands/config_command.py:1063:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/cli/commands/daemon_utils.py#L64'>airflow-core/src/airflow/cli/commands/daemon_utils.py:64:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/cli/commands/daemon_utils.py#L64'>airflow-core/src/airflow/cli/commands/daemon_utils.py:64:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/cli/commands/daemon_utils.py#L64'>airflow-core/src/airflow/cli/commands/daemon_utils.py:64:50:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/cli/commands/daemon_utils.py#L64'>airflow-core/src/airflow/cli/commands/daemon_utils.py:64:50:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/cli/commands/pool_command.py#L117'>airflow-core/src/airflow/cli/commands/pool_command.py:117:10:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/cli/commands/pool_command.py#L117'>airflow-core/src/airflow/cli/commands/pool_command.py:117:10:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/cli/commands/pool_command.py#L147'>airflow-core/src/airflow/cli/commands/pool_command.py:147:10:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/cli/commands/pool_command.py#L147'>airflow-core/src/airflow/cli/commands/pool_command.py:147:10:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/configuration.py#L194'>airflow-core/src/airflow/configuration.py:194:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/configuration.py#L194'>airflow-core/src/airflow/configuration.py:194:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/configuration.py#L2077'>airflow-core/src/airflow/configuration.py:2077:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/configuration.py#L2077'>airflow-core/src/airflow/configuration.py:2077:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/dag_processing/bundles/base.py#L168'>airflow-core/src/airflow/dag_processing/bundles/base.py:168:18:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/airflow/blob/1dc6736d584242e9b13b818c5af59626f12ab56d/airflow-core/src/airflow/dag_processing/bundles/base.py#L168'>airflow-core/src/airflow/dag_processing/bundles/base.py:168:18:</a> PTH123 `open()` should be replaced by `Path.open()`
... 788 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +64 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/3e554674ff3b994b8d73f42737e6fc89ee96c22c/RELEASING/changelog.py#L391'>RELEASING/changelog.py:391:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/superset/blob/3e554674ff3b994b8d73f42737e6fc89ee96c22c/RELEASING/changelog.py#L391'>RELEASING/changelog.py:391:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/apache/superset/blob/3e554674ff3b994b8d73f42737e6fc89ee96c22c/RELEASING/generate_email.py#L51'>RELEASING/generate_email.py:51:32:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/superset/blob/3e554674ff3b994b8d73f42737e6fc89ee96c22c/RELEASING/generate_email.py#L51'>RELEASING/generate_email.py:51:32:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/apache/superset/blob/3e554674ff3b994b8d73f42737e6fc89ee96c22c/RELEASING/verify_release.py#L39'>RELEASING/verify_release.py:39:10:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/superset/blob/3e554674ff3b994b8d73f42737e6fc89ee96c22c/RELEASING/verify_release.py#L39'>RELEASING/verify_release.py:39:10:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/apache/superset/blob/3e554674ff3b994b8d73f42737e6fc89ee96c22c/scripts/change_detector.py#L142'>scripts/change_detector.py:142:10:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/superset/blob/3e554674ff3b994b8d73f42737e6fc89ee96c22c/scripts/change_detector.py#L142'>scripts/change_detector.py:142:10:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/apache/superset/blob/3e554674ff3b994b8d73f42737e6fc89ee96c22c/scripts/erd/erd.py#L179'>scripts/erd/erd.py:179:10:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/apache/superset/blob/3e554674ff3b994b8d73f42737e6fc89ee96c22c/scripts/erd/erd.py#L179'>scripts/erd/erd.py:179:10:</a> PTH123 `open()` should be replaced by `Path.open()`
... 54 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +188 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/conf.py#L69'>docs/bokeh/source/conf.py:69:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/conf.py#L69'>docs/bokeh/source/conf.py:69:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/basic_plot.py#L42'>examples/models/basic_plot.py:42:10:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/basic_plot.py#L42'>examples/models/basic_plot.py:42:10:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/buttons.py#L75'>examples/models/buttons.py:75:10:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/buttons.py#L75'>examples/models/buttons.py:75:10:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/calendars.py#L104'>examples/models/calendars.py:104:10:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/calendars.py#L104'>examples/models/calendars.py:104:10:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/colors.py#L69'>examples/models/colors.py:69:10:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/colors.py#L69'>examples/models/colors.py:69:10:</a> PTH123 `open()` should be replaced by `Path.open()`
... 178 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +36 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch/executions.py#L56'>src/latch/executions.py:56:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch/executions.py#L56'>src/latch/executions.py:56:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch/ldata/_transfer/download.py#L229'>src/latch/ldata/_transfer/download.py:229:10:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch/ldata/_transfer/download.py#L229'>src/latch/ldata/_transfer/download.py:229:10:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch/ldata/_transfer/upload.py#L329'>src/latch/ldata/_transfer/upload.py:329:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch/ldata/_transfer/upload.py#L329'>src/latch/ldata/_transfer/upload.py:329:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch/ldata/_transfer/upload.py#L424'>src/latch/ldata/_transfer/upload.py:424:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch/ldata/_transfer/upload.py#L424'>src/latch/ldata/_transfer/upload.py:424:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch_cli/exceptions/traceback.py#L24'>src/latch_cli/exceptions/traceback.py:24:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch_cli/exceptions/traceback.py#L24'>src/latch_cli/exceptions/traceback.py:24:14:</a> PTH123 `open()` should be replaced by `Path.open()`
... 26 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -0 violations, +8 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/cabc1de290e817bba43ea4bf56b7ad7c3f7144b6/examples/orm_deprecated/bulk_import/example_bulkinsert_csv.py#L114'>examples/orm_deprecated/bulk_import/example_bulkinsert_csv.py:114:10:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/milvus-io/pymilvus/blob/cabc1de290e817bba43ea4bf56b7ad7c3f7144b6/examples/orm_deprecated/bulk_import/example_bulkinsert_csv.py#L114'>examples/orm_deprecated/bulk_import/example_bulkinsert_csv.py:114:10:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/milvus-io/pymilvus/blob/cabc1de290e817bba43ea4bf56b7ad7c3f7144b6/examples/orm_deprecated/bulk_import/example_bulkinsert_json.py#L147'>examples/orm_deprecated/bulk_import/example_bulkinsert_json.py:147:10:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/milvus-io/pymilvus/blob/cabc1de290e817bba43ea4bf56b7ad7c3f7144b6/examples/orm_deprecated/bulk_import/example_bulkinsert_json.py#L147'>examples/orm_deprecated/bulk_import/example_bulkinsert_json.py:147:10:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/milvus-io/pymilvus/blob/cabc1de290e817bba43ea4bf56b7ad7c3f7144b6/examples/orm_deprecated/bulk_import/example_bulkinsert_withfunction.py#L116'>examples/orm_deprecated/bulk_import/example_bulkinsert_withfunction.py:116:10:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/milvus-io/pymilvus/blob/cabc1de290e817bba43ea4bf56b7ad7c3f7144b6/examples/orm_deprecated/bulk_import/example_bulkinsert_withfunction.py#L116'>examples/orm_deprecated/bulk_import/example_bulkinsert_withfunction.py:116:10:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/milvus-io/pymilvus/blob/cabc1de290e817bba43ea4bf56b7ad7c3f7144b6/examples/orm_deprecated/bulk_import/example_bulkwriter.py#L210'>examples/orm_deprecated/bulk_import/example_bulkwriter.py:210:18:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/milvus-io/pymilvus/blob/cabc1de290e817bba43ea4bf56b7ad7c3f7144b6/examples/orm_deprecated/bulk_import/example_bulkwriter.py#L210'>examples/orm_deprecated/bulk_import/example_bulkwriter.py:210:18:</a> PTH123 `open()` should be replaced by `Path.open()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +576 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/analytics/management/commands/populate_analytics_db.py#L146'>analytics/management/commands/populate_analytics_db.py:146:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/analytics/management/commands/populate_analytics_db.py#L146'>analytics/management/commands/populate_analytics_db.py:146:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/corporate/tests/test_stripe.py#L156'>corporate/tests/test_stripe.py:156:18:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/corporate/tests/test_stripe.py#L156'>corporate/tests/test_stripe.py:156:18:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/corporate/tests/test_stripe.py#L169'>corporate/tests/test_stripe.py:169:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/corporate/tests/test_stripe.py#L169'>corporate/tests/test_stripe.py:169:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/corporate/tests/test_stripe.py#L187'>corporate/tests/test_stripe.py:187:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/corporate/tests/test_stripe.py#L187'>corporate/tests/test_stripe.py:187:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/corporate/tests/test_stripe.py#L280'>corporate/tests/test_stripe.py:280:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/corporate/tests/test_stripe.py#L280'>corporate/tests/test_stripe.py:280:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/corporate/tests/test_stripe.py#L306'>corporate/tests/test_stripe.py:306:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/corporate/tests/test_stripe.py#L306'>corporate/tests/test_stripe.py:306:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/corporate/views/portico.py#L286'>corporate/views/portico.py:286:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/corporate/views/portico.py#L286'>corporate/views/portico.py:286:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/scripts/lib/check_rabbitmq_queue.py#L194'>scripts/lib/check_rabbitmq_queue.py:194:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
- <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/scripts/lib/check_rabbitmq_queue.py#L194'>scripts/lib/check_rabbitmq_queue.py:194:14:</a> PTH123 `open()` should be replaced by `Path.open()`
+ <a href='https://github.com/zulip/zulip/blob/421637ce31d3bde260af9f2a67d361abbb19a059/scripts/lib/node_cache.py#L15'>scripts/lib/node_cache.py:15:14:</a> PTH123 [*] `open()` should be replaced by `Path.open()`
... 559 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH123 | 1684 | 0 | 0 | 1684 | 0 |

</p>
</details>




---

_Converted to draft by @chirizxc on 2025-09-02 21:39_

---

_Comment by @chirizxc on 2025-09-03 12:56_

I'm not sure which tests should be included in the tests files, but i tested in this file:

The first line with `#noqa` so that they don't turn during the fix, the second line is needed to observe how the code is fixed, and the third line is what I expect to see after the fix, and then I compare lines 2 and 3 
```python
# ruff: noqa: SIM115, PLW1514, FBT003
import builtins
from pathlib import Path

_file = "file.txt"
_x = ("r+", -1)
r_plus = "r+"

builtins.open(file=_file)  # noqa: PTH123
builtins.open(file=_file)
Path(_file).open()

open(mode="wb", file=_file)  # noqa: PTH123
open(mode="wb", file=_file)
Path(_file).open(mode="wb")

open(mode="r+", buffering=-1, file=_file, encoding="utf-8")  # noqa: PTH123
open(mode="r+", buffering=-1, file=_file, encoding="utf-8")
Path(_file).open(mode="r+", buffering=-1, encoding="utf-8")

open(_file, "r+", -1, None, None, None)  # noqa: PTH123
open(_file, "r+", -1, None, None, None)
Path(_file).open("r+", -1, None, None, None)

open(_file, "r+", -1, None, None, None, closefd=True, opener=None)  # noqa: PTH123
open(_file, "r+", -1, None, None, None, closefd=True, opener=None)
Path(_file).open("r+", -1, None, None, None)

open(_file, mode="r+", buffering=-1, encoding=None, errors=None, newline=None)  # noqa: PTH123
open(_file, mode="r+", buffering=-1, encoding=None, errors=None, newline=None)
Path(_file).open(mode="r+", buffering=-1, encoding=None, errors=None, newline=None)


open(buffering=-      1, file=_file, encoding=         "utf-8")  # noqa: PTH123
open(buffering=-      1, file=_file, encoding=         "utf-8")
Path(_file).open(buffering=-1, encoding="utf-8")

open(_file, "r+", - 1, None, None, None, True, None)  # noqa: PTH123
open(_file, "r+", - 1, None, None, None, True, None)
Path(_file).open("r+", -1, None, None, None)

open(_file, "r+ ", -  1)  # noqa: PTH123
open(_file, "r+ ", -  1)
Path(_file).open("r+ ", -1)

open(_file, f"  {r_plus}      ", -  1)  # noqa: PTH123
open(_file, f"  {r_plus}      ", -  1)
Path(_file).open(f"  {r_plus}      ", -1)

# Only diagnostic
open()
open(_file, *_x)
open(_file, "r+", unknown=True)
open(_file, "r+", closefd=False)
open(_file, "r+", None, None, None, None, None, None, None)
```

---

_Marked ready for review by @chirizxc on 2025-09-03 12:58_

---

_Converted to draft by @chirizxc on 2025-09-05 16:12_

---

_Marked ready for review by @chirizxc on 2025-09-06 10:09_

---

_Label `fixes` added by @ntBre on 2025-09-11 18:20_

---

_Label `preview` added by @ntBre on 2025-09-11 18:20_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/builtin_open.rs`:111 on 2025-09-11 18:36_

Could we move this closer to where it's used? I could be missing another usage, but I only see this used right at the end of the function since we use `call.func.range()` in the `report_diagnostic` call.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/builtin_open.rs`:152 on 2025-09-11 18:52_

Which test cases does this affect? As I mentioned on your other PTH PR, we shouldn't alter the user's code unless it's necessary. This doesn't look necessary to me. If we drop whitespace from using `locator.slice(expr)`, that's okay, but this kind of intentional reformatting seems gratuitous.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__preview_import_from.py.snap`:788 on 2025-09-11 18:57_

I remember we discussed this before, and it wasn't a good fit for another rule, but could we use [`remove_argument`](https://github.com/astral-sh/ruff/blob/59c8fda3f8f3bf2cc5c1ae34e7ca9dbea4d0278f/crates/ruff_linter/src/fix/edits.rs#L207) here? This really looks like all we need to do is remove the `file` argument and replace `open` with `pathlib.Path(file).open`. Then we could leave more of the formatting untouched again.

It's not a huge deal, but it might lead to simpler code too if `remove_argument` can work.

---

_@ntBre reviewed on 2025-09-11 19:01_

Thanks! I think you could add those tests to a new file called `PTH123.py`. It looks like there's some overlap with the test you added in `import_from.py`, so we could either add the additional file or just add more cases there?

This looks good to me overall, just a few minor suggestions.

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__preview_import_from.py.snap`:788 on 2025-09-11 20:12_

it seems with `remove_argument` there will be more calls since we will be removing 1, 6, 7 arguments, would it be easier to discard them just by index, don't really know the best way to do it

---

_@chirizxc reviewed on 2025-09-11 20:12_

---

_@ntBre reviewed on 2025-09-11 20:13_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__preview_import_from.py.snap`:788 on 2025-09-11 20:13_

Oh right, I forgot we had to discard other default arguments. This seems fine again then!

---

_Comment by @chirizxc on 2025-09-11 20:36_

https://github.com/astral-sh/ruff/pull/20169/files#diff-c2de436c83da6c97764f1d9381108ba93899ba45b32492dc04cfb7d1a84db760R569

i don't think that's the way to fix it either 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/builtin_open.rs`:121 on 2025-09-12 20:02_

What is this check for? I thought the fix it removed looked okay, but I could be missing something.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/builtin_open.rs`:170 on 2025-09-12 20:03_

Could we just slice out the whole `kw` instead of `format!` here? Then we could remove the other `to_string` call too. I think `locator.slice(kw)` should work and preserve both the `arg` name and `value`.

---

_@ntBre reviewed on 2025-09-12 20:15_

Thanks! Just two more minor things, but I think this looks good otherwise.

---

_@chirizxc reviewed on 2025-09-12 20:45_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/builtin_open.rs`:121 on 2025-09-12 20:45_

https://github.com/astral-sh/ruff/pull/20169#issuecomment-3282552111

---

_@chirizxc reviewed on 2025-09-12 20:47_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/builtin_open.rs`:121 on 2025-09-12 20:47_

there after `open()` comes `.close()` it seems if after `open()` there is some call it is better not to suggest fix

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/builtin_open.rs`:121 on 2025-09-12 20:59_

I saw the other comment, but I don't think the GitHub link is working properly. It keeps showing me this snippet:

<img width="606" height="392" alt="image" src="https://github.com/user-attachments/assets/671f9c76-a477-4d39-bbdb-ce0f46871fea" />

I think it's fine to call `open().close()` as long as both functions return the same type, which I think is true? I tried checking the types with [ty](https://play.ty.dev/0b9e6089-f92a-4775-8cc4-6697d8f80298), but the number of overloads makes it a bit difficult to parse. I'd just assume it's okay based on the `pathlib` [docs](https://docs.python.org/3/library/pathlib.html#pathlib.Path.open):

> Open the file pointed to by the path, like the built-in `open()` function does

I don't think checking the grandparent expression type is that robust anyway, there are plenty of other erroneous ways to use the return type if it's not the same. I think all of the PTH fixes rely on this assumption.


---

_@ntBre reviewed on 2025-09-12 20:59_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/builtin_open.rs`:152 on 2025-09-17 19:31_

```suggestion
                    Some(locator.slice(expr.range()))
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/builtin_open.rs`:157 on 2025-09-17 19:32_

```suggestion
                    Some(locator.slice(kw))
```

---

_@ntBre approved on 2025-09-17 19:33_

Thank you!

---

_Merged by @ntBre on 2025-09-17 19:47_

---

_Closed by @ntBre on 2025-09-17 19:47_

---

_Comment by @chirizxc on 2025-09-17 20:27_

I guess I forgot about that, thanks for the change

---

_Branch deleted on 2025-09-17 20:27_

---
