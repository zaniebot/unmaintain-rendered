```yaml
number: 18516
title: "[`flake8-return`] Stabilize only add `return None` at the end when fixing `implicit-return` (`RET503`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - fixes
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-return-none
created_at: 2025-06-06T22:25:37Z
updated_at: 2025-06-12T16:18:25Z
url: https://github.com/astral-sh/ruff/pull/18516
synced_at: 2026-01-10T18:45:04Z
```

# [`flake8-return`] Stabilize only add `return None` at the end when fixing `implicit-return` (`RET503`)

---

_Pull request opened by @dylwil3 on 2025-06-06 22:25_

This involved slightly more code changes than usual for a stabilization - so maybe worth double-checking the logic!

I did verify by hand that the new stable behavior on the test fixture matches the old preview behavior, even after the internal refactor.

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-06 22:25_

---

_Label `rule` added by @dylwil3 on 2025-06-06 22:25_

---

_Label `fixes` added by @dylwil3 on 2025-06-06 22:25_

---

_Comment by @github-actions[bot] on 2025-06-06 22:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+134 -155 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+77 -88 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/airflow-core/src/airflow/logging_config.py#L38'>airflow-core/src/airflow/logging_config.py:38:1:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/airflow-core/src/airflow/logging_config.py#L39'>airflow-core/src/airflow/logging_config.py:39:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/airflow-core/src/airflow/metrics/otel_logger.py#L180'>airflow-core/src/airflow/metrics/otel_logger.py:180:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/airflow-core/src/airflow/metrics/otel_logger.py#L201'>airflow-core/src/airflow/metrics/otel_logger.py:201:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/airflow-core/src/airflow/metrics/otel_logger.py#L206'>airflow-core/src/airflow/metrics/otel_logger.py:206:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/airflow-core/src/airflow/metrics/otel_logger.py#L227'>airflow-core/src/airflow/metrics/otel_logger.py:227:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/airflow-core/src/airflow/triggers/base.py#L224'>airflow-core/src/airflow/triggers/base.py:224:1:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/airflow-core/src/airflow/triggers/base.py#L227'>airflow-core/src/airflow/triggers/base.py:227:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/airflow-core/src/airflow/utils/retries.py#L77'>airflow-core/src/airflow/utils/retries.py:77:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/airflow-core/src/airflow/utils/retries.py#L93'>airflow-core/src/airflow/utils/retries.py:93:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/airflow-core/tests/unit/models/test_mappedoperator.py#L463'>airflow-core/tests/unit/models/test_mappedoperator.py:463:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/airflow-core/tests/unit/models/test_mappedoperator.py#L466'>airflow-core/tests/unit/models/test_mappedoperator.py:466:17:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/dev/breeze/src/airflow_breeze/global_constants.py#L566'>dev/breeze/src/airflow_breeze/global_constants.py:566:1:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/dev/breeze/src/airflow_breeze/global_constants.py#L570'>dev/breeze/src/airflow_breeze/global_constants.py:570:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/dev/breeze/src/airflow_breeze/utils/run_utils.py#L465'>dev/breeze/src/airflow_breeze/utils/run_utils.py:465:1:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/dev/breeze/src/airflow_breeze/utils/run_utils.py#L496'>dev/breeze/src/airflow_breeze/utils/run_utils.py:496:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/dev/breeze/src/airflow_breeze/utils/version_utils.py#L20'>dev/breeze/src/airflow_breeze/utils/version_utils.py:20:1:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/dev/breeze/src/airflow_breeze/utils/version_utils.py#L25'>dev/breeze/src/airflow_breeze/utils/version_utils.py:25:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/devel-common/src/tests_common/test_utils/api_client_helpers.py#L62'>devel-common/src/tests_common/test_utils/api_client_helpers.py:62:1:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/devel-common/src/tests_common/test_utils/api_client_helpers.py#L84'>devel-common/src/tests_common/test_utils/api_client_helpers.py:84:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/devel-common/src/tests_common/test_utils/logging_command_executor.py#L105'>devel-common/src/tests_common/test_utils/logging_command_executor.py:105:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/devel-common/src/tests_common/test_utils/logging_command_executor.py#L83'>devel-common/src/tests_common/test_utils/logging_command_executor.py:83:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/providers/amazon/src/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py#L308'>providers/amazon/src/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py:308:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/providers/amazon/src/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py#L319'>providers/amazon/src/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py:319:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/providers/amazon/src/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py#L320'>providers/amazon/src/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py:320:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/providers/amazon/src/airflow/providers/amazon/aws/executors/ecs/utils.py#L254'>providers/amazon/src/airflow/providers/amazon/aws/executors/ecs/utils.py:254:1:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/providers/amazon/src/airflow/providers/amazon/aws/executors/ecs/utils.py#L258'>providers/amazon/src/airflow/providers/amazon/aws/executors/ecs/utils.py:258:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/airflow/blob/3535bcd6f611c3540d3790a65cc4e5d887bd8ea2/providers/amazon/src/airflow/providers/amazon/aws/operators/emr.py#L1277'>providers/amazon/src/airflow/providers/amazon/aws/operators/emr.py:1277:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
... 137 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/examples/helpers.py#L128'>superset/examples/helpers.py:128:1:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/examples/helpers.py#L140'>superset/examples/helpers.py:140:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/migrations/versions/2018-07-05_15-19_3dda56f1c4c6_migrate_num_period_compare_and_period_.py#L112'>superset/migrations/versions/2018-07-05_15-19_3dda56f1c4c6_migrate_num_period_compare_and_period_.py:112:1:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/superset/migrations/versions/2018-07-05_15-19_3dda56f1c4c6_migrate_num_period_compare_and_period_.py#L129'>superset/migrations/versions/2018-07-05_15-19_3dda56f1c4c6_migrate_num_period_compare_and_period_.py:129:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/tests/integration_tests/utils/hashing_tests.py#L59'>tests/integration_tests/utils/hashing_tests.py:59:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/tests/integration_tests/utils/hashing_tests.py#L60'>tests/integration_tests/utils/hashing_tests.py:60:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/tests/unit_tests/queries/query_object_test.py#L103'>tests/unit_tests/queries/query_object_test.py:103:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/apache/superset/blob/e6af4ea126058052506e5437cbe90f9c5f2192d0/tests/unit_tests/queries/query_object_test.py#L104'>tests/unit_tests/queries/query_object_test.py:104:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+43 -48 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/clustering/main.py#L82'>examples/server/app/clustering/main.py:82:1:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/clustering/main.py#L83'>examples/server/app/clustering/main.py:83:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L314'>src/bokeh/core/has_props.py:314:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L340'>src/bokeh/core/has_props.py:340:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L342'>src/bokeh/core/has_props.py:342:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L367'>src/bokeh/core/has_props.py:367:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L386'>src/bokeh/core/serialization.py:386:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L409'>src/bokeh/core/serialization.py:409:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L438'>src/bokeh/core/serialization.py:438:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L469'>src/bokeh/core/serialization.py:469:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L530'>src/bokeh/core/serialization.py:530:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L562'>src/bokeh/core/serialization.py:562:21:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L572'>src/bokeh/core/serialization.py:572:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L578'>src/bokeh/core/serialization.py:578:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L710'>src/bokeh/core/serialization.py:710:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
... 76 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+6 -11 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/types/directory.py#L127'>src/latch/types/directory.py:127:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/types/directory.py#L129'>src/latch/types/directory.py:129:17:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/types/file.py#L90'>src/latch/types/file.py:90:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/types/file.py#L92'>src/latch/types/file.py:92:17:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch_cli/centromere/utils.py#L250'>src/latch_cli/centromere/utils.py:250:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch_cli/centromere/utils.py#L253'>src/latch_cli/centromere/utils.py:253:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch_cli/snakemake/config/utils.py#L281'>src/latch_cli/snakemake/config/utils.py:281:1:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch_cli/snakemake/config/utils.py#L291'>src/latch_cli/snakemake/config/utils.py:291:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch_cli/snakemake/config/utils.py#L304'>src/latch_cli/snakemake/config/utils.py:304:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch_cli/snakemake/config/utils.py#L309'>src/latch_cli/snakemake/config/utils.py:309:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/f2a988f6843fb7f3b4b60ba3c593fe5e3f517958/_version_helper.py#L41'>_version_helper.py:41:1:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/milvus-io/pymilvus/blob/f2a988f6843fb7f3b4b60ba3c593fe5e3f517958/_version_helper.py#L42'>_version_helper.py:42:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/24e8ba97cb9f462ecabb800eae379bbcde9a306e/zerver/lib/addressee.py#L100'>zerver/lib/addressee.py:100:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/zulip/zulip/blob/24e8ba97cb9f462ecabb800eae379bbcde9a306e/zerver/lib/addressee.py#L141'>zerver/lib/addressee.py:141:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/zulip/zulip/blob/24e8ba97cb9f462ecabb800eae379bbcde9a306e/zerver/tests/test_import_export.py#L2374'>zerver/tests/test_import_export.py:2374:13:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/zulip/zulip/blob/24e8ba97cb9f462ecabb800eae379bbcde9a306e/zerver/tests/test_import_export.py#L2377'>zerver/tests/test_import_export.py:2377:17:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
+ <a href='https://github.com/zulip/zulip/blob/24e8ba97cb9f462ecabb800eae379bbcde9a306e/zerver/tests/test_openapi.py#L430'>zerver/tests/test_openapi.py:430:5:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
- <a href='https://github.com/zulip/zulip/blob/24e8ba97cb9f462ecabb800eae379bbcde9a306e/zerver/tests/test_openapi.py#L444'>zerver/tests/test_openapi.py:444:9:</a> RET503 Missing explicit `return` at the end of function able to return non-`None` value
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RET503 | 289 | 134 | 155 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_return/snapshots/ruff_linter__rules__flake8_return__tests__RET503_RET503.py.snap`:29 on 2025-06-09 20:51_

Hmm, this old range was actually kinda cool. Do you think it's worth preserving some of the old implementation? This range could be nice to point to in a multi-part diagnostic (`info: This branch may not return` or something). On the other hand, it's obviously nice not to have to allocate a vec in the new version.

---

_@ntBre approved on 2025-06-09 20:54_

Nice, this looks right to me! Just one idea/question

---

_@dylwil3 reviewed on 2025-06-09 21:14_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_return/snapshots/ruff_linter__rules__flake8_return__tests__RET503_RET503.py.snap`:29 on 2025-06-09 21:14_

Oh that sounds fun! Can you remind me how `noqa` plays with multi-part diagnostics? At the moment part of the breaking change here is the change in noqa range I think. If we stabilizie as-is in this pr, and then later add back in the multi-part info, would that change the noqa range again?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_return/snapshots/ruff_linter__rules__flake8_return__tests__RET503_RET503.py.snap`:29 on 2025-06-09 21:23_

Oh good point, I don't think I know for sure. I guess I'm assuming the noqa would correspond to the main diagnostic, and the sub-diagnostics are just for extra info. My idea in this case was to keep the new diagnostic range of the whole function, but keep gathering the sub-ranges for future use, which I think is in line with that. I just didn't want to have to resurrect the `Vec` code later, although it would be mostly unused for now.

We should add this PR to https://github.com/astral-sh/ruff/issues/17203 if we decide to keep the `Vec` around.

Do you think it's even helpful to know which arm caused the diagnostic? I could easily be convinced this is not a good use of multi-span diagnostics :laughing: just an idea

---

_@ntBre reviewed on 2025-06-09 21:23_

---

_Review requested from @AlexWaygood by @ntBre on 2025-06-10 20:44_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-10 20:51_

---

_@dylwil3 reviewed on 2025-06-12 15:50_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_return/snapshots/ruff_linter__rules__flake8_return__tests__RET503_RET503.py.snap`:29 on 2025-06-12 15:50_

Ok looking at this again, maybe it's not so helpful? I think in almost all cases where someone wants this rule enabled, they will be in the situation where the correct fix is to add `return None` at the end of the function. 

The multi-spans would only be useful if they actually intended to remove all the explicit returns - right?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_return/snapshots/ruff_linter__rules__flake8_return__tests__RET503_RET503.py.snap`:29 on 2025-06-12 15:56_

Ah yeah that's probably fair, more of a piece of trivia than something useful. I was thinking about saying let's just proceed with the current version anyway. Thanks for considering it!

---

_@ntBre reviewed on 2025-06-12 15:56_

---

_Merged by @dylwil3 on 2025-06-12 16:10_

---

_Closed by @dylwil3 on 2025-06-12 16:10_

---

_Branch deleted on 2025-06-12 16:10_

---
