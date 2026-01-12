```yaml
number: 18553
title: "[`pylint`] Stabilize `unnecessary-lambda` (`PLW0108`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-unnecessary-lambda
created_at: 2025-06-08T18:38:34Z
updated_at: 2025-06-09T17:47:01Z
url: https://github.com/astral-sh/ruff/pull/18553
synced_at: 2026-01-12T15:56:21Z
```

# [`pylint`] Stabilize `unnecessary-lambda` (`PLW0108`)

---

_@dylwil3_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-08 18:38_

---

_Label `rule` added by @dylwil3 on 2025-06-08 18:38_

---

_Comment by @github-actions[bot] on 2025-06-08 18:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+121 -1 violations, +0 -0 fixes in 15 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/client.py#L1292'>disnake/client.py:1292:52:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/client.py#L1293'>disnake/client.py:1293:53:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/flag_converter.py#L330'>disnake/ext/commands/flag_converter.py:330:33:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+17 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/src/airflow/traces/tracer.py#L291'>airflow-core/src/airflow/traces/tracer.py:291:40:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/models/test_xcom.py#L331'>airflow-core/tests/unit/models/test_xcom.py:331:17:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L3803'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:3803:46:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/dev/breeze/src/airflow_breeze/utils/publish_docs_to_s3.py#L325'>dev/breeze/src/airflow_breeze/utils/publish_docs_to_s3.py:325:17:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/utils/pod_manager.py#L635'>providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/utils/pod_manager.py:635:59:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py#L75'>providers/common/compat/src/airflow/providers/common/compat/lineage/hook.py:75:18:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L358'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:358:27:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L412'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:412:21:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/openlineage/src/airflow/providers/openlineage/utils/utils.py#L503'>providers/openlineage/src/airflow/providers/openlineage/utils/utils.py:503:32:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/standard/tests/unit/standard/operators/test_bash.py#L241'>providers/standard/tests/unit/standard/operators/test_bash.py:241:30:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/dashboards/schemas.py#L306'>superset/dashboards/schemas.py:306:42:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/datasets/schemas.py#L105'>superset/datasets/schemas.py:105:23:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/utils/csv.py#L112'>superset/utils/csv.py:112:43:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/tests/unit_tests/utils/date_parser_tests.py#L514'>tests/unit_tests/utils/date_parser_tests.py:514:33:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/tests/unit_tests/utils/date_parser_tests.py#L528'>tests/unit_tests/utils/date_parser_tests.py:528:33:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/65baa83e3df8b80e608ae7e2c2fa64c42e385022/tests/unit/commands/local/lib/test_stack_provider.py#L114'>tests/unit/commands/local/lib/test_stack_provider.py:114:51:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/aws/aws-sam-cli/blob/65baa83e3df8b80e608ae7e2c2fa64c42e385022/tests/unit/commands/local/lib/test_stack_provider.py#L152'>tests/unit/commands/local/lib/test_stack_provider.py:152:51:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/aws/aws-sam-cli/blob/65baa83e3df8b80e608ae7e2c2fa64c42e385022/tests/unit/commands/local/lib/test_stack_provider.py#L190'>tests/unit/commands/local/lib/test_stack_provider.py:190:51:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/aws/aws-sam-cli/blob/65baa83e3df8b80e608ae7e2c2fa64c42e385022/tests/unit/commands/local/lib/test_stack_provider.py#L229'>tests/unit/commands/local/lib/test_stack_provider.py:229:51:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/aws/aws-sam-cli/blob/65baa83e3df8b80e608ae7e2c2fa64c42e385022/tests/unit/commands/local/lib/test_stack_provider.py#L270'>tests/unit/commands/local/lib/test_stack_provider.py:270:51:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/aws/aws-sam-cli/blob/65baa83e3df8b80e608ae7e2c2fa64c42e385022/tests/unit/lib/observability/xray_traces/test_xray_event_mappers.py#L69'>tests/unit/lib/observability/xray_traces/test_xray_event_mappers.py:69:46:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/aws/aws-sam-cli/blob/65baa83e3df8b80e608ae7e2c2fa64c42e385022/tests/unit/lib/telemetry/test_event.py#L137'>tests/unit/lib/telemetry/test_event.py:137:21:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+43 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/dataspec.py#L201'>src/bokeh/core/property/dataspec.py:201:71:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/webdriver.py#L219'>src/bokeh/io/webdriver.py:219:17:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/annotations/geometry.py#L85'>src/bokeh/models/annotations/geometry.py:85:31:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/annotations/geometry.py#L89'>src/bokeh/models/annotations/geometry.py:89:32:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/annotations/geometry.py#L93'>src/bokeh/models/annotations/geometry.py:93:30:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/annotations/geometry.py#L97'>src/bokeh/models/annotations/geometry.py:97:33:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L203'>src/bokeh/models/sources.py:203:37:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L205'>src/bokeh/models/sources.py:205:48:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/tools.py#L1367'>src/bokeh/models/tools.py:1367:101:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/tools.py#L1875'>src/bokeh/models/tools.py:1875:28:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/tools.py#L1877'>src/bokeh/models/tools.py:1877:34:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/tools.py#L1878'>src/bokeh/models/tools.py:1878:35:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/tools.py#L1881'>src/bokeh/models/tools.py:1881:36:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/tools.py#L1882'>src/bokeh/models/tools.py:1882:37:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/tools.py#L1887'>src/bokeh/models/tools.py:1887:29:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/tools.py#L1888'>src/bokeh/models/tools.py:1888:29:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/tools.py#L1889'>src/bokeh/models/tools.py:1889:29:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
... 26 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/93d909adcafa8b3fccd7fed2b8f9cc22d81c3dda/securedrop/pretty_bad_protocol/gnupg.py#L644'>securedrop/pretty_bad_protocol/gnupg.py:644:26:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/db2576334c8b792d21855c7bb2d33c8b785cbe6f/tests/test_blink_functions.py#L192'>tests/test_blink_functions.py:192:13:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/fronzbot/blinkpy/blob/db2576334c8b792d21855c7bb2d33c8b785cbe6f/tests/test_sync_module.py#L616'>tests/test_sync_module.py:616:13:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/fronzbot/blinkpy/blob/db2576334c8b792d21855c7bb2d33c8b785cbe6f/tests/test_util.py#L135'>tests/test_util.py:135:13:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/fronzbot/blinkpy/blob/db2576334c8b792d21855c7bb2d33c8b785cbe6f/tests/test_util.py#L149'>tests/test_util.py:149:13:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/fronzbot/blinkpy/blob/db2576334c8b792d21855c7bb2d33c8b785cbe6f/tests/test_util.py#L166'>tests/test_util.py:166:13:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/backends/impala/tests/test_value_exprs.py#L217'>ibis/backends/impala/tests/test_value_exprs.py:217:9:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/backends/tests/sql/test_sql.py#L671'>ibis/backends/tests/sql/test_sql.py:671:22:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/backends/tests/test_aggregation.py#L1481'>ibis/backends/tests/test_aggregation.py:1481:16:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/backends/tests/test_aggregation.py#L1481'>ibis/backends/tests/test_aggregation.py:1481:35:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/backends/tests/test_client.py#L209'>ibis/backends/tests/test_client.py:209:24:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/backends/tests/test_client.py#L844'>ibis/backends/tests/test_client.py:844:13:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/backends/tests/test_client.py#L863'>ibis/backends/tests/test_client.py:863:13:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/backends/tests/test_client.py#L871'>ibis/backends/tests/test_client.py:871:13:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/backends/tests/test_vectorized_udf.py#L75'>ibis/backends/tests/test_vectorized_udf.py:75:42:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/backends/tests/test_vectorized_udf.py#L76'>ibis/backends/tests/test_vectorized_udf.py:76:42:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/tests/unit_tests/messages/test_utils.py#L425'>libs/core/tests/unit_tests/messages/test_utils.py:425:23:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/tests/unit_tests/messages/test_utils.py#L443'>libs/core/tests/unit_tests/messages/test_utils.py:443:23:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/tests/unit_tests/runnables/test_history.py#L810'>libs/core/tests/unit_tests/runnables/test_history.py:810:31:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/tests/unit_tests/runnables/test_history.py#L836'>libs/core/tests/unit_tests/runnables/test_history.py:836:31:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/account.py#L67'>src/latch/account.py:67:25:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/idl/core/execution.py#L60'>src/latch/idl/core/execution.py:60:25:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/ldata/path.py#L335'>src/latch/ldata/path.py:335:29:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/registry/project.py#L37'>src/latch/registry/project.py:37:25:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/registry/record.py#L92'>src/latch/registry/record.py:92:25:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/registry/table.py#L92'>src/latch/registry/table.py:92:25:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/bulk_writer/constants.py#L55'>pymilvus/bulk_writer/constants.py:55:33:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/bulk_writer/constants.py#L56'>pymilvus/bulk_writer/constants.py:56:34:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/bulk_writer/constants.py#L59'>pymilvus/bulk_writer/constants.py:59:40:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/milvus-io/pymilvus/blob/368960c141800cd0a01416d048c6ada7aa33a1c6/pymilvus/bulk_writer/constants.py#L60'>pymilvus/bulk_writer/constants.py:60:32:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/6fc06cfcc5434e984aa676b51896ab2c0fe433c4/rotkehlchen/tests/utils/blockchain.py#L684'>rotkehlchen/tests/utils/blockchain.py:684:96:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PLW0108`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/lib/typed_endpoint_validators.py#L101'>zerver/lib/typed_endpoint_validators.py:101:28:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/lib/typed_endpoint_validators.py#L72'>zerver/lib/typed_endpoint_validators.py:72:27:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/lib/typed_endpoint_validators.py#L84'>zerver/lib/typed_endpoint_validators.py:84:27:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/views/realm.py#L166'>zerver/views/realm.py:166:24:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0108 | 121 | 121 | 0 | 0 | 0 |
| RUF100 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-06-08 19:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-08 20:29_

---

_@ntBre approved on 2025-06-09 14:13_

I spot-checked the ecosystem results, and they LGTM too

---

_Merged by @dylwil3 on 2025-06-09 17:47_

---

_Closed by @dylwil3 on 2025-06-09 17:47_

---

_Branch deleted on 2025-06-09 17:47_

---
