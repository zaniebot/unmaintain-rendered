```yaml
number: 10511
title: Fix F821 regression for classes marked as runtime-required
type: pull_request
state: closed
author: AlexWaygood
labels:
  - bug
assignees: []
draft: true
base: main
head: pydantic-f821
created_at: 2024-03-21T16:04:37Z
updated_at: 2024-03-21T16:59:28Z
url: https://github.com/astral-sh/ruff/pull/10511
synced_at: 2026-01-10T22:47:02Z
```

# Fix F821 regression for classes marked as runtime-required

---

_Pull request opened by @AlexWaygood on 2024-03-21 16:04_

Fixes #10451. This is basically a revert of 704fefc, but includes a more principled fix for the issue that that PR was trying to fix (#10362)

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-03-21 16:04_

---

_Label `bug` added by @AlexWaygood on 2024-03-21 16:16_

---

_Comment by @github-actions[bot] on 2024-03-21 16:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+97 -24 violations, +0 -0 fixes in 5 projects; 38 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -23 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/api_internal/endpoints/rpc_api_endpoint.py#L58'>airflow/api_internal/endpoints/rpc_api_endpoint.py:58:9:</a> SLF001 Private member accessed: `_add_to_db`
- <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/api_internal/endpoints/rpc_api_endpoint.py#L59'>airflow/api_internal/endpoints/rpc_api_endpoint.py:59:9:</a> SLF001 Private member accessed: `_fetch_from_db`
- <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/api_internal/endpoints/rpc_api_endpoint.py#L60'>airflow/api_internal/endpoints/rpc_api_endpoint.py:60:9:</a> SLF001 Private member accessed: `_kill`
- <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/api_internal/endpoints/rpc_api_endpoint.py#L61'>airflow/api_internal/endpoints/rpc_api_endpoint.py:61:9:</a> SLF001 Private member accessed: `_update_heartbeat`
- <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/api_internal/endpoints/rpc_api_endpoint.py#L62'>airflow/api_internal/endpoints/rpc_api_endpoint.py:62:9:</a> SLF001 Private member accessed: `_update_in_db`
- <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/api_internal/endpoints/rpc_api_endpoint.py#L64'>airflow/api_internal/endpoints/rpc_api_endpoint.py:64:9:</a> SLF001 Private member accessed: `_fetch_connection`
... 18 additional changes omitted for rule SLF001
+ <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/providers/amazon/aws/operators/appflow.py#L486'>airflow/providers/amazon/aws/operators/appflow.py:486:13:</a> UP037 [*] Remove quotes from type annotation
... 17 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L52'>src/bokeh/colors/color.py:52:35:</a> UP037 [*] Remove quotes from type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L58'>src/bokeh/core/query.py:58:48:</a> UP037 [*] Remove quotes from type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L186'>src/bokeh/core/serialization.py:186:37:</a> UP037 [*] Remove quotes from type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L187'>src/bokeh/core/serialization.py:187:40:</a> UP037 [*] Remove quotes from type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/types.py#L56'>src/bokeh/core/types.py:56:34:</a> UP037 [*] Remove quotes from type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L60'>tests/support/util/examples.py:60:41:</a> UP037 [*] Remove quotes from type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L60'>tests/support/util/examples.py:60:61:</a> UP037 [*] Remove quotes from type annotation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/0a00a0532c6a134985273fb492af8f77ecbb47c6/ibis/common/typing.py#L22'>ibis/common/typing.py:22:54:</a> UP037 [*] Remove quotes from type annotation
+ <a href='https://github.com/ibis-project/ibis/blob/0a00a0532c6a134985273fb492af8f77ecbb47c6/ibis/common/typing.py#L27'>ibis/common/typing.py:27:47:</a> UP037 [*] Remove quotes from type annotation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/python/mypy/blob/394d17b758bae6c95cbe91b84c5cccf0f4d73c28/mypy/build.py#L121'>mypy/build.py:121:31:</a> F821 Undefined name `State`
+ <a href='https://github.com/python/mypy/blob/394d17b758bae6c95cbe91b84c5cccf0f4d73c28/mypy/nodes.py#L265'>mypy/nodes.py:265:37:</a> F821 Undefined name `SymbolTableNode`
+ <a href='https://github.com/python/mypy/blob/394d17b758bae6c95cbe91b84c5cccf0f4d73c28/mypy/nodes.py#L265'>mypy/nodes.py:265:63:</a> F821 Undefined name `TypeInfo`
+ <a href='https://github.com/python/mypy/blob/394d17b758bae6c95cbe91b84c5cccf0f4d73c28/mypy/nodes.py#L548'>mypy/nodes.py:548:34:</a> F821 Undefined name `FuncDef`
+ <a href='https://github.com/python/mypy/blob/394d17b758bae6c95cbe91b84c5cccf0f4d73c28/mypy/nodes.py#L548'>mypy/nodes.py:548:43:</a> F821 Undefined name `Decorator`
+ <a href='https://github.com/python/mypy/blob/394d17b758bae6c95cbe91b84c5cccf0f4d73c28/mypy/server/astdiff.py#L117'>mypy/server/astdiff.py:117:51:</a> F821 Undefined name `SnapshotItem`
+ <a href='https://github.com/python/mypy/blob/394d17b758bae6c95cbe91b84c5cccf0f4d73c28/mypy/subtypes.py#L77'>mypy/subtypes.py:77:69:</a> F821 Undefined name `SubtypeContext`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+80 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/lib/counts.py#L376'>analytics/lib/counts.py:376:32:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Dict`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/management/commands/populate_analytics_db.py#L153'>analytics/management/commands/populate_analytics_db.py:153:42:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Union`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/management/commands/populate_analytics_db.py#L153'>analytics/management/commands/populate_analytics_db.py:153:65:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/views/stats.py#L262'>analytics/views/stats.py:262:28:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Union`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/views/stats.py#L263'>analytics/views/stats.py:263:9:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/views/stats.py#L264'>analytics/views/stats.py:264:9:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/views/stats.py#L265'>analytics/views/stats.py:265:9:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/views/stats.py#L266'>analytics/views/stats.py:266:9:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/confirmation/models.py#L65'>confirmation/models.py:65:41:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Union`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L702'>zerver/actions/streams.py:702:19:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Tuple`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L702'>zerver/actions/streams.py:702:25:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L702'>zerver/actions/streams.py:702:40:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L889'>zerver/actions/streams.py:889:29:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Tuple`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L890'>zerver/actions/streams.py:890:10:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Tuple`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L890'>zerver/actions/streams.py:890:39:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L890'>zerver/actions/streams.py:890:44:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Tuple`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L890'>zerver/actions/streams.py:890:5:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
... 64 additional changes omitted for rule FA100
- <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/lib/queue.py#L84'>zerver/lib/queue.py:84:13:</a> SLF001 Private member accessed: `_DEFAULT`
... 63 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FA100 | 80 | 80 | 0 | 0 | 0 |
| SLF001 | 24 | 0 | 24 | 0 | 0 |
| UP037 | 10 | 10 | 0 | 0 | 0 |
| F821 | 7 | 7 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+92 -28 violations, +0 -0 fixes in 6 projects; 37 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -26 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/api_internal/endpoints/rpc_api_endpoint.py#L58'>airflow/api_internal/endpoints/rpc_api_endpoint.py:58:9:</a> SLF001 Private member accessed: `_add_to_db`
- <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/api_internal/endpoints/rpc_api_endpoint.py#L59'>airflow/api_internal/endpoints/rpc_api_endpoint.py:59:9:</a> SLF001 Private member accessed: `_fetch_from_db`
- <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/api_internal/endpoints/rpc_api_endpoint.py#L60'>airflow/api_internal/endpoints/rpc_api_endpoint.py:60:9:</a> SLF001 Private member accessed: `_kill`
- <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/api_internal/endpoints/rpc_api_endpoint.py#L61'>airflow/api_internal/endpoints/rpc_api_endpoint.py:61:9:</a> SLF001 Private member accessed: `_update_heartbeat`
- <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/api_internal/endpoints/rpc_api_endpoint.py#L62'>airflow/api_internal/endpoints/rpc_api_endpoint.py:62:9:</a> SLF001 Private member accessed: `_update_in_db`
- <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/api_internal/endpoints/rpc_api_endpoint.py#L64'>airflow/api_internal/endpoints/rpc_api_endpoint.py:64:9:</a> SLF001 Private member accessed: `_fetch_connection`
... 18 additional changes omitted for rule SLF001
+ <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/providers/amazon/aws/operators/appflow.py#L486'>airflow/providers/amazon/aws/operators/appflow.py:486:13:</a> UP037 [*] Remove quotes from type annotation
- <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/providers/google/cloud/hooks/gcs.py#L859'>airflow/providers/google/cloud/hooks/gcs.py:859:49:</a> PLC2701 Private name import `_blobs_page_start` from external module `google.cloud.storage.bucket`
- <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/airflow/providers/google/cloud/hooks/gcs.py#L859'>airflow/providers/google/cloud/hooks/gcs.py:859:68:</a> PLC2701 Private name import `_item_to_blob` from external module `google.cloud.storage.bucket`
- <a href='https://github.com/apache/airflow/blob/a1671f1f7d4c3d74de3121c0f8aa18cef8823464/tests/providers/amazon/aws/utils/test_utils.py#L22'>tests/providers/amazon/aws/utils/test_utils.py:22:5:</a> PLC2701 Private name import `_StringCompareEnum` from external module `airflow.providers.amazon.aws.utils`
... 17 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L52'>src/bokeh/colors/color.py:52:35:</a> UP037 [*] Remove quotes from type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L58'>src/bokeh/core/query.py:58:48:</a> UP037 [*] Remove quotes from type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L186'>src/bokeh/core/serialization.py:186:37:</a> UP037 [*] Remove quotes from type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L187'>src/bokeh/core/serialization.py:187:40:</a> UP037 [*] Remove quotes from type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/types.py#L56'>src/bokeh/core/types.py:56:34:</a> UP037 [*] Remove quotes from type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L60'>tests/support/util/examples.py:60:41:</a> UP037 [*] Remove quotes from type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L60'>tests/support/util/examples.py:60:61:</a> UP037 [*] Remove quotes from type annotation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/0853b11966927ef91474783646bc4927b609fe4e/securedrop/secure_tempfile.py#L3'>securedrop/secure_tempfile.py:3:22:</a> PLC2701 Private name import `_TemporaryFileWrapper` from external module `tempfile`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/0a00a0532c6a134985273fb492af8f77ecbb47c6/ibis/common/typing.py#L22'>ibis/common/typing.py:22:54:</a> UP037 [*] Remove quotes from type annotation
+ <a href='https://github.com/ibis-project/ibis/blob/0a00a0532c6a134985273fb492af8f77ecbb47c6/ibis/common/typing.py#L27'>ibis/common/typing.py:27:47:</a> UP037 [*] Remove quotes from type annotation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/mypy/blob/394d17b758bae6c95cbe91b84c5cccf0f4d73c28/mypy/build.py#L121'>mypy/build.py:121:31:</a> F821 Undefined name `State`
+ <a href='https://github.com/python/mypy/blob/394d17b758bae6c95cbe91b84c5cccf0f4d73c28/mypy/subtypes.py#L77'>mypy/subtypes.py:77:69:</a> F821 Undefined name `SubtypeContext`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+80 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/lib/counts.py#L376'>analytics/lib/counts.py:376:32:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Dict`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/management/commands/populate_analytics_db.py#L153'>analytics/management/commands/populate_analytics_db.py:153:42:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Union`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/management/commands/populate_analytics_db.py#L153'>analytics/management/commands/populate_analytics_db.py:153:65:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/views/stats.py#L262'>analytics/views/stats.py:262:28:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Union`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/views/stats.py#L263'>analytics/views/stats.py:263:9:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/views/stats.py#L264'>analytics/views/stats.py:264:9:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/views/stats.py#L265'>analytics/views/stats.py:265:9:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/analytics/views/stats.py#L266'>analytics/views/stats.py:266:9:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Type`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/confirmation/models.py#L65'>confirmation/models.py:65:41:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Union`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L702'>zerver/actions/streams.py:702:19:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Tuple`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L702'>zerver/actions/streams.py:702:25:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L702'>zerver/actions/streams.py:702:40:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L889'>zerver/actions/streams.py:889:29:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Tuple`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L890'>zerver/actions/streams.py:890:10:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Tuple`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L890'>zerver/actions/streams.py:890:39:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L890'>zerver/actions/streams.py:890:44:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.Tuple`
+ <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/actions/streams.py#L890'>zerver/actions/streams.py:890:5:</a> FA100 Missing `from __future__ import annotations`, but uses `typing.List`
... 64 additional changes omitted for rule FA100
- <a href='https://github.com/zulip/zulip/blob/6f81fe1b5b8d7987f7a06197b5a106182c3eaa8e/zerver/lib/queue.py#L84'>zerver/lib/queue.py:84:13:</a> SLF001 Private member accessed: `_DEFAULT`
... 63 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (5 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FA100 | 80 | 80 | 0 | 0 | 0 |
| SLF001 | 24 | 0 | 24 | 0 | 0 |
| UP037 | 10 | 10 | 0 | 0 | 0 |
| PLC2701 | 4 | 0 | 4 | 0 | 0 |
| F821 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-03-21 16:21_

I think this is not quite right but I haven't dug into why. It seems off to me that we have to set `runtime_annotation` in those various places here.

---

_Comment by @AlexWaygood on 2024-03-21 16:22_

Yeah, the ecosystem results indicate that this definitely is not quite right just yet :/

---

_Comment by @charliermarsh on 2024-03-21 16:22_

Let's start with a revert and revisit the fix itself.

---

_Comment by @AlexWaygood on 2024-03-21 16:23_

Sure, I'll do a revert PR now 👍

---

_Comment by @AlexWaygood on 2024-03-21 16:34_

#10513 to revert.

---

_Converted to draft by @AlexWaygood on 2024-03-21 16:34_

---

_Closed by @AlexWaygood on 2024-03-21 16:58_

---

_Branch deleted on 2024-03-21 16:59_

---

_Comment by @AlexWaygood on 2024-03-21 16:59_

I'm still looking at this locally, but closing the PR for now

---
