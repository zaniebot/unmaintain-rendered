```yaml
number: 15313
title: "[`pyupgrade`] Split `UP007` to two individual rules for `Union` and `Optional` (`UP007`, `UP045`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: UP007
created_at: 2025-01-07T06:20:40Z
updated_at: 2025-01-07T14:49:34Z
url: https://github.com/astral-sh/ruff/pull/15313
synced_at: 2026-01-12T15:55:50Z
```

# [`pyupgrade`] Split `UP007` to two individual rules for `Union` and `Optional` (`UP007`, `UP045`)

---

_@InSyncWithFoo_

## Summary

Resolves #4858. Part of this PR was taken from #11379.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-07 06:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1556 -94 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/0cc2d724d1534bf2f758fb6ce606f7b18cc81232/airflow/utils/log/logging_mixin.py#L175'>airflow/utils/log/logging_mixin.py:175:9:</a> PLR6301 Method `writable` could be a function, class method, or static method
+ <a href='https://github.com/apache/airflow/blob/0cc2d724d1534bf2f758fb6ce606f7b18cc81232/airflow/utils/log/logging_mixin.py#L175'>airflow/utils/log/logging_mixin.py:175:9:</a> PLR6301 Method `writable` could be a function, class method, or static method
+ <a href='https://github.com/apache/airflow/blob/0cc2d724d1534bf2f758fb6ce606f7b18cc81232/airflow/utils/log/logging_mixin.py#L1'>airflow/utils/log/logging_mixin.py:1:1:</a> D100 Missing docstring in public module
- <a href='https://github.com/apache/airflow/blob/0cc2d724d1534bf2f758fb6ce606f7b18cc81232/airflow/utils/log/logging_mixin.py#L1'>airflow/utils/log/logging_mixin.py:1:1:</a> D100 Missing docstring in public module
+ <a href='https://github.com/apache/airflow/blob/0cc2d724d1534bf2f758fb6ce606f7b18cc81232/airflow/utils/log/logging_mixin.py#L76'>airflow/utils/log/logging_mixin.py:76:30:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/apache/airflow/blob/0cc2d724d1534bf2f758fb6ce606f7b18cc81232/airflow/utils/log/logging_mixin.py#L76'>airflow/utils/log/logging_mixin.py:76:52:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
+ <a href='https://github.com/apache/airflow/blob/0cc2d724d1534bf2f758fb6ce606f7b18cc81232/airflow/utils/log/logging_mixin.py#L78'>airflow/utils/log/logging_mixin.py:78:19:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/apache/airflow/blob/0cc2d724d1534bf2f758fb6ce606f7b18cc81232/airflow/utils/log/logging_mixin.py#L78'>airflow/utils/log/logging_mixin.py:78:41:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+79 -79 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/9e17304523f74cb97558cbb1e78c57e4f2045a14/superset/async_events/async_query_manager.py#L106'>superset/async_events/async_query_manager.py:106:22:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/superset/blob/9e17304523f74cb97558cbb1e78c57e4f2045a14/superset/async_events/async_query_manager.py#L106'>superset/async_events/async_query_manager.py:106:22:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/9e17304523f74cb97558cbb1e78c57e4f2045a14/superset/async_events/async_query_manager.py#L108'>superset/async_events/async_query_manager.py:108:29:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/superset/blob/9e17304523f74cb97558cbb1e78c57e4f2045a14/superset/async_events/async_query_manager.py#L108'>superset/async_events/async_query_manager.py:108:29:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/9e17304523f74cb97558cbb1e78c57e4f2045a14/superset/async_events/async_query_manager.py#L109'>superset/async_events/async_query_manager.py:109:38:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/superset/blob/9e17304523f74cb97558cbb1e78c57e4f2045a14/superset/async_events/async_query_manager.py#L109'>superset/async_events/async_query_manager.py:109:38:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/9e17304523f74cb97558cbb1e78c57e4f2045a14/superset/async_events/async_query_manager.py#L112'>superset/async_events/async_query_manager.py:112:34:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/superset/blob/9e17304523f74cb97558cbb1e78c57e4f2045a14/superset/async_events/async_query_manager.py#L112'>superset/async_events/async_query_manager.py:112:34:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/9e17304523f74cb97558cbb1e78c57e4f2045a14/superset/async_events/async_query_manager.py#L113'>superset/async_events/async_query_manager.py:113:36:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/apache/superset/blob/9e17304523f74cb97558cbb1e78c57e4f2045a14/superset/async_events/async_query_manager.py#L113'>superset/async_events/async_query_manager.py:113:36:</a> UP045 Use `X | None` for type annotations
... 148 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+102 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/backends/bigquery/__init__.py#L847'>ibis/backends/bigquery/__init__.py:847:48:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/graph.py#L171'>ibis/common/graph.py:171:46:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/graph.py#L203'>ibis/common/graph.py:203:50:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/graph.py#L275'>ibis/common/graph.py:275:41:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/graph.py#L310'>ibis/common/graph.py:310:47:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/graph.py#L359'>ibis/common/graph.py:359:47:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/graph.py#L379'>ibis/common/graph.py:379:17:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/graph.py#L380'>ibis/common/graph.py:380:18:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/graph.py#L418'>ibis/common/graph.py:418:17:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/graph.py#L419'>ibis/common/graph.py:419:18:</a> UP045 [*] Use `X | None` for type annotations
... 92 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1148 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/caches.py#L149'>libs/core/langchain_core/caches.py:149:36:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/caches.py#L167'>libs/core/langchain_core/caches.py:167:55:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/caches.py#L200'>libs/core/langchain_core/caches.py:200:62:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/caches.py#L52'>libs/core/langchain_core/caches.py:52:55:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/caches.py#L97'>libs/core/langchain_core/caches.py:97:62:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L104'>libs/core/langchain_core/callbacks/base.py:104:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L125'>libs/core/langchain_core/callbacks/base.py:125:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L141'>libs/core/langchain_core/callbacks/base.py:141:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L157'>libs/core/langchain_core/callbacks/base.py:157:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L173'>libs/core/langchain_core/callbacks/base.py:173:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L193'>libs/core/langchain_core/callbacks/base.py:193:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L209'>libs/core/langchain_core/callbacks/base.py:209:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L230'>libs/core/langchain_core/callbacks/base.py:230:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L231'>libs/core/langchain_core/callbacks/base.py:231:15:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L232'>libs/core/langchain_core/callbacks/base.py:232:19:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L257'>libs/core/langchain_core/callbacks/base.py:257:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L258'>libs/core/langchain_core/callbacks/base.py:258:15:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L259'>libs/core/langchain_core/callbacks/base.py:259:19:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L287'>libs/core/langchain_core/callbacks/base.py:287:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L288'>libs/core/langchain_core/callbacks/base.py:288:15:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L289'>libs/core/langchain_core/callbacks/base.py:289:19:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L29'>libs/core/langchain_core/callbacks/base.py:29:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L310'>libs/core/langchain_core/callbacks/base.py:310:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L311'>libs/core/langchain_core/callbacks/base.py:311:15:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L312'>libs/core/langchain_core/callbacks/base.py:312:19:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L333'>libs/core/langchain_core/callbacks/base.py:333:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L334'>libs/core/langchain_core/callbacks/base.py:334:15:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L335'>libs/core/langchain_core/callbacks/base.py:335:19:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L336'>libs/core/langchain_core/callbacks/base.py:336:17:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L361'>libs/core/langchain_core/callbacks/base.py:361:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L378'>libs/core/langchain_core/callbacks/base.py:378:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L396'>libs/core/langchain_core/callbacks/base.py:396:15:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L397'>libs/core/langchain_core/callbacks/base.py:397:19:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/langchain_core/callbacks/base.py#L46'>libs/core/langchain_core/callbacks/base.py:46:24:</a> UP045 Use `X | None` for type annotations
... 1114 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+13 -13 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L188'>src/latch/registry/record.py:188:57:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L188'>src/latch/registry/record.py:188:57:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L190'>src/latch/registry/record.py:190:64:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L190'>src/latch/registry/record.py:190:64:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L215'>src/latch/registry/record.py:215:53:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L215'>src/latch/registry/record.py:215:53:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L217'>src/latch/registry/record.py:217:60:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L217'>src/latch/registry/record.py:217:60:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L250'>src/latch/registry/record.py:250:10:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L250'>src/latch/registry/record.py:250:10:</a> UP045 Use `X | None` for type annotations
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+208 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L102'>lnbits/core/models.py:102:15:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L107'>lnbits/core/models.py:107:20:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L108'>lnbits/core/models.py:108:15:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L109'>lnbits/core/models.py:109:15:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L110'>lnbits/core/models.py:110:12:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L111'>lnbits/core/models.py:111:19:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L112'>lnbits/core/models.py:112:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L113'>lnbits/core/models.py:113:19:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L114'>lnbits/core/models.py:114:19:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L129'>lnbits/core/models.py:129:19:</a> UP045 Use `X | None` for type annotations
... 198 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (5 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP045 | 1552 | 1552 | 0 | 0 | 0 |
| UP007 | 92 | 0 | 92 | 0 | 0 |
| PLR6301 | 2 | 1 | 1 | 0 | 0 |
| D100 | 2 | 1 | 1 | 0 | 0 |
| RUF100 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Label `breaking` added by @MichaReiser on 2025-01-07 08:20_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_annotation.rs`:14 on 2025-01-07 09:10_

We should keep the documentation as is and instead add a Preview section that mentions that the rule now only covers `typing.Union` and that the `typing.Optional` is covered by a new rule

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_annotation.rs`:102 on 2025-01-07 09:11_

I think we can remove this note, considering that it's already covered by `UP007`. 

---

_Label `breaking` removed by @MichaReiser on 2025-01-07 09:26_

---

_Label `rule` added by @MichaReiser on 2025-01-07 09:26_

---

_Label `preview` added by @MichaReiser on 2025-01-07 09:26_

---

_Comment by @MichaReiser on 2025-01-07 09:27_

It's unclear to me why the ecosystem changes show more new UP045 violations than removed UP007 violations. Do we need to rebase this on main to get the `--no-fix` in ecosystem checks?

---

_@MichaReiser reviewed on 2025-01-07 09:27_

---

_Comment by @InSyncWithFoo on 2025-01-07 09:47_

> It's unclear to me why the ecosystem changes show more new UP045 violations than removed UP007 violations.

Some of the projects have `ignore = ["UP007"]` ([@Inbits/Inbits](https://github.com/lnbits/lnbits/blob/3900d2871d2e8ec5a00028eb4e2e59417c4fb08e/pyproject.toml#L192), [@ibis-project/ibis](https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/pyproject.toml#L538)). This PR seems to be making a good change, given the comments nearby.

---

_Comment by @MichaReiser on 2025-01-07 10:15_

Ah, that makes sense. 

---

_Merged by @MichaReiser on 2025-01-07 10:22_

---

_Closed by @MichaReiser on 2025-01-07 10:22_

---

_Branch deleted on 2025-01-07 14:49_

---

_Comment by @InSyncWithFoo on 2025-01-07 14:49_

@MichaReiser I think #11379 can be closed now.

---
