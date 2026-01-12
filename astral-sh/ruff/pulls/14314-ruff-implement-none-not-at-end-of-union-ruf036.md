```yaml
number: 14314
title: "[`ruff`] Implement `none-not-at-end-of-union` (`RUF036`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: none-not-at-end-of-union
created_at: 2024-11-13T11:48:39Z
updated_at: 2024-11-14T18:37:13Z
url: https://github.com/astral-sh/ruff/pull/14314
synced_at: 2026-01-12T15:55:47Z
```

# [`ruff`] Implement `none-not-at-end-of-union` (`RUF036`)

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ruff/issues/14290.

Naming pattern inspired by https://docs.astral.sh/ruff/rules/module-import-not-at-top-of-file/.

This rule would greatly benefit from an autofix, but that's for a follow-up PR.

## Test Plan

<!-- How was it tested? -->

`cargo test` and reviewed the ecosystem results (all good).


---

_Comment by @github-actions[bot] on 2024-11-13 12:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+397 -0 violations, +0 -0 fixes in 19 projects; 35 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/ef7d82f087e201e297625f2b135b4b7d61089f91/tests/test_utils.py#L762'>tests/test_utils.py:762:25:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/core/events.py#L597'>rasa/shared/core/events.py:597:48:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/shared/core/events.py#L624'>rasa/shared/core/events.py:624:31:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/rasa/utils/tensorflow/models.py#L247'>rasa/utils/tensorflow/models.py:247:35:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+122 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/f60886cf368b943120af20889b83704ccdbb8c91/airflow/auth/managers/base_auth_manager.py#L443'>airflow/auth/managers/base_auth_manager.py:443:36:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/f60886cf368b943120af20889b83704ccdbb8c91/airflow/cli/commands/task_command.py#L242'>airflow/cli/commands/task_command.py:242:6:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/f60886cf368b943120af20889b83704ccdbb8c91/airflow/cli/commands/task_command.py#L323'>airflow/cli/commands/task_command.py:323:46:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/f60886cf368b943120af20889b83704ccdbb8c91/airflow/decorators/__init__.pyi#L116'>airflow/decorators/__init__.pyi:116:23:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/f60886cf368b943120af20889b83704ccdbb8c91/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:25:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/f60886cf368b943120af20889b83704ccdbb8c91/airflow/decorators/__init__.pyi#L123'>airflow/decorators/__init__.pyi:123:21:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/f60886cf368b943120af20889b83704ccdbb8c91/airflow/decorators/__init__.pyi#L124'>airflow/decorators/__init__.pyi:124:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/f60886cf368b943120af20889b83704ccdbb8c91/airflow/decorators/__init__.pyi#L255'>airflow/decorators/__init__.pyi:255:23:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/f60886cf368b943120af20889b83704ccdbb8c91/airflow/decorators/__init__.pyi#L256'>airflow/decorators/__init__.pyi:256:25:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/f60886cf368b943120af20889b83704ccdbb8c91/airflow/decorators/__init__.pyi#L262'>airflow/decorators/__init__.pyi:262:21:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/f60886cf368b943120af20889b83704ccdbb8c91/airflow/decorators/__init__.pyi#L263'>airflow/decorators/__init__.pyi:263:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/f60886cf368b943120af20889b83704ccdbb8c91/airflow/example_dags/plugins/event_listener.py#L93'>airflow/example_dags/plugins/event_listener.py:93:76:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/f60886cf368b943120af20889b83704ccdbb8c91/airflow/executors/base_executor.py#L124'>airflow/executors/base_executor.py:124:13:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/f60886cf368b943120af20889b83704ccdbb8c91/airflow/executors/base_executor.py#L125'>airflow/executors/base_executor.py:125:11:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/f60886cf368b943120af20889b83704ccdbb8c91/airflow/executors/local_executor.py#L238'>airflow/executors/local_executor.py:238:20:</a> RUF036 `None` not at the end of the type annotation.
... 107 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/superset/config.py#L1127'>superset/config.py:1127:5:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/superset/config.py#L1723'>superset/config.py:1723:43:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/superset/config.py#L714'>superset/config.py:714:5:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/superset/db_engine_specs/gsheets.py#L166'>superset/db_engine_specs/gsheets.py:166:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/58f9be9b85cfd34f861d232cf834c96747133a39/superset/jinja_context.py#L84'>superset/jinja_context.py:84:16:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/websocket.py#L73'>src/bokeh/client/websocket.py:73:85:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/standalone.py#L84'>src/bokeh/embed/standalone.py:84:30:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/tornado.py#L231'>src/bokeh/util/tornado.py:231:26:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/registry/types.py#L45'>src/latch/registry/types.py:45:5:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/types/metadata.py#L419'>src/latch/types/metadata.py:419:5:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_cli/services/get.py#L38'>src/latch_cli/services/get.py:38:32:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_cli/services/launch.py#L135'>src/latch_cli/services/launch.py:135:46:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_cli/snakemake/config/utils.py#L12'>src/latch_cli/snakemake/config/utils.py:12:53:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_cli/snakemake/config/utils.py#L196'>src/latch_cli/snakemake/config/utils.py:196:56:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch_sdk_gql/utils.py#L38'>src/latch_sdk_gql/utils.py:38:59:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/payment_api.py#L172'>lnbits/core/views/payment_api.py:172:27:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/016ff553af24fa32fdebaf418b6afe23581bab71/pymilvus/client/abstract.py#L799'>pymilvus/client/abstract.py:799:40:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+59 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/ba4d1cfdda14bf521ff91d6ad432b21095c417fd/pandas/_libs/json.pyi#L14'>pandas/_libs/json.pyi:14:22:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/pandas-dev/pandas/blob/ba4d1cfdda14bf521ff91d6ad432b21095c417fd/pandas/_libs/tslibs/timedeltas.pyi#L147'>pandas/_libs/tslibs/timedeltas.pyi:147:36:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/pandas-dev/pandas/blob/ba4d1cfdda14bf521ff91d6ad432b21095c417fd/pandas/_libs/tslibs/timestamps.pyi#L30'>pandas/_libs/tslibs/timestamps.pyi:30:41:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/pandas-dev/pandas/blob/ba4d1cfdda14bf521ff91d6ad432b21095c417fd/pandas/core/arrays/datetimelike.py#L1040'>pandas/core/arrays/datetimelike.py:1040:45:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/pandas-dev/pandas/blob/ba4d1cfdda14bf521ff91d6ad432b21095c417fd/pandas/core/dtypes/cast.py#L182'>pandas/core/dtypes/cast.py:182:38:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/pandas-dev/pandas/blob/ba4d1cfdda14bf521ff91d6ad432b21095c417fd/pandas/core/dtypes/cast.py#L182'>pandas/core/dtypes/cast.py:182:65:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/pandas-dev/pandas/blob/ba4d1cfdda14bf521ff91d6ad432b21095c417fd/pandas/core/dtypes/dtypes.py#L1271'>pandas/core/dtypes/dtypes.py:1271:15:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/pandas-dev/pandas/blob/ba4d1cfdda14bf521ff91d6ad432b21095c417fd/pandas/core/generic.py#L6821'>pandas/core/generic.py:6821:15:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/pandas-dev/pandas/blob/ba4d1cfdda14bf521ff91d6ad432b21095c417fd/pandas/core/generic.py#L6823'>pandas/core/generic.py:6823:16:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/pandas-dev/pandas/blob/ba4d1cfdda14bf521ff91d6ad432b21095c417fd/pandas/core/generic.py#L7116'>pandas/core/generic.py:7116:15:</a> RUF036 `None` not at the end of the type annotation.
... 49 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/945953340898a7c0e81b6dadd0279059a487ecec/cibuildwheel/options.py#L719'>cibuildwheel/options.py:719:41:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+146 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/_ssl.pyi#L79'>stdlib/_ssl.pyi:79:58:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1726'>stdlib/ast.pyi:1726:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1736'>stdlib/ast.pyi:1736:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1746'>stdlib/ast.pyi:1746:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1756'>stdlib/ast.pyi:1756:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1765'>stdlib/ast.pyi:1765:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1774'>stdlib/ast.pyi:1774:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1783'>stdlib/ast.pyi:1783:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1793'>stdlib/ast.pyi:1793:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1805'>stdlib/ast.pyi:1805:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1814'>stdlib/ast.pyi:1814:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1823'>stdlib/ast.pyi:1823:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1832'>stdlib/ast.pyi:1832:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1840'>stdlib/ast.pyi:1840:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1848'>stdlib/ast.pyi:1848:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1856'>stdlib/ast.pyi:1856:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/ast.pyi#L1865'>stdlib/ast.pyi:1865:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/5052fa2f18db4493892e0f2775030683c9d06531/stdlib/asyncio/base_events.pyi#L27'>stdlib/asyncio/base_events.pyi:27:33:</a> RUF036 `None` not at the end of the type annotation.
... 128 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/utils/env/env_manager.py#L117'>src/poetry/utils/env/env_manager.py:117:35:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/utils/env/env_manager.py#L142'>src/poetry/utils/env/env_manager.py:142:13:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/utils/env/env_manager.py#L96'>src/poetry/utils/env/env_manager.py:96:44:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/4547f22aa38892bf1daf09ee6ed59921ec236373/rotkehlchen/api/rest.py#L3189'>rotkehlchen/api/rest.py:3189:55:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/rotki/rotki/blob/4547f22aa38892bf1daf09ee6ed59921ec236373/rotkehlchen/chain/ethereum/modules/compound/utils.py#L13'>rotkehlchen/chain/ethereum/modules/compound/utils.py:13:44:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/rotki/rotki/blob/4547f22aa38892bf1daf09ee6ed59921ec236373/rotkehlchen/db/dbhandler.py#L433'>rotkehlchen/db/dbhandler.py:433:16:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/rotki/rotki/blob/4547f22aa38892bf1daf09ee6ed59921ec236373/rotkehlchen/db/filtering.py#L1657'>rotkehlchen/db/filtering.py:1657:34:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/9e20ec612893541d253674e688269e2495be0a12/tests/conftest.py#L143'>tests/conftest.py:143:104:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/bfbd625ac0fb2d1b68b3c7754ed0d1fb1cef0e5e/tests/test_cmake_config.py#L25'>tests/test_cmake_config.py:25:26:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/2de648df021116c94faafd0c1ea6296821125e52/confirmation/models.py#L132'>confirmation/models.py:132:32:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/zulip/zulip/blob/2de648df021116c94faafd0c1ea6296821125e52/confirmation/models.py#L175'>confirmation/models.py:175:32:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/zulip/zulip/blob/2de648df021116c94faafd0c1ea6296821125e52/zerver/lib/markdown/__init__.py#L2165'>zerver/lib/markdown/__init__.py:2165:59:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/zulip/zulip/blob/2de648df021116c94faafd0c1ea6296821125e52/zerver/lib/remote_server.py#L125'>zerver/lib/remote_server.py:125:49:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/zulip/zulip/blob/2de648df021116c94faafd0c1ea6296821125e52/zerver/lib/webhooks/common.py#L158'>zerver/lib/webhooks/common.py:158:40:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/zulip/zulip/blob/2de648df021116c94faafd0c1ea6296821125e52/zerver/management/commands/send_webhook_fixture_message.py#L54'>zerver/management/commands/send_webhook_fixture_message.py:54:45:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/zulip/zulip/blob/2de648df021116c94faafd0c1ea6296821125e52/zerver/management/commands/send_webhook_fixture_message.py#L54'>zerver/management/commands/send_webhook_fixture_message.py:54:60:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/zulip/zulip/blob/2de648df021116c94faafd0c1ea6296821125e52/zerver/tests/test_push_notifications.py#L2279'>zerver/tests/test_push_notifications.py:2279:61:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/zulip/zulip/blob/2de648df021116c94faafd0c1ea6296821125e52/zerver/tests/test_users.py#L860'>zerver/tests/test_users.py:860:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/zulip/zulip/blob/2de648df021116c94faafd0c1ea6296821125e52/zerver/tornado/django_api.py#L41'>zerver/tornado/django_api.py:41:18:</a> RUF036 `None` not at the end of the type annotation.
... 1 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF036 | 397 | 397 | 0 | 0 | 0 |

</p>
</details>




---

_@MichaReiser reviewed on 2024-11-14 08:05_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:67 on 2024-11-14 08:05_

I think we only need to track the last-none value in that case or are there cases where the last collected `None` value wouldn't be the last-`None` in the expression? 


I think we could simplify the entire implementation by simply doing

```
let mut find_none = |expr: &'a Expr, _parent: &Expr| {
        if matches!(expr, Expr::NoneLiteral(none)) {
            last_none = Some(none);
        } else {
					last_none = None;
        }
    };
```

we then don't need to check if `last_none` is the last expression. That's already given from the iteration order.

---

_@MichaReiser reviewed on 2024-11-14 08:06_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:51 on 2024-11-14 08:06_

Nit: You could consider just pushing the inner `ExprNoneLiteral`. It retains more information that all elements in `none_exprs` aren't just arbitrary expressions, but `ExprNoneLiteral` only

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:67 on 2024-11-14 08:25_

Wouldn’t that give false positives when None is not present at all? 

---

_@sbrugman reviewed on 2024-11-14 08:25_

---

_@MichaReiser reviewed on 2024-11-14 08:27_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:67 on 2024-11-14 08:27_

Yes, we still have to check that `last_none` is `Some` and otherwise return (we need to keep the equivalent of the `is_empty` check)

---

_Label `rule` added by @MichaReiser on 2024-11-14 08:28_

---

_Label `preview` added by @MichaReiser on 2024-11-14 08:28_

---

_@sbrugman reviewed on 2024-11-14 10:39_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:67 on 2024-11-14 10:39_

I'll have a look. I think the reason that prevents us to do it exactly like this is that we return if `None`, not return if `Some`.

---

_@MichaReiser reviewed on 2024-11-14 10:52_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:67 on 2024-11-14 10:52_

Oh, we do need to keep the list for the diagnostic itself... 


In this case, we can only simplify 

```
if none_exprs.last(|none_expr| *none_expr == last_expr) {
```

---

_@MichaReiser reviewed on 2024-11-14 10:54_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:44 on 2024-11-14 10:54_

Nit: Consider using a smallvec here so that we avoid allocating in all valid cases (where there is a single `None`) at the end.

---

_@sbrugman reviewed on 2024-11-14 14:48_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:51 on 2024-11-14 14:48_

I ended up not applying this now that there is a comparison between `last_expr` and `none_expr`. Let me know if there if this advice still can be applied.

---

_Merged by @MichaReiser on 2024-11-14 18:37_

---

_Closed by @MichaReiser on 2024-11-14 18:37_

---
