```yaml
number: 17333
title: "[WIP] Cfg try"
type: pull_request
state: closed
author: dylwil3
labels: []
assignees: []
draft: true
base: main
head: cfg-try
created_at: 2025-04-10T12:32:38Z
updated_at: 2025-12-10T18:00:33Z
url: https://github.com/astral-sh/ruff/pull/17333
synced_at: 2026-01-10T16:42:11Z
```

# [WIP] Cfg try

---

_Pull request opened by @dylwil3 on 2025-04-10 12:32_

_No description provided._

---

_Comment by @github-actions[bot] on 2025-04-10 12:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+131 -193 violations, +0 -0 fixes in 18 projects; 37 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+38 -124 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py#L230'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/connections.py:230:9:</a> PLW0101 Unreachable code in `test_connection`
+ <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/src/airflow/cli/commands/standalone_command.py#L121'>airflow-core/src/airflow/cli/commands/standalone_command.py:121:13:</a> PLW0101 Unreachable code in `run`
+ <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/src/airflow/jobs/job.py#L347'>airflow-core/src/airflow/jobs/job.py:347:9:</a> PLW0101 Unreachable code in `run_job`
+ <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/src/airflow/jobs/triggerer_job_runner.py#L164'>airflow-core/src/airflow/jobs/triggerer_job_runner.py:164:13:</a> PLW0101 Unreachable code in `_execute`
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/src/airflow/triggers/base.py#L103'>airflow-core/src/airflow/triggers/base.py:103:9:</a> PLW0101 Unreachable code in `run`
+ <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/src/airflow/utils/cli.py#L111'>airflow-core/src/airflow/utils/cli.py:111:17:</a> PLW0101 Unreachable code in `wrapper`
... 34 additional changes omitted for rule PLW0101
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L115'>airflow-core/tests/unit/jobs/test_triggerer_job.py:115:20:</a> ANN001 Missing type annotation for function argument `session`
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L115'>airflow-core/tests/unit/jobs/test_triggerer_job.py:115:5:</a> ANN201 Missing return type annotation for public function `test_is_needed`
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L129'>airflow-core/tests/unit/jobs/test_triggerer_job.py:129:5:</a> ANN201 Missing return type annotation for public function `test_capacity_decode`
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L142'>airflow-core/tests/unit/jobs/test_triggerer_job.py:142:16:</a> PLR1714 Consider merging multiple comparisons: `job_runner.capacity in (input_str, 1000)`. Use a `set` if the elements are hashable.
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L142'>airflow-core/tests/unit/jobs/test_triggerer_job.py:142:75:</a> PLR2004 Magic value used in comparison, consider replacing `1000` with a constant variable
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L153'>airflow-core/tests/unit/jobs/test_triggerer_job.py:153:28:</a> PT011 `pytest.raises(ValueError)` is too broad, set the `match` parameter or use a more specific exception
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L158'>airflow-core/tests/unit/jobs/test_triggerer_job.py:158:24:</a> ANN001 Missing type annotation for function argument `mocker`
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L158'>airflow-core/tests/unit/jobs/test_triggerer_job.py:158:32:</a> ANN001 Missing type annotation for function argument `session`
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L158'>airflow-core/tests/unit/jobs/test_triggerer_job.py:158:5:</a> ANN201 Missing return type annotation for public function `supervisor_builder`
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L159'>airflow-core/tests/unit/jobs/test_triggerer_job.py:159:17:</a> ANN001 Missing type annotation for function argument `job`
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L159'>airflow-core/tests/unit/jobs/test_triggerer_job.py:159:9:</a> ANN202 Missing return type annotation for private function `builder`
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L160'>airflow-core/tests/unit/jobs/test_triggerer_job.py:160:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L189'>airflow-core/tests/unit/jobs/test_triggerer_job.py:189:51:</a> ANN001 Missing type annotation for function argument `session`
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L189'>airflow-core/tests/unit/jobs/test_triggerer_job.py:189:5:</a> ANN201 Missing return type annotation for public function `test_trigger_lifecycle`
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L197'>airflow-core/tests/unit/jobs/test_triggerer_job.py:197:16:</a> RUF059 Unpacked variable `run` is never used
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L197'>airflow-core/tests/unit/jobs/test_triggerer_job.py:197:34:</a> RUF059 Unpacked variable `task_instance` is never used
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L197'>airflow-core/tests/unit/jobs/test_triggerer_job.py:197:5:</a> RUF059 Unpacked variable `dag_model` is never used
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L1'>airflow-core/tests/unit/jobs/test_triggerer_job.py:1:1:</a> CPY001 Missing copyright notice at top of file
- <a href='https://github.com/apache/airflow/blob/225a439af186067aec104a66ee417b191cc60be9/airflow-core/tests/unit/jobs/test_triggerer_job.py#L206'>airflow-core/tests/unit/jobs/test_triggerer_job.py:206:13:</a> ANN202 Missing return type annotation for private function `send_msg_spy`
... 137 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/26ff734ef94658f622eef494555863c9f9f68aaa/superset/utils/decorators.py#L264'>superset/utils/decorators.py:264:17:</a> PLW0101 Unreachable code in `wrapped`
+ <a href='https://github.com/apache/superset/blob/26ff734ef94658f622eef494555863c9f9f68aaa/superset/utils/network.py#L34'>superset/utils/network.py:34:13:</a> PLW0101 Unreachable code in `is_port_open`
+ <a href='https://github.com/apache/superset/blob/26ff734ef94658f622eef494555863c9f9f68aaa/tests/unit_tests/utils/test_core.py#L1102'>tests/unit_tests/utils/test_core.py:1102:9:</a> PLW0101 Unreachable code in `test_get_stacktrace`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3c7943b96161c2a448c6b89874a3c6db42a2c42/samcli/commands/init/init_templates.py#L70'>samcli/commands/init/init_templates.py:70:13:</a> PLW0101 Unreachable code in `location_from_app_template`
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3c7943b96161c2a448c6b89874a3c6db42a2c42/samcli/local/apigw/local_apigw_service.py#L163'>samcli/local/apigw/local_apigw_service.py:163:17:</a> PLW0101 Unreachable code in `create`
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3c7943b96161c2a448c6b89874a3c6db42a2c42/tests/unit/commands/test_exceptions.py#L16'>tests/unit/commands/test_exceptions.py:16:13:</a> PLW0101 Unreachable code in `test_show_must_print_traceback_and_message`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L252'>src/bokeh/core/serialization.py:252:13:</a> PLW0101 Unreachable code in `encode`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L525'>src/bokeh/core/serialization.py:525:13:</a> PLW0101 Unreachable code in `decode`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+2 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/ae4def34fd74e9749fb2715cee1b2fe297876fe2/securedrop/tests/functional/conftest.py#L166'>securedrop/tests/functional/conftest.py:166:17:</a> PLW0101 Unreachable code in `spawn_sd_servers`
+ <a href='https://github.com/freedomofpress/securedrop/blob/ae4def34fd74e9749fb2715cee1b2fe297876fe2/securedrop/tests/functional/conftest.py#L176'>securedrop/tests/functional/conftest.py:176:17:</a> PLW0101 Unreachable code in `spawn_sd_servers`
- <a href='https://github.com/freedomofpress/securedrop/blob/ae4def34fd74e9749fb2715cee1b2fe297876fe2/securedrop/tests/test_worker.py#L4'>securedrop/tests/test_worker.py:4:8:</a> S404 `subprocess` module is possibly insecure
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/d7cd846627269b1f4c901cc3f0e380ee9024f796/ibis/backends/clickhouse/__init__.py#L554'>ibis/backends/clickhouse/__init__.py:554:13:</a> PLW0101 Unreachable code in `_get_schema_using_query`
+ <a href='https://github.com/ibis-project/ibis/blob/d7cd846627269b1f4c901cc3f0e380ee9024f796/ibis/backends/impala/__init__.py#L405'>ibis/backends/impala/__init__.py:405:13:</a> PLW0101 Unreachable code in `_get_schema_using_query`
+ <a href='https://github.com/ibis-project/ibis/blob/d7cd846627269b1f4c901cc3f0e380ee9024f796/ibis/backends/postgres/__init__.py#L594'>ibis/backends/postgres/__init__.py:594:13:</a> PLW0101 Unreachable code in `_get_schema_using_query`
+ <a href='https://github.com/ibis-project/ibis/blob/d7cd846627269b1f4c901cc3f0e380ee9024f796/ibis/backends/risingwave/__init__.py#L370'>ibis/backends/risingwave/__init__.py:370:13:</a> PLW0101 Unreachable code in `_get_schema_using_query`
+ <a href='https://github.com/ibis-project/ibis/blob/d7cd846627269b1f4c901cc3f0e380ee9024f796/ibis/common/collections.py#L88'>ibis/common/collections.py:88:13:</a> PLW0101 Unreachable code in `__iter__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+5 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/88fce67724123d4cabacffbc8a07ba97534a11ff/libs/core/langchain_core/_api/internal.py#L16'>libs/core/langchain_core/_api/internal.py:16:17:</a> PLW0101 Unreachable code in `is_caller_internal`
+ <a href='https://github.com/langchain-ai/langchain/blob/88fce67724123d4cabacffbc8a07ba97534a11ff/libs/core/langchain_core/output_parsers/pydantic.py#L37'>libs/core/langchain_core/output_parsers/pydantic.py:37:17:</a> PLW0101 Unreachable code in `_parse_obj`
+ <a href='https://github.com/langchain-ai/langchain/blob/88fce67724123d4cabacffbc8a07ba97534a11ff/libs/core/langchain_core/runnables/graph_png.py#L155'>libs/core/langchain_core/runnables/graph_png.py:155:13:</a> PLW0101 Unreachable code in `draw`
+ <a href='https://github.com/langchain-ai/langchain/blob/88fce67724123d4cabacffbc8a07ba97534a11ff/libs/core/langchain_core/tracers/log_stream.py#L438'>libs/core/langchain_core/tracers/log_stream.py:438:17:</a> PLW0101 Unreachable code in `_on_run_update`
+ <a href='https://github.com/langchain-ai/langchain/blob/88fce67724123d4cabacffbc8a07ba97534a11ff/libs/core/langchain_core/utils/iter.py#L83'>libs/core/langchain_core/utils/iter.py:83:9:</a> PLW0101 Unreachable code in `tee_peer`
- <a href='https://github.com/langchain-ai/langchain/blob/88fce67724123d4cabacffbc8a07ba97534a11ff/libs/core/tests/unit_tests/runnables/test_fallbacks.py#L300'>libs/core/tests/unit_tests/runnables/test_fallbacks.py:300:5:</a> PLW0101 Unreachable code in `_agenerate_immediate_error`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+5 -41 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/3484cfceabaf956e2d739bd0dd79b79079bbd42e/src/latch_cli/menus.py#L124'>src/latch_cli/menus.py:124:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/latchbio/latch/blob/3484cfceabaf956e2d739bd0dd79b79079bbd42e/src/latch_cli/menus.py#L124'>src/latch_cli/menus.py:124:5:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/latchbio/latch/blob/3484cfceabaf956e2d739bd0dd79b79079bbd42e/src/latch_cli/menus.py#L125'>src/latch_cli/menus.py:125:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/latchbio/latch/blob/3484cfceabaf956e2d739bd0dd79b79079bbd42e/src/latch_cli/menus.py#L125'>src/latch_cli/menus.py:125:5:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/latchbio/latch/blob/3484cfceabaf956e2d739bd0dd79b79079bbd42e/src/latch_cli/menus.py#L128'>src/latch_cli/menus.py:128:5:</a> D205 1 blank line required between summary line and description
- <a href='https://github.com/latchbio/latch/blob/3484cfceabaf956e2d739bd0dd79b79079bbd42e/src/latch_cli/menus.py#L128'>src/latch_cli/menus.py:128:5:</a> D212 [*] Multi-line docstring summary should start at the first line
- <a href='https://github.com/latchbio/latch/blob/3484cfceabaf956e2d739bd0dd79b79079bbd42e/src/latch_cli/menus.py#L13'>src/latch_cli/menus.py:13:17:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/latchbio/latch/blob/3484cfceabaf956e2d739bd0dd79b79079bbd42e/src/latch_cli/menus.py#L151'>src/latch_cli/menus.py:151:5:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/latchbio/latch/blob/3484cfceabaf956e2d739bd0dd79b79079bbd42e/src/latch_cli/menus.py#L151'>src/latch_cli/menus.py:151:5:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/latchbio/latch/blob/3484cfceabaf956e2d739bd0dd79b79079bbd42e/src/latch_cli/menus.py#L152'>src/latch_cli/menus.py:152:5:</a> FBT001 Boolean-typed positional argument in function definition
... 36 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+2 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/7a5e3f4982cda67d5f1e891bb85a98c038303057/examples/iterator/iterator.py#L64'>examples/iterator/iterator.py:64:9:</a> PLW0101 Unreachable code in `external_filter_func`
+ <a href='https://github.com/milvus-io/pymilvus/blob/7a5e3f4982cda67d5f1e891bb85a98c038303057/examples/iterator/iterator.py#L65'>examples/iterator/iterator.py:65:9:</a> PLW0101 Unreachable code in `external_filter_func`
+ <a href='https://github.com/milvus-io/pymilvus/blob/7a5e3f4982cda67d5f1e891bb85a98c038303057/pymilvus/decorators.py#L86'>pymilvus/decorators.py:86:21:</a> PLW0101 Unreachable code in `handler`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/a393c31931af4fc69ea0006fd851eeb00175be3c/pandas/io/parquet.py#L280'>pandas/io/parquet.py:280:13:</a> PLW0101 Unreachable code in `read`
+ <a href='https://github.com/pandas-dev/pandas/blob/a393c31931af4fc69ea0006fd851eeb00175be3c/pandas/io/parsers/python_parser.py#L1314'>pandas/io/parsers/python_parser.py:1314:25:</a> PLW0101 Unreachable code in `_get_lines`
- <a href='https://github.com/pandas-dev/pandas/blob/a393c31931af4fc69ea0006fd851eeb00175be3c/pandas/util/_exceptions.py#L32'>pandas/util/_exceptions.py:32:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/99183d68ac491ca04c562a5de91c7287b1bd1728/bin/make_dependency_update_pr.py#L50'>bin/make_dependency_update_pr.py:50:13:</a> PLW0101 Unreachable code in `main`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/cf57bd264a65c3f58f8eb104be581827e65f2ff3/rotkehlchen/globaldb/manual_price_oracles.py#L52'>rotkehlchen/globaldb/manual_price_oracles.py:52:17:</a> PLW0101 Unreachable code in `query_current_price`
+ <a href='https://github.com/rotki/rotki/blob/cf57bd264a65c3f58f8eb104be581827e65f2ff3/rotkehlchen/tasks/manager.py#L489'>rotkehlchen/tasks/manager.py:489:17:</a> PLW0101 Unreachable code in `_maybe_check_premium_status`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/765af303efdd47820a25ad11d05ffff778c8cd71/src/scikit_build_core/settings/_load_provider.py#L61'>src/scikit_build_core/settings/_load_provider.py:61:9:</a> PLW0101 Unreachable code in `load_provider`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+16 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/d34bdf8af5b6190df95d837ff2cccaf39f973092/zerver/actions/message_flags.py#L93'>zerver/actions/message_flags.py:93:5:</a> PLW0101 Unreachable code in `do_mark_all_as_read`
+ <a href='https://github.com/zulip/zulip/blob/d34bdf8af5b6190df95d837ff2cccaf39f973092/zerver/lib/logging_util.py#L96'>zerver/lib/logging_util.py:96:13:</a> PLW0101 Unreachable code in `filter`
+ <a href='https://github.com/zulip/zulip/blob/d34bdf8af5b6190df95d837ff2cccaf39f973092/zerver/lib/markdown/__init__.py#L2829'>zerver/lib/markdown/__init__.py:2829:9:</a> PLW0101 Unreachable code in `do_convert`
+ <a href='https://github.com/zulip/zulip/blob/d34bdf8af5b6190df95d837ff2cccaf39f973092/zerver/lib/streams.py#L887'>zerver/lib/streams.py:887:9:</a> PLW0101 Unreachable code in `check_stream_name_available`
+ <a href='https://github.com/zulip/zulip/blob/d34bdf8af5b6190df95d837ff2cccaf39f973092/zerver/lib/test_helpers.py#L188'>zerver/lib/test_helpers.py:188:13:</a> PLW0101 Unreachable code in `cursor_execute`
+ <a href='https://github.com/zulip/zulip/blob/d34bdf8af5b6190df95d837ff2cccaf39f973092/zerver/lib/test_helpers.py#L206'>zerver/lib/test_helpers.py:206:13:</a> PLW0101 Unreachable code in `cursor_executemany`
+ <a href='https://github.com/zulip/zulip/blob/d34bdf8af5b6190df95d837ff2cccaf39f973092/zerver/management/commands/deliver_scheduled_emails.py#L56'>zerver/management/commands/deliver_scheduled_emails.py:56:13:</a> PLW0101 Unreachable code in `handle`
+ <a href='https://github.com/zulip/zulip/blob/d34bdf8af5b6190df95d837ff2cccaf39f973092/zerver/management/commands/deliver_scheduled_messages.py#L35'>zerver/management/commands/deliver_scheduled_messages.py:35:13:</a> PLW0101 Unreachable code in `handle`
+ <a href='https://github.com/zulip/zulip/blob/d34bdf8af5b6190df95d837ff2cccaf39f973092/zerver/middleware.py#L418'>zerver/middleware.py:418:13:</a> PLW0101 Unreachable code in `process_exception`
+ <a href='https://github.com/zulip/zulip/blob/d34bdf8af5b6190df95d837ff2cccaf39f973092/zerver/tests/test_openapi.py#L288'>zerver/tests/test_openapi.py:288:13:</a> PLW0101 Unreachable code in `ensure_no_documentation_if_intentionally_undocumented`
... 6 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (40 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0101 | 134 | 131 | 3 | 0 | 0 |
| ANN001 | 45 | 0 | 45 | 0 | 0 |
| PLC0415 | 23 | 0 | 23 | 0 | 0 |
| ANN201 | 16 | 0 | 16 | 0 | 0 |
| SLF001 | 10 | 0 | 10 | 0 | 0 |
| COM812 | 9 | 0 | 9 | 0 | 0 |
| ARG002 | 7 | 0 | 7 | 0 | 0 |
| PLR6301 | 6 | 0 | 6 | 0 | 0 |
| FBT001 | 6 | 0 | 6 | 0 | 0 |
| FBT002 | 6 | 0 | 6 | 0 | 0 |
| D212 | 6 | 0 | 6 | 0 | 0 |
| ANN202 | 4 | 0 | 4 | 0 | 0 |
| RUF059 | 4 | 0 | 4 | 0 | 0 |
| ANN003 | 4 | 0 | 4 | 0 | 0 |
| RUF029 | 4 | 0 | 4 | 0 | 0 |
| D205 | 4 | 0 | 4 | 0 | 0 |
| ANN204 | 3 | 0 | 3 | 0 | 0 |
| UP035 | 3 | 0 | 3 | 0 | 0 |
| PLR2004 | 2 | 0 | 2 | 0 | 0 |
| ANN002 | 2 | 0 | 2 | 0 | 0 |
| ERA001 | 2 | 0 | 2 | 0 | 0 |
| FBT003 | 2 | 0 | 2 | 0 | 0 |
| ARG001 | 2 | 0 | 2 | 0 | 0 |
| PLR6201 | 2 | 0 | 2 | 0 | 0 |
| RET505 | 2 | 0 | 2 | 0 | 0 |
| D200 | 2 | 0 | 2 | 0 | 0 |
| PLR1714 | 1 | 0 | 1 | 0 | 0 |
| PT011 | 1 | 0 | 1 | 0 | 0 |
| CPY001 | 1 | 0 | 1 | 0 | 0 |
| TC003 | 1 | 0 | 1 | 0 | 0 |
| S404 | 1 | 0 | 1 | 0 | 0 |
| PERF402 | 1 | 0 | 1 | 0 | 0 |
| F541 | 1 | 0 | 1 | 0 | 0 |
| F401 | 1 | 0 | 1 | 0 | 0 |
| PLR0402 | 1 | 0 | 1 | 0 | 0 |
| PLR6104 | 1 | 0 | 1 | 0 | 0 |
| PLR1702 | 1 | 0 | 1 | 0 | 0 |
| PLR0904 | 1 | 0 | 1 | 0 | 0 |
| PLW3201 | 1 | 0 | 1 | 0 | 0 |
| RUF056 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Closed by @dylwil3 on 2025-12-10 18:00_

---
