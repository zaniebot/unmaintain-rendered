```yaml
number: 15139
title: "Autofix for `none-not-at-end-of-union (RUF036)`"
type: pull_request
state: closed
author: harupy
labels:
  - rule
  - fixes
  - preview
assignees: []
base: main
head: RUF036-autofix
created_at: 2024-12-25T03:27:55Z
updated_at: 2025-04-28T07:05:57Z
url: https://github.com/astral-sh/ruff/pull/15139
synced_at: 2026-01-12T15:55:50Z
```

# Autofix for `none-not-at-end-of-union (RUF036)`

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Close #15136. Implement autofix for `none-not-at-end-of-union (RUF036)`

## Test Plan

<!-- How was it tested? -->

Existing and new tests

---

_Comment by @github-actions[bot] on 2024-12-25 03:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -82 violations, +390 -0 fixes in 18 projects; 37 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2867a91ca15f06f6f5387cd74cdb6b72472865d5/tests/test_utils.py#L782'>tests/test_utils.py:782:25:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/DisnakeDev/disnake/blob/2867a91ca15f06f6f5387cd74cdb6b72472865d5/tests/test_utils.py#L782'>tests/test_utils.py:782:25:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/events.py#L597'>rasa/shared/core/events.py:597:48:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/events.py#L597'>rasa/shared/core/events.py:597:48:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/events.py#L624'>rasa/shared/core/events.py:624:31:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/events.py#L624'>rasa/shared/core/events.py:624:31:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/tensorflow/models.py#L247'>rasa/utils/tensorflow/models.py:247:35:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/tensorflow/models.py#L247'>rasa/utils/tensorflow/models.py:247:35:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +242 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/api_fastapi/common/parameters.py#L641'>airflow/api_fastapi/common/parameters.py:641:22:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/api_fastapi/common/parameters.py#L641'>airflow/api_fastapi/common/parameters.py:641:22:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/auth/managers/base_auth_manager.py#L448'>airflow/auth/managers/base_auth_manager.py:448:36:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/auth/managers/base_auth_manager.py#L448'>airflow/auth/managers/base_auth_manager.py:448:36:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/cli/commands/remote_commands/task_command.py#L250'>airflow/cli/commands/remote_commands/task_command.py:250:71:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/cli/commands/remote_commands/task_command.py#L250'>airflow/cli/commands/remote_commands/task_command.py:250:71:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/cli/commands/remote_commands/task_command.py#L336'>airflow/cli/commands/remote_commands/task_command.py:336:46:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/cli/commands/remote_commands/task_command.py#L336'>airflow/cli/commands/remote_commands/task_command.py:336:46:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:23:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:23:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L118'>airflow/decorators/__init__.pyi:118:25:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L118'>airflow/decorators/__init__.pyi:118:25:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L124'>airflow/decorators/__init__.pyi:124:21:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L124'>airflow/decorators/__init__.pyi:124:21:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L125'>airflow/decorators/__init__.pyi:125:26:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L125'>airflow/decorators/__init__.pyi:125:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L245'>airflow/decorators/__init__.pyi:245:23:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L245'>airflow/decorators/__init__.pyi:245:23:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L246'>airflow/decorators/__init__.pyi:246:25:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L246'>airflow/decorators/__init__.pyi:246:25:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L252'>airflow/decorators/__init__.pyi:252:21:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L252'>airflow/decorators/__init__.pyi:252:21:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L253'>airflow/decorators/__init__.pyi:253:26:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/decorators/__init__.pyi#L253'>airflow/decorators/__init__.pyi:253:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/23d6d0db70735c8a51981ca4e97bb278eaca446d/airflow/example_dags/plugins/event_listener.py#L93'>airflow/example_dags/plugins/event_listener.py:93:76:</a> RUF036 [*] `None` not at the end of the type annotation.
... 217 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +10 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/a193d790b23eccd32e843d005d841d6bc590c705/superset/config.py#L1126'>superset/config.py:1126:5:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/superset/blob/a193d790b23eccd32e843d005d841d6bc590c705/superset/config.py#L1126'>superset/config.py:1126:5:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/a193d790b23eccd32e843d005d841d6bc590c705/superset/config.py#L1728'>superset/config.py:1728:43:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/superset/blob/a193d790b23eccd32e843d005d841d6bc590c705/superset/config.py#L1728'>superset/config.py:1728:43:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/a193d790b23eccd32e843d005d841d6bc590c705/superset/config.py#L713'>superset/config.py:713:5:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/superset/blob/a193d790b23eccd32e843d005d841d6bc590c705/superset/config.py#L713'>superset/config.py:713:5:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/a193d790b23eccd32e843d005d841d6bc590c705/superset/db_engine_specs/gsheets.py#L166'>superset/db_engine_specs/gsheets.py:166:26:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/superset/blob/a193d790b23eccd32e843d005d841d6bc590c705/superset/db_engine_specs/gsheets.py#L166'>superset/db_engine_specs/gsheets.py:166:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/a193d790b23eccd32e843d005d841d6bc590c705/superset/jinja_context.py#L84'>superset/jinja_context.py:84:16:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/superset/blob/a193d790b23eccd32e843d005d841d6bc590c705/superset/jinja_context.py#L84'>superset/jinja_context.py:84:16:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/websocket.py#L73'>src/bokeh/client/websocket.py:73:85:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/websocket.py#L73'>src/bokeh/client/websocket.py:73:85:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/standalone.py#L84'>src/bokeh/embed/standalone.py:84:30:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/standalone.py#L84'>src/bokeh/embed/standalone.py:84:30:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/tornado.py#L231'>src/bokeh/util/tornado.py:231:26:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/tornado.py#L231'>src/bokeh/util/tornado.py:231:26:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +14 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/types.py#L45'>src/latch/registry/types.py:45:5:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/types.py#L45'>src/latch/registry/types.py:45:5:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/types/metadata.py#L419'>src/latch/types/metadata.py:419:5:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/types/metadata.py#L419'>src/latch/types/metadata.py:419:5:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/services/get.py#L38'>src/latch_cli/services/get.py:38:32:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/services/get.py#L38'>src/latch_cli/services/get.py:38:32:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/services/launch.py#L135'>src/latch_cli/services/launch.py:135:46:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/services/launch.py#L135'>src/latch_cli/services/launch.py:135:46:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/snakemake/config/utils.py#L12'>src/latch_cli/snakemake/config/utils.py:12:53:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/snakemake/config/utils.py#L12'>src/latch_cli/snakemake/config/utils.py:12:53:</a> RUF036 `None` not at the end of the type annotation.
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/payment_api.py#L172'>lnbits/core/views/payment_api.py:172:27:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/payment_api.py#L172'>lnbits/core/views/payment_api.py:172:27:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/a6fbc66270b0c8ee74236bde6afdb229ac5ab648/pymilvus/client/abstract.py#L806'>pymilvus/client/abstract.py:806:40:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/milvus-io/pymilvus/blob/a6fbc66270b0c8ee74236bde6afdb229ac5ab648/pymilvus/client/abstract.py#L806'>pymilvus/client/abstract.py:806:40:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -31 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/8a5344742c5165b2595f7ccca9e17d5eff7f7886/pandas/core/dtypes/cast.py#L182'>pandas/core/dtypes/cast.py:182:38:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/8a5344742c5165b2595f7ccca9e17d5eff7f7886/pandas/core/dtypes/cast.py#L182'>pandas/core/dtypes/cast.py:182:65:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/8a5344742c5165b2595f7ccca9e17d5eff7f7886/pandas/core/dtypes/dtypes.py#L1269'>pandas/core/dtypes/dtypes.py:1269:15:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/8a5344742c5165b2595f7ccca9e17d5eff7f7886/pandas/core/groupby/groupby.py#L2784'>pandas/core/groupby/groupby.py:2784:24:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/8a5344742c5165b2595f7ccca9e17d5eff7f7886/pandas/core/groupby/groupby.py#L4248'>pandas/core/groupby/groupby.py:4248:22:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/8a5344742c5165b2595f7ccca9e17d5eff7f7886/pandas/core/reshape/encoding.py#L367'>pandas/core/reshape/encoding.py:367:10:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/8a5344742c5165b2595f7ccca9e17d5eff7f7886/pandas/core/reshape/encoding.py#L368'>pandas/core/reshape/encoding.py:368:23:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/8a5344742c5165b2595f7ccca9e17d5eff7f7886/pandas/io/_util.py#L69'>pandas/io/_util.py:69:41:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/8a5344742c5165b2595f7ccca9e17d5eff7f7886/pandas/io/excel/_util.py#L181'>pandas/io/excel/_util.py:181:6:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/8a5344742c5165b2595f7ccca9e17d5eff7f7886/pandas/io/parsers/readers.py#L101'>pandas/io/parsers/readers.py:101:20:</a> RUF036 `None` not at the end of the type annotation.
... 21 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/148b87f6fedd1efef6c20af1d18736aa7496888e/cibuildwheel/options.py#L753'>cibuildwheel/options.py:753:41:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/pypa/cibuildwheel/blob/148b87f6fedd1efef6c20af1d18736aa7496888e/cibuildwheel/options.py#L753'>cibuildwheel/options.py:753:41:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -51 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/dee35e22bb86835fb7209f074dc78e26b29c52e5/stdlib/ast.pyi#L1726'>stdlib/ast.pyi:1726:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/dee35e22bb86835fb7209f074dc78e26b29c52e5/stdlib/ast.pyi#L1736'>stdlib/ast.pyi:1736:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/dee35e22bb86835fb7209f074dc78e26b29c52e5/stdlib/ast.pyi#L1746'>stdlib/ast.pyi:1746:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/dee35e22bb86835fb7209f074dc78e26b29c52e5/stdlib/ast.pyi#L1756'>stdlib/ast.pyi:1756:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/dee35e22bb86835fb7209f074dc78e26b29c52e5/stdlib/ast.pyi#L1765'>stdlib/ast.pyi:1765:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/dee35e22bb86835fb7209f074dc78e26b29c52e5/stdlib/ast.pyi#L1774'>stdlib/ast.pyi:1774:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/dee35e22bb86835fb7209f074dc78e26b29c52e5/stdlib/ast.pyi#L1783'>stdlib/ast.pyi:1783:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/dee35e22bb86835fb7209f074dc78e26b29c52e5/stdlib/ast.pyi#L1793'>stdlib/ast.pyi:1793:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/dee35e22bb86835fb7209f074dc78e26b29c52e5/stdlib/ast.pyi#L1805'>stdlib/ast.pyi:1805:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/dee35e22bb86835fb7209f074dc78e26b29c52e5/stdlib/ast.pyi#L1814'>stdlib/ast.pyi:1814:26:</a> RUF036 `None` not at the end of the type annotation.
... 41 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF036 | 472 | 0 | 82 | 390 | 0 |

</p>
</details>




---

_@harupy reviewed on 2024-12-25 07:47_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:103 on 2024-12-25 07:47_

Should we set the fix to all the diagnostics?

---

_Label `fixes` added by @AlexWaygood on 2024-12-25 11:04_

---

_Comment by @dylwil3 on 2024-12-25 14:37_

Thanks for working on this! Can you add some tests with nested and mixed uses of unions? Like these:

```python
def f(x: None | Union[None ,int]): ...

def g(x: int | (str | None) | list): ...

def h(x: Union[int, Union[None, list | set]]: ...
```

also the examples from `poetry` in the ecosystem check like:

```python
    def _detect_active_python(io: None | IO = None) -> Path | None: ...
```

---

_Comment by @dylwil3 on 2024-12-25 14:46_

It looks like in the ecosystem check a bunch of violations were removed with no fix, so I'm wondering if a syntax error was introduced by the fix. Maybe when there is a default argument something goes wrong?

---

_Comment by @Daverball on 2024-12-25 21:46_

> It looks like in the ecosystem check a bunch of violations were removed with no fix, so I'm wondering if a syntax error was introduced by the fix. Maybe when there is a default argument something goes wrong?

I think this happens for projects where the `ruff.toml` or `pyproject.toml` contains `fix = true`. I also got tripped up by this behavior in some of my pull requests. I think it would be a good idea to change the ecosystem script to pass `--no-fix`, so this doesn't happen, since the output will always be interpreted incorrectly when fixes have been applied (this can also lead to other confusing things like new E501 flags, because the fix caused a line to get too long, or an unrelated unfixable violation getting flagged, because the fix caused the violation to be slightly offset, so you see a +1 -1 for that violation).

---

_@MichaReiser reviewed on 2024-12-26 09:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:109 on 2024-12-26 09:33_

```suggestion
						let (range, separator) = if let Expr::Subscript(ast::ExprSubscript { slice, .. }) = union {
							(slice.range(), ", "))
						} else {
							(union.range, " | ")
						}
            
            let applicability = if checker.comment_ranges().intersects(range) {
                Applicability::Unsafe
            } else {
                Applicability::Safe
            };
            let fix = Fix::applicable_edit(
                Edit::range_replacement(elements.join(separator), range),
                applicability,
            )
            diagnostic.set_fix(fix);
```

---

_@MichaReiser reviewed on 2024-12-26 09:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:103 on 2024-12-26 09:33_

Can you tell me a bit more why you think we should (or shouldn't) set the fix for all violations?

---

_@harupy reviewed on 2024-12-26 09:43_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:103 on 2024-12-26 09:43_

@MichaReiser For example, this part looks like the same fix is set to all the diagnostics:

https://github.com/astral-sh/ruff/blob/8b9c843c727de463efccafc7e012414b93458dc7/crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs#L126-L129

---

_Review requested from @MichaReiser by @harupy on 2024-12-27 12:34_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:103 on 2024-12-30 13:49_

I'm actually leaning towards only emitting a single diagnostic rather than emitting multiple diagnostics. This should also simplify the code a bit because we can change `none_exprs` to an `Option` only tracking the `last_none` rather than all `None` values

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF036_RUF036.py.snap`:62 on 2024-12-30 13:50_

We should avoid removing duplicate `None` values. While the second `None` is useless, it's not what the rule is about

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF036_RUF036.py.snap`:208 on 2024-12-30 13:55_

The fix here looks incorrect. It flattens the nested `U` types.

---

_@MichaReiser requested changes on 2024-12-30 14:00_

Thanks for working on this. 

The fix now seems to remove duplicate `None` elements and I think it also isn't handling nesting correctly - see the inline comments in the tests. 

I tried to implement a fix that does more narrow edits than regenerating the entire union / subscript but the whitespace handling isn't perfect yet and I haven't looked into some edge cases like quoted annotations or parentheses. But maybe it's useful for you

```patch
Index: crates/ruff_python_parser/src/lib.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_parser/src/lib.rs b/crates/ruff_python_parser/src/lib.rs
--- a/crates/ruff_python_parser/src/lib.rs	(revision aa1a5cb2c7ef0dc3b38595a92285066721e626c0)
+++ b/crates/ruff_python_parser/src/lib.rs	(date 1735565508471)
@@ -478,6 +478,35 @@
             }
         }
     }
+
+    /// Returns a slice of tokens before the given [`TextSize`] offset.
+    ///
+    /// If the given offset is between two tokens, the returned slice will start from the following
+    /// token. In other words, if the offset is between the end of previous token and start of next
+    /// token, the returned slice will start from the next token.
+    ///
+    /// # Panics
+    ///
+    /// If the given offset is inside a token range.
+    pub fn before(&self, offset: TextSize) -> &[Token] {
+        match self.binary_search_by(|token| token.start().cmp(&offset)) {
+            Ok(idx) => &self[..idx],
+            Err(idx) => {
+                if let Some(prev) = self.get(idx.saturating_sub(1)) {
+                    // If it's equal to the end offset, then it's at a token boundary which is
+                    // valid. If it's greater than the end offset, then it's in the gap between
+                    // the tokens which is valid as well.
+                    assert!(
+                        offset >= prev.end(),
+                        "Offset {:?} is inside a token range {:?}",
+                        offset,
+                        prev.range()
+                    );
+                }
+                &self[..idx]
+            }
+        }
+    }
 }
 
 impl<'a> IntoIterator for &'a Tokens {
Index: crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs b/crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs
--- a/crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs	(revision aa1a5cb2c7ef0dc3b38595a92285066721e626c0)
+++ b/crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs	(date 1735567068721)
@@ -1,13 +1,14 @@
+use crate::checkers::ast::Checker;
 use itertools::{Itertools, Position};
 use ruff_diagnostics::{Applicability, Diagnostic, Edit, Fix, FixAvailability, Violation};
 use ruff_macros::{derive_message_formats, ViolationMetadata};
 use ruff_python_ast::{self as ast, Expr};
+use ruff_python_parser::TokenKind;
 use ruff_python_semantic::analyze::typing::traverse_union;
-use ruff_text_size::Ranged;
+use ruff_python_trivia::{SimpleTokenKind, SimpleTokenizer};
+use ruff_text_size::{Ranged, TextRange, TextSlice};
 use smallvec::SmallVec;
 
-use crate::checkers::ast::Checker;
-
 /// ## What it does
 /// Checks for type annotations where `None` is not at the end of an union.
 ///
@@ -49,15 +50,13 @@
 /// RUF036
 pub(crate) fn none_not_at_end_of_union<'a>(checker: &mut Checker, union: &'a Expr) {
     let semantic = checker.semantic();
-    let mut none_exprs: SmallVec<[&Expr; 1]> = SmallVec::new();
-    let mut non_none_exprs: SmallVec<[&Expr; 1]> = SmallVec::new();
-    let mut last_expr: Option<&Expr> = None;
+    let mut last_none = None;
+    let mut last_expr: Option<&'a Expr> = None;
     let mut find_none = |expr: &'a Expr, _parent: &Expr| {
-        if matches!(expr, Expr::NoneLiteral(_)) {
-            none_exprs.push(expr);
-        } else {
-            non_none_exprs.push(expr);
+        if expr.is_none_literal_expr() {
+            last_none = Some(expr);
         }
+
         last_expr = Some(expr);
     };
 
@@ -69,40 +68,72 @@
     };
 
     // There must be at least one `None` expression.
-    let Some(last_none) = none_exprs.last() else {
+    let Some(last_none) = last_none else {
         return;
     };
 
     // If any of the `None` literals is last we do not emit.
-    if *last_none == last_expr {
+    if last_none == last_expr {
         return;
     }
 
-    for (pos, none_expr) in none_exprs.iter().with_position() {
-        let mut diagnostic = Diagnostic::new(NoneNotAtEndOfUnion, none_expr.range());
-        if matches!(pos, Position::Last | Position::Only) {
-            let mut elements = non_none_exprs
-                .iter()
-                .map(|expr| checker.locator().slice(expr.range()).to_string())
-                .chain(std::iter::once("None".to_string()));
-            let (range, separator) =
-                if let Expr::Subscript(ast::ExprSubscript { slice, .. }) = union {
-                    (slice.range(), ", ")
-                } else {
-                    (union.range(), " | ")
-                };
-            let applicability = if checker.comment_ranges().intersects(range) {
-                Applicability::Unsafe
-            } else {
-                Applicability::Safe
-            };
-            let fix = Fix::applicable_edit(
-                Edit::range_replacement(elements.join(separator), range),
-                applicability,
-            );
-            diagnostic.set_fix(fix);
-        }
+    let range = if let Expr::Subscript(ast::ExprSubscript { slice, .. }) = union {
+        slice.range()
+    } else {
+        union.range()
+    };
+
+    let preceding_separator = checker.tokens().before(last_none.start());
+
+    let applicability = if checker.comment_ranges().intersects(range) {
+        Applicability::Unsafe
+    } else {
+        Applicability::Safe
+    };
+
+    // ```python
+    // int | None | str
+    // ```
+    let (insertion, range) = if let Some(last) = preceding_separator
+        .last()
+        .filter(|last| matches!(last.kind(), TokenKind::Comma | TokenKind::Vbar))
+    {
+        let range = TextRange::new(last.start(), last_none.end());
+        (
+            Edit::insertion(checker.source().slice(range).to_string(), last_expr.end()),
+            range,
+        )
+    } else {
+        // ```python
+        // None | int
+        // int | (None | str) | Literal[4]
+        // ```
+        let separator_after = checker
+            .tokens()
+            .after(last_none.end())
+            .first()
+            .filter(|token| matches!(token.kind(), TokenKind::Comma | TokenKind::Vbar))
+            .unwrap();
+        (
+            Edit::insertion(
+                format!(
+                    "{separator} None",
+                    separator = if separator_after.kind() == TokenKind::Comma {
+                        ","
+                    } else {
+                        "|"
+                    }
+                ),
+                last_expr.end(),
+            ),
+            TextRange::new(last_none.start(), separator_after.end()),
+        )
+    };
+
+    let fix = Fix::applicable_edits(insertion, [Edit::range_deletion(range)], applicability);
+
+    let mut diagnostic = Diagnostic::new(NoneNotAtEndOfUnion, last_none.range());
+    diagnostic.set_fix(fix);
 
-        checker.diagnostics.push(diagnostic);
-    }
+    checker.diagnostics.push(diagnostic);
 }
```

---

_Label `rule` added by @dhruvmanila on 2025-01-02 05:38_

---

_Label `preview` added by @dhruvmanila on 2025-01-02 05:38_

---

_@harupy reviewed on 2025-01-05 11:06_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/ruff/rules/none_not_at_end_of_union.rs`:103 on 2025-01-05 11:06_

A single diagnostic makes sense.

---

_@harupy reviewed on 2025-01-06 14:35_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF036_RUF036.py.snap`:208 on 2025-01-06 14:35_

@MichaReiser Do we need to fix this case? Can we only generate a fix when the type annotation has a single `None` and no nested unions?

---

_@MichaReiser reviewed on 2025-01-07 07:39_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF036_RUF036.py.snap`:208 on 2025-01-07 07:39_

I don't think we necessarily have to but we at least shouldn't provide a fix for those cases.

---

_Comment by @MichaReiser on 2025-04-28 07:05_

This PR has become stale. I'll close it. Feel free to open a new PR if you are interested in implementing this fix, but take the feedback into acocunt.

---

_Closed by @MichaReiser on 2025-04-28 07:05_

---
