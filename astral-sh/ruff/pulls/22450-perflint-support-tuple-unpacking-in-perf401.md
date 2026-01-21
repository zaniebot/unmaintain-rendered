```yaml
number: 22450
title: "perflint: support tuple unpacking in PERF401"
type: pull_request
state: open
author: Jkhall81
labels:
  - rule
assignees: []
base: main
head: fix/21648-perf401-unpacking
created_at: 2026-01-08T02:13:07Z
updated_at: 2026-01-21T01:08:25Z
url: https://github.com/astral-sh/ruff/pull/22450
synced_at: 2026-01-21T02:01:17Z
```

# perflint: support tuple unpacking in PERF401

---

_@Jkhall81_

## Summary

This PR fixes a false negative in `PERF401` where the rule failed to identify `for` loops using tuple or list unpacking (e.g., `for x, y in [(1, 2)]`).

The rule now correctly identifies `Expr::Tuple` and `Expr::List` targets, performs semantic safety checks on all identified bindings, and suggests a list comprehension fix that preserves the unpacking logic.

Closes #21648

## Test Plan

- Added a new test case to `PERF401.py` covering tuple unpacking.


---

_Comment by @astral-sh-bot[bot] on 2026-01-08 02:24_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+119 -0 violations, +0 -0 fixes in 13 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+27 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/src/airflow/cli/commands/connection_command.py#L154'>airflow-core/src/airflow/cli/commands/connection_command.py:154:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/src/airflow/cli/commands/dag_command.py#L483'>airflow-core/src/airflow/cli/commands/dag_command.py:483:25:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/src/airflow/cli/commands/dag_command.py#L487'>airflow-core/src/airflow/cli/commands/dag_command.py:487:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/src/airflow/dag_processing/manager.py#L775'>airflow-core/src/airflow/dag_processing/manager.py:775:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/src/airflow/models/taskinstance.py#L1761'>airflow-core/src/airflow/models/taskinstance.py:1761:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/src/airflow/models/taskinstance.py#L1771'>airflow-core/src/airflow/models/taskinstance.py:1771:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/tests/conftest.py#L86'>airflow-core/tests/conftest.py:86:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L85'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:85:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/dev/breeze/src/airflow_breeze/utils/parallel.py#L247'>dev/breeze/src/airflow_breeze/utils/parallel.py:247:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/dev/stats/get_important_pr_candidates.py#L121'>dev/stats/get_important_pr_candidates.py:121:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/dev/stats/get_important_pr_candidates.py#L275'>dev/stats/get_important_pr_candidates.py:275:21:</a> PERF401 Use a list comprehension to create a transformed list
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/commands/importers/v1/__init__.py#L146'>superset/commands/importers/v1/__init__.py:146:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/daos/tag.py#L368'>superset/daos/tag.py:368:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/db_engine_specs/presto.py#L489'>superset/db_engine_specs/presto.py:489:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/mcp_service/chart/tool/get_chart_preview.py#L1442'>superset/mcp_service/chart/tool/get_chart_preview.py:1442:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/mcp_service/chart/tool/get_chart_preview.py#L1482'>superset/mcp_service/chart/tool/get_chart_preview.py:1482:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/mcp_service/screenshot/webdriver_pool.py#L279'>superset/mcp_service/screenshot/webdriver_pool.py:279:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/reports/notifications/webhook.py#L88'>superset/reports/notifications/webhook.py:88:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/viz.py#L894'>superset/viz.py:894:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/tests/integration_tests/security/migrate_roles_tests.py#L44'>tests/integration_tests/security/migrate_roles_tests.py:44:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/tests/integration_tests/security_tests.py#L1677'>tests/integration_tests/security_tests.py:1677:21:</a> PERF401 Use `list.extend` to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L813'>src/bokeh/core/has_props.py:813:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_sampledata_xref.py#L206'>src/bokeh/sphinxext/bokeh_sampledata_xref.py:206:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_no_request_host.py#L48'>tests/codebase/test_no_request_host.py:48:17:</a> PERF401 Use `list.extend` to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/64aed03898c543ede8ad2b7ce039b50d25861d33/ibis/backends/databricks/__init__.py#L187'>ibis/backends/databricks/__init__.py:187:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/ibis-project/ibis/blob/64aed03898c543ede8ad2b7ce039b50d25861d33/ibis/backends/flink/ddl.py#L34'>ibis/backends/flink/ddl.py:34:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/ibis-project/ibis/blob/64aed03898c543ede8ad2b7ce039b50d25861d33/ibis/backends/impala/ddl.py#L33'>ibis/backends/impala/ddl.py:33:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/ibis-project/ibis/blob/64aed03898c543ede8ad2b7ce039b50d25861d33/ibis/backends/risingwave/__init__.py#L56'>ibis/backends/risingwave/__init__.py:56:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/ibis-project/ibis/blob/64aed03898c543ede8ad2b7ce039b50d25861d33/ibis/expr/format.py#L194'>ibis/expr/format.py:194:13:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/cli/langchain_cli/namespaces/migrate/generate/generic.py#L68'>libs/cli/langchain_cli/namespaces/migrate/generate/generic.py:68:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/cli/langchain_cli/namespaces/migrate/generate/utils.py#L91'>libs/cli/langchain_cli/namespaces/migrate/generate/utils.py:91:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/core/langchain_core/messages/ai.py#L375'>libs/core/langchain_core/messages/ai.py:375:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/core/langchain_core/tools/base.py#L599'>libs/core/langchain_core/tools/base.py:599:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/core/langchain_core/utils/function_calling.py#L689'>libs/core/langchain_core/utils/function_calling.py:689:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/core/tests/unit_tests/runnables/test_concurrency.py#L73'>libs/core/tests/unit_tests/runnables/test_concurrency.py:73:9:</a> PERF401 Use an async list comprehension to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/langchain/langchain_classic/retrievers/merger_retriever.py#L117'>libs/langchain/langchain_classic/retrievers/merger_retriever.py:117:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/langchain/langchain_classic/retrievers/merger_retriever.py#L82'>libs/langchain/langchain_classic/retrievers/merger_retriever.py:82:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/langchain_v1/langchain/agents/middleware/file_search.py#L370'>libs/langchain_v1/langchain/agents/middleware/file_search.py:370:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/langchain_v1/tests/unit_tests/agents/test_agent_name.py#L122'>libs/langchain_v1/tests/unit_tests/agents/test_agent_name.py:122:13:</a> PERF401 Use a list comprehension to create a transformed list
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/functions/operators.py#L186'>src/latch/functions/operators.py:186:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/download.py#L135'>src/latch/ldata/_transfer/download.py:135:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/upload.py#L216'>src/latch/ldata/_transfer/upload.py:216:29:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/exceptions/traceback.py#L30'>src/latch_cli/exceptions/traceback.py:30:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/services/sync.py#L27'>src/latch_cli/services/sync.py:27:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/services/workspace.py#L26'>src/latch_cli/services/workspace.py:26:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/snakemake/config/utils.py#L298'>src/latch_cli/snakemake/config/utils.py:298:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/snakemake/workflow.py#L1532'>src/latch_cli/snakemake/workflow.py:1532:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/snakemake/workflow.py#L387'>src/latch_cli/snakemake/workflow.py:387:13:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+15 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/dev/check_function_signatures.py#L65'>dev/check_function_signatures.py:65:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/dev/check_function_signatures.py#L97'>dev/check_function_signatures.py:97:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/dev/remove_experimental_decorators.py#L121'>dev/remove_experimental_decorators.py:121:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/dev/update_changelog.py#L119'>dev/update_changelog.py:119:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/docs/api_reference/source/conf.py#L421'>docs/api_reference/source/conf.py:421:17:</a> PERF401 Use `list.extend` to create a transformed list
+ examples/evaluation/rag-evaluation.ipynb:cell 9:4:9: PERF401 Use a list comprehension to create a transformed list
+ examples/llms/RAG/question-generation-retrieval-evaluation.ipynb:cell 16:21:9: PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/examples/spacy/train.py#L39'>examples/spacy/train.py:39:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/mlflow/sklearn/__init__.py#L977'>mlflow/sklearn/__init__.py:977:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/mlflow/sklearn/__init__.py#L980'>mlflow/sklearn/__init__.py:980:13:</a> PERF401 Use `list.extend` to create a transformed list
... 5 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/_testing/__init__.py#L414'>pandas/_testing/__init__.py:414:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/core/_numba/kernels/min_max_.py#L176'>pandas/core/_numba/kernels/min_max_.py:176:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/core/arrays/period.py#L1467'>pandas/core/arrays/period.py:1467:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/core/internals/blocks.py#L567'>pandas/core/internals/blocks.py:567:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/core/reshape/merge.py#L1267'>pandas/core/reshape/merge.py:1267:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/io/formats/printing.py#L161'>pandas/io/formats/printing.py:161:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/io/json/_table_schema.py#L320'>pandas/io/json/_table_schema.py:320:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/tests/test_sorting.py#L290'>pandas/tests/test_sorting.py:290:17:</a> PERF401 Use `list.extend` to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/90da3e21e2cb44781a494d772839caa979ad972b/cibuildwheel/platforms/android.py#L507'>cibuildwheel/platforms/android.py:507:13:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+19 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/api/services/assets.py#L1179'>rotkehlchen/api/services/assets.py:1179:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/api/services/assets.py#L1208'>rotkehlchen/api/services/assets.py:1208:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/chain/decoding/decoder.py#L521'>rotkehlchen/chain/decoding/decoder.py:521:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/db/eth2.py#L830'>rotkehlchen/db/eth2.py:830:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/db/evmtx.py#L365'>rotkehlchen/db/evmtx.py:365:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/db/history_events.py#L1356'>rotkehlchen/db/history_events.py:1356:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/db/reports.py#L151'>rotkehlchen/db/reports.py:151:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/db/solanatx.py#L113'>rotkehlchen/db/solanatx.py:113:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/db/upgrades/v34_v35.py#L160'>rotkehlchen/db/upgrades/v34_v35.py:160:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/db/upgrades/v35_v36.py#L104'>rotkehlchen/db/upgrades/v35_v36.py:104:13:</a> PERF401 Use `list.extend` to create a transformed list
... 9 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/0b48eee8160553d67ac5b078304b025d053c125c/scripts/lib/check_rabbitmq_queue.py#L202'>scripts/lib/check_rabbitmq_queue.py:202:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/zulip/zulip/blob/0b48eee8160553d67ac5b078304b025d053c125c/zerver/actions/message_flags.py#L284'>zerver/actions/message_flags.py:284:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/zulip/zulip/blob/0b48eee8160553d67ac5b078304b025d053c125c/zerver/checks.py#L112'>zerver/checks.py:112:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/zulip/zulip/blob/0b48eee8160553d67ac5b078304b025d053c125c/zerver/checks.py#L124'>zerver/checks.py:124:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/zulip/zulip/blob/0b48eee8160553d67ac5b078304b025d053c125c/zerver/migrations/0436_realmauthenticationmethods.py#L18'>zerver/migrations/0436_realmauthenticationmethods.py:18:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/zulip/zulip/blob/0b48eee8160553d67ac5b078304b025d053c125c/zerver/openapi/markdown_extension.py#L347'>zerver/openapi/markdown_extension.py:347:13:</a> PERF401 Use `list.extend` to create a transformed list
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF401 | 119 | 119 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+119 -0 violations, +0 -0 fixes in 13 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+27 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/src/airflow/cli/commands/connection_command.py#L154'>airflow-core/src/airflow/cli/commands/connection_command.py:154:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/src/airflow/cli/commands/dag_command.py#L483'>airflow-core/src/airflow/cli/commands/dag_command.py:483:25:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/src/airflow/cli/commands/dag_command.py#L487'>airflow-core/src/airflow/cli/commands/dag_command.py:487:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/src/airflow/dag_processing/manager.py#L775'>airflow-core/src/airflow/dag_processing/manager.py:775:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/src/airflow/models/taskinstance.py#L1761'>airflow-core/src/airflow/models/taskinstance.py:1761:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/src/airflow/models/taskinstance.py#L1771'>airflow-core/src/airflow/models/taskinstance.py:1771:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/tests/conftest.py#L86'>airflow-core/tests/conftest.py:86:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/tests/unit/api_fastapi/common/test_exceptions.py#L85'>airflow-core/tests/unit/api_fastapi/common/test_exceptions.py:85:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/dev/breeze/src/airflow_breeze/utils/parallel.py#L247'>dev/breeze/src/airflow_breeze/utils/parallel.py:247:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/dev/stats/get_important_pr_candidates.py#L121'>dev/stats/get_important_pr_candidates.py:121:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/dev/stats/get_important_pr_candidates.py#L275'>dev/stats/get_important_pr_candidates.py:275:21:</a> PERF401 Use a list comprehension to create a transformed list
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/commands/importers/v1/__init__.py#L146'>superset/commands/importers/v1/__init__.py:146:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/daos/tag.py#L368'>superset/daos/tag.py:368:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/db_engine_specs/presto.py#L489'>superset/db_engine_specs/presto.py:489:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/mcp_service/chart/tool/get_chart_preview.py#L1442'>superset/mcp_service/chart/tool/get_chart_preview.py:1442:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/mcp_service/chart/tool/get_chart_preview.py#L1482'>superset/mcp_service/chart/tool/get_chart_preview.py:1482:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/mcp_service/screenshot/webdriver_pool.py#L279'>superset/mcp_service/screenshot/webdriver_pool.py:279:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/reports/notifications/webhook.py#L88'>superset/reports/notifications/webhook.py:88:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/viz.py#L894'>superset/viz.py:894:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/tests/integration_tests/security/migrate_roles_tests.py#L44'>tests/integration_tests/security/migrate_roles_tests.py:44:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/tests/integration_tests/security_tests.py#L1677'>tests/integration_tests/security_tests.py:1677:21:</a> PERF401 Use `list.extend` to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L813'>src/bokeh/core/has_props.py:813:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_sampledata_xref.py#L206'>src/bokeh/sphinxext/bokeh_sampledata_xref.py:206:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_no_request_host.py#L48'>tests/codebase/test_no_request_host.py:48:17:</a> PERF401 Use `list.extend` to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/64aed03898c543ede8ad2b7ce039b50d25861d33/ibis/backends/databricks/__init__.py#L187'>ibis/backends/databricks/__init__.py:187:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/ibis-project/ibis/blob/64aed03898c543ede8ad2b7ce039b50d25861d33/ibis/backends/flink/ddl.py#L34'>ibis/backends/flink/ddl.py:34:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/ibis-project/ibis/blob/64aed03898c543ede8ad2b7ce039b50d25861d33/ibis/backends/impala/ddl.py#L33'>ibis/backends/impala/ddl.py:33:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/ibis-project/ibis/blob/64aed03898c543ede8ad2b7ce039b50d25861d33/ibis/backends/risingwave/__init__.py#L56'>ibis/backends/risingwave/__init__.py:56:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/ibis-project/ibis/blob/64aed03898c543ede8ad2b7ce039b50d25861d33/ibis/expr/format.py#L194'>ibis/expr/format.py:194:13:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/cli/langchain_cli/namespaces/migrate/generate/generic.py#L68'>libs/cli/langchain_cli/namespaces/migrate/generate/generic.py:68:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/cli/langchain_cli/namespaces/migrate/generate/utils.py#L91'>libs/cli/langchain_cli/namespaces/migrate/generate/utils.py:91:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/core/langchain_core/messages/ai.py#L375'>libs/core/langchain_core/messages/ai.py:375:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/core/langchain_core/tools/base.py#L599'>libs/core/langchain_core/tools/base.py:599:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/core/langchain_core/utils/function_calling.py#L689'>libs/core/langchain_core/utils/function_calling.py:689:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/core/tests/unit_tests/runnables/test_concurrency.py#L73'>libs/core/tests/unit_tests/runnables/test_concurrency.py:73:9:</a> PERF401 Use an async list comprehension to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/langchain/langchain_classic/retrievers/merger_retriever.py#L117'>libs/langchain/langchain_classic/retrievers/merger_retriever.py:117:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/langchain/langchain_classic/retrievers/merger_retriever.py#L82'>libs/langchain/langchain_classic/retrievers/merger_retriever.py:82:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/langchain_v1/langchain/agents/middleware/file_search.py#L370'>libs/langchain_v1/langchain/agents/middleware/file_search.py:370:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/langchain-ai/langchain/blob/a6e8c8387882005081716821e0b55e53ed390cbf/libs/langchain_v1/tests/unit_tests/agents/test_agent_name.py#L122'>libs/langchain_v1/tests/unit_tests/agents/test_agent_name.py:122:13:</a> PERF401 Use a list comprehension to create a transformed list
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/functions/operators.py#L186'>src/latch/functions/operators.py:186:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/download.py#L135'>src/latch/ldata/_transfer/download.py:135:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch/ldata/_transfer/upload.py#L216'>src/latch/ldata/_transfer/upload.py:216:29:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/exceptions/traceback.py#L30'>src/latch_cli/exceptions/traceback.py:30:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/services/sync.py#L27'>src/latch_cli/services/sync.py:27:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/services/workspace.py#L26'>src/latch_cli/services/workspace.py:26:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/snakemake/config/utils.py#L298'>src/latch_cli/snakemake/config/utils.py:298:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/snakemake/workflow.py#L1532'>src/latch_cli/snakemake/workflow.py:1532:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/latchbio/latch/blob/9d6f3c68e6ad91b3b192f16990b55b38bc682048/src/latch_cli/snakemake/workflow.py#L387'>src/latch_cli/snakemake/workflow.py:387:13:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+15 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/dev/check_function_signatures.py#L65'>dev/check_function_signatures.py:65:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/dev/check_function_signatures.py#L97'>dev/check_function_signatures.py:97:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/dev/remove_experimental_decorators.py#L121'>dev/remove_experimental_decorators.py:121:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/dev/update_changelog.py#L119'>dev/update_changelog.py:119:9:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/docs/api_reference/source/conf.py#L421'>docs/api_reference/source/conf.py:421:17:</a> PERF401 Use `list.extend` to create a transformed list
+ examples/evaluation/rag-evaluation.ipynb:cell 9:4:9: PERF401 Use a list comprehension to create a transformed list
+ examples/llms/RAG/question-generation-retrieval-evaluation.ipynb:cell 16:21:9: PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/examples/spacy/train.py#L39'>examples/spacy/train.py:39:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/mlflow/sklearn/__init__.py#L977'>mlflow/sklearn/__init__.py:977:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/mlflow/mlflow/blob/df645374f8563bed620549ffca06001213f76bd3/mlflow/sklearn/__init__.py#L980'>mlflow/sklearn/__init__.py:980:13:</a> PERF401 Use `list.extend` to create a transformed list
... 5 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/_testing/__init__.py#L414'>pandas/_testing/__init__.py:414:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/core/_numba/kernels/min_max_.py#L176'>pandas/core/_numba/kernels/min_max_.py:176:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/core/arrays/period.py#L1467'>pandas/core/arrays/period.py:1467:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/core/internals/blocks.py#L567'>pandas/core/internals/blocks.py:567:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/core/reshape/merge.py#L1267'>pandas/core/reshape/merge.py:1267:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/io/formats/printing.py#L161'>pandas/io/formats/printing.py:161:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/io/json/_table_schema.py#L320'>pandas/io/json/_table_schema.py:320:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/pandas-dev/pandas/blob/d51494eeae96b4118057153c70555c31731605f0/pandas/tests/test_sorting.py#L290'>pandas/tests/test_sorting.py:290:17:</a> PERF401 Use `list.extend` to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/90da3e21e2cb44781a494d772839caa979ad972b/cibuildwheel/platforms/android.py#L507'>cibuildwheel/platforms/android.py:507:13:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+19 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/api/services/assets.py#L1179'>rotkehlchen/api/services/assets.py:1179:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/api/services/assets.py#L1208'>rotkehlchen/api/services/assets.py:1208:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/chain/decoding/decoder.py#L521'>rotkehlchen/chain/decoding/decoder.py:521:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/db/eth2.py#L830'>rotkehlchen/db/eth2.py:830:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/db/evmtx.py#L365'>rotkehlchen/db/evmtx.py:365:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/db/history_events.py#L1356'>rotkehlchen/db/history_events.py:1356:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/db/reports.py#L151'>rotkehlchen/db/reports.py:151:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/db/solanatx.py#L113'>rotkehlchen/db/solanatx.py:113:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/db/upgrades/v34_v35.py#L160'>rotkehlchen/db/upgrades/v34_v35.py:160:17:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/rotki/rotki/blob/5a10dc4aa1591155e5ce8aa92cb7e8f4fd1d4e44/rotkehlchen/db/upgrades/v35_v36.py#L104'>rotkehlchen/db/upgrades/v35_v36.py:104:13:</a> PERF401 Use `list.extend` to create a transformed list
... 9 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/0b48eee8160553d67ac5b078304b025d053c125c/scripts/lib/check_rabbitmq_queue.py#L202'>scripts/lib/check_rabbitmq_queue.py:202:9:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/zulip/zulip/blob/0b48eee8160553d67ac5b078304b025d053c125c/zerver/actions/message_flags.py#L284'>zerver/actions/message_flags.py:284:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/zulip/zulip/blob/0b48eee8160553d67ac5b078304b025d053c125c/zerver/checks.py#L112'>zerver/checks.py:112:13:</a> PERF401 Use a list comprehension to create a transformed list
+ <a href='https://github.com/zulip/zulip/blob/0b48eee8160553d67ac5b078304b025d053c125c/zerver/checks.py#L124'>zerver/checks.py:124:21:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/zulip/zulip/blob/0b48eee8160553d67ac5b078304b025d053c125c/zerver/migrations/0436_realmauthenticationmethods.py#L18'>zerver/migrations/0436_realmauthenticationmethods.py:18:17:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/zulip/zulip/blob/0b48eee8160553d67ac5b078304b025d053c125c/zerver/openapi/markdown_extension.py#L347'>zerver/openapi/markdown_extension.py:347:13:</a> PERF401 Use `list.extend` to create a transformed list
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF401 | 119 | 119 | 0 | 0 | 0 |

</p>
</details>





---

_Comment by @amyreese on 2026-01-08 19:57_

Some of the ecosystem changes looks legit (and good!) but at least a couple stand out as false positives, particularly ones where there are multiple places where `append` happens in different branches or in places where it clearly shouldn't (or couldn't) be a list comprehension:

https://github.com/apache/airflow/blob/6e93a46c39621a74bcb7b8276d3ecacd3dca9e54/airflow-core/src/airflow/models/taskinstance.py#L1759

https://github.com/apache/airflow/blob/6e93a46c39621a74bcb7b8276d3ecacd3dca9e54/airflow-core/src/airflow/cli/commands/dag_command.py#L479

---

_Label `rule` added by @amyreese on 2026-01-08 19:58_

---

_Comment by @Jkhall81 on 2026-01-08 20:13_

Looks like the rule is a little too aggressive now.  I'll fix it, and i'll be sure to check the ecosystem results.  I'm still getting used to how everything works here.  Thanks you for pointing this out though.

---

_Comment by @Jkhall81 on 2026-01-08 23:55_

I need some time to read over all the ecosystem stuff.  I looked over a few.  But it looks better so far.  I will finish this later today.

---

_Comment by @Jkhall81 on 2026-01-09 02:41_

It seems to be working.

---

_Comment by @amyreese on 2026-01-09 16:28_

Now it seems there are a number of existing violations that have turned into obvious false negatives. Do you know what would have caused this? A few examples from the ecosystem report that aren't "-0 violations":

https://github.com/apache/airflow/blob/7d6f0c2cc67cb0ef2bfcc1993fcfa8978c81f3a4/airflow-core/tests/unit/utils/test_task_group.py#L253

https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch/ldata/_transfer/upload.py#L169

https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/container.py#L101

---

_Comment by @ntBre on 2026-01-12 20:03_

Is this ready for another review? Should we add a couple of test cases based on the ecosystem results?

Based on the ecosystem impact, I'm thinking this should probably also be a preview change.

---

_Comment by @Jkhall81 on 2026-01-12 20:05_

I worked it over a few times.  If it still needs improvement, I need a shove in the right direction.  But yes, it's ready for another review.

---

_Comment by @Jkhall81 on 2026-01-12 20:22_

I can add tests based on the ecosystem results, no problem, if the 'rule update' is acceptable.

---

_Comment by @ntBre on 2026-01-12 20:26_

Sounds good! I just wanted to double check before I took a look. @amyreese might have a better sense for whether we should add more tests based on what she saw in the earlier ecosystem reports.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:272 on 2026-01-12 20:45_

Could we revert changing the order of this comparison and restore the comments that were here? I think the comments were still accurate.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:111 on 2026-01-12 20:52_

I think if we move the `targets.contains` check into the `for target_id in &targets` loop, we can avoid allocating a `Vec` here and just return a `&[Expr]` from the `match`. This is the patch I was working with locally:

```diff
diff --git a/crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs b/crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs
index 60edacb5e2..465211b334 100644
--- a/crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs
+++ b/crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs
@@ -95,20 +95,13 @@ impl Violation for ManualListComprehension {
 
 /// PERF401
 pub(crate) fn manual_list_comprehension(checker: &Checker, for_stmt: &ast::StmtFor) {
-    let mut targets = Vec::new();
-    match &*for_stmt.target {
-        Expr::Name(ast::ExprName { id, .. }) => targets.push(id),
+    let targets = match &*for_stmt.target {
+        name @ Expr::Name(_) => std::slice::from_ref(name),
         Expr::Tuple(ast::ExprTuple { elts, .. }) | Expr::List(ast::ExprList { elts, .. }) => {
-            for elt in elts {
-                if let Expr::Name(ast::ExprName { id, .. }) = elt {
-                    targets.push(id);
-                } else {
-                    return;
-                }
-            }
+            elts.as_slice()
         }
         _ => return,
-    }
+    };
 
     let (stmt, if_test) = match &*for_stmt.body {
         // ```python
@@ -181,19 +174,6 @@ pub(crate) fn manual_list_comprehension(checker: &Checker, for_stmt: &ast::StmtF
         return;
     };
 
-    // Ignore direct list copies (e.g., `for x in y: filtered.append(x)`), unless it's async, which
-    // `manual-list-copy` doesn't cover.
-    if !for_stmt.is_async {
-        if if_test.is_none() {
-            if arg
-                .as_name_expr()
-                .is_some_and(|arg| targets.contains(&&arg.id))
-            {
-                return;
-            }
-        }
-    }
-
     // Avoid, e.g., `for x in y: filtered.append(filtered[-1] * 2)`.
     if any_over_expr(arg, &|expr| {
         expr.as_name_expr()
@@ -254,11 +234,32 @@ pub(crate) fn manual_list_comprehension(checker: &Checker, for_stmt: &ast::StmtF
 
     // Ensure none of the loop targets (e.g., x, y in `for x, y in ...`)
     // leak outside the loop.
-    for target_id in &targets {
-        let Some(target_binding) = checker.semantic().bindings.iter().find(|binding| {
-            binding.name(checker.locator().contents()) == **target_id
-                && for_stmt.target.range().contains(binding.range().start())
-        }) else {
+    for target in targets {
+        let ast::Expr::Name(ast::ExprName {
+            id: target_id,
+            range: target_range,
+            ..
+        }) = target
+        else {
+            return;
+        };
+
+        // Ignore direct list copies (e.g., `for x in y: filtered.append(x)`), unless it's async, which
+        // `manual-list-copy` doesn't cover.
+        if !for_stmt.is_async {
+            if if_test.is_none() {
+                if arg.as_name_expr().is_some_and(|arg| arg.id == *target_id) {
+                    return;
+                }
+            }
+        }
+
+        let Some(target_binding) = checker
+            .semantic()
+            .bindings
+            .iter()
+            .find(|binding| binding.range == *target_range)
+        else {
             return;
         };
```

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/perflint/PERF401.py`:313 on 2026-01-12 20:53_

I think it would make sense to add a test with a list target:

```py
def f():
    xs = []
    for [x, y] in [[1, 2]]:
        xs.append(x + y)  # PERF401
```

since you handled it in the implementation.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:260 on 2026-01-12 21:01_

I included this in the patch above, but if we keep a reference to the whole `Expr` or `ExprName`, we can simplify this back to:

```suggestion
            binding.range == *target_range
```

`Binding::name` is just using its range internally, so I don't think we're gaining anything by comparing both the str derived from the range and the range itself.

---

_@ntBre reviewed on 2026-01-12 21:03_

Thank you! This looks reasonable to me overall, I Just had a few minor suggestions, mostly around avoiding allocating a `Vec` for the loop targets.

---

_Review comment by @Jkhall81 on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:260 on 2026-01-19 23:17_

I will work on this later today.  For some reason I didn't get an email notification when you left these comments (on the 12th), which is strange.  Because it says I should be receiving notifications from this page.  Otherwise I would have had these updates done sooner.  Oh well.  Thank you for all the feedback.

---

_@Jkhall81 reviewed on 2026-01-19 23:17_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:260 on 2026-01-20 00:40_

No worries, and no rush! Thanks for working on this!

---

_@ntBre reviewed on 2026-01-20 00:40_

---

_@Jkhall81 reviewed on 2026-01-20 02:36_

---

_Review comment by @Jkhall81 on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:260 on 2026-01-20 02:36_

I updated `manual_list_comprehension.rs` and `PERF401.py` to incorporate all of your suggested changes.

---

_Comment by @amyreese on 2026-01-21 01:08_

I'm still concerned about some of the ecosystem reports, like the ones below, where there are multiple locations in different branches that append to the same list, or are appending to lists that are passed as arguments. Unless I'm missing something, these are major false positives that should be "easy" to catch.

1: https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/src/airflow/cli/commands/dag_command.py#L487
2: https://github.com/apache/airflow/blob/41403de774f8db47c48de0448329d8751b090e98/airflow-core/src/airflow/models/taskinstance.py#L1761
3: https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/commands/importers/v1/__init__.py#L146
4: https://github.com/apache/superset/blob/f4597be341c3cc982a4c4639800c906be4ec39ba/superset/reports/notifications/webhook.py#L88
5: https://github.com/ibis-project/ibis/blob/64aed03898c543ede8ad2b7ce039b50d25861d33/ibis/backends/databricks/__init__.py#L187

---
