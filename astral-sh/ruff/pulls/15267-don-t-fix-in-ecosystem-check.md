```yaml
number: 15267
title: "Don't fix in ecosystem check"
type: pull_request
state: merged
author: eclbg
labels:
  - ci
assignees: []
merged: true
base: main
head: ecosystem-check-no-fix
created_at: 2025-01-05T09:34:17Z
updated_at: 2025-01-06T09:21:35Z
url: https://github.com/astral-sh/ruff/pull/15267
synced_at: 2026-01-10T20:34:00Z
```

# Don't fix in ecosystem check

---

_Pull request opened by @eclbg on 2025-01-05 09:34_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Close #15146  
Available fixes won't be applied in ecosystem checks even when the checked repository has `fix = true` in their settings. This way the check output better reflects the actual changes in a given branch.

## Test Plan

<!-- How was it tested? -->

I've run the ecosystem checks locally and compared the outputs before and after the change. I used a build from `main` and a build from https://github.com/astral-sh/ruff/pull/15139, which is where the need to make this change was identified. The branch in question only adds a fix, so it is expected that the ecosystem checks only find new fixes. These are the results (unfold to see full):  

<details><summary>Before the change: +0 -133 violations, +390 -0 fixes in 19 projects; 36 projects unchanged</summary>

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/d847f505441d7ccf1ec4b63645ba8eae2cdc8882/tests/test_utils.py#L782'>tests/test_utils.py:782:25:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/DisnakeDev/disnake/blob/d847f505441d7ccf1ec4b63645ba8eae2cdc8882/tests/test_utils.py#L782'>tests/test_utils.py:782:25:</a> RUF036 `None` not at the end of the type annotation.
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
+ <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/api_fastapi/common/parameters.py#L641'>airflow/api_fastapi/common/parameters.py:641:22:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/api_fastapi/common/parameters.py#L641'>airflow/api_fastapi/common/parameters.py:641:22:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/auth/managers/base_auth_manager.py#L448'>airflow/auth/managers/base_auth_manager.py:448:36:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/auth/managers/base_auth_manager.py#L448'>airflow/auth/managers/base_auth_manager.py:448:36:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/cli/commands/remote_commands/task_command.py#L250'>airflow/cli/commands/remote_commands/task_command.py:250:71:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/cli/commands/remote_commands/task_command.py#L250'>airflow/cli/commands/remote_commands/task_command.py:250:71:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/cli/commands/remote_commands/task_command.py#L336'>airflow/cli/commands/remote_commands/task_command.py:336:46:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/cli/commands/remote_commands/task_command.py#L336'>airflow/cli/commands/remote_commands/task_command.py:336:46:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:23:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:23:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/decorators/__init__.pyi#L118'>airflow/decorators/__init__.pyi:118:25:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/decorators/__init__.pyi#L118'>airflow/decorators/__init__.pyi:118:25:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/decorators/__init__.pyi#L124'>airflow/decorators/__init__.pyi:124:21:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/decorators/__init__.pyi#L124'>airflow/decorators/__init__.pyi:124:21:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/decorators/__init__.pyi#L125'>airflow/decorators/__init__.pyi:125:26:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/decorators/__init__.pyi#L125'>airflow/decorators/__init__.pyi:125:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/decorators/__init__.pyi#L245'>airflow/decorators/__init__.pyi:245:23:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/decorators/__init__.pyi#L245'>airflow/decorators/__init__.pyi:245:23:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/decorators/__init__.pyi#L246'>airflow/decorators/__init__.pyi:246:25:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/decorators/__init__.pyi#L246'>airflow/decorators/__init__.pyi:246:25:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/decorators/__init__.pyi#L252'>airflow/decorators/__init__.pyi:252:21:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/decorators/__init__.pyi#L252'>airflow/decorators/__init__.pyi:252:21:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/c81610b7425ee65b7118e2256d5a9efe087dd080/airflow/decorators/__init__.pyi#L253'>airflow/decorators/__init__.pyi:253:26:</a> RUF036 [*] `None` not at the end of the type annotation.
... 219 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +10 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/config.py#L1126'>superset/config.py:1126:5:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/config.py#L1126'>superset/config.py:1126:5:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/config.py#L1728'>superset/config.py:1728:43:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/config.py#L1728'>superset/config.py:1728:43:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/config.py#L713'>superset/config.py:713:5:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/config.py#L713'>superset/config.py:713:5:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/db_engine_specs/gsheets.py#L166'>superset/db_engine_specs/gsheets.py:166:26:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/db_engine_specs/gsheets.py#L166'>superset/db_engine_specs/gsheets.py:166:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/jinja_context.py#L84'>superset/jinja_context.py:84:16:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/jinja_context.py#L84'>superset/jinja_context.py:84:16:</a> RUF036 `None` not at the end of the type annotation.
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
+ <a href='https://github.com/milvus-io/pymilvus/blob/c70d44c45e7f96761d1d3808c7d7401eb65d5afe/pymilvus/client/abstract.py#L810'>pymilvus/client/abstract.py:810:40:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/milvus-io/pymilvus/blob/c70d44c45e7f96761d1d3808c7d7401eb65d5afe/pymilvus/client/abstract.py#L810'>pymilvus/client/abstract.py:810:40:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -41 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/core/dtypes/cast.py#L182'>pandas/core/dtypes/cast.py:182:38:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/core/dtypes/cast.py#L182'>pandas/core/dtypes/cast.py:182:65:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/core/dtypes/dtypes.py#L1269'>pandas/core/dtypes/dtypes.py:1269:15:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/core/generic.py#L6829'>pandas/core/generic.py:6829:15:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/core/generic.py#L6831'>pandas/core/generic.py:6831:16:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/core/generic.py#L7124'>pandas/core/generic.py:7124:15:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/core/generic.py#L7126'>pandas/core/generic.py:7126:16:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/core/generic.py#L7134'>pandas/core/generic.py:7134:15:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/core/generic.py#L7136'>pandas/core/generic.py:7136:16:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/core/generic.py#L7144'>pandas/core/generic.py:7144:15:</a> RUF036 `None` not at the end of the type annotation.
... 31 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/9358823214b4b995427de88228a15ba7a857c678/cibuildwheel/options.py#L753'>cibuildwheel/options.py:753:41:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/pypa/cibuildwheel/blob/9358823214b4b995427de88228a15ba7a857c678/cibuildwheel/options.py#L753'>cibuildwheel/options.py:753:41:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -89 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/_interpreters.pyi#L26'>stdlib/_interpreters.pyi:26:6:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/_ssl.pyi#L79'>stdlib/_ssl.pyi:79:58:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1743'>stdlib/ast.pyi:1743:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1753'>stdlib/ast.pyi:1753:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1763'>stdlib/ast.pyi:1763:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1773'>stdlib/ast.pyi:1773:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1782'>stdlib/ast.pyi:1782:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1791'>stdlib/ast.pyi:1791:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1800'>stdlib/ast.pyi:1800:26:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1810'>stdlib/ast.pyi:1810:26:</a> RUF036 `None` not at the end of the type annotation.
... 79 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/utils/env/env_manager.py#L117'>src/poetry/utils/env/env_manager.py:117:35:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/utils/env/env_manager.py#L142'>src/poetry/utils/env/env_manager.py:142:13:</a> RUF036 `None` not at the end of the type annotation.
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/utils/env/env_manager.py#L96'>src/poetry/utils/env/env_manager.py:96:44:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF036 | 523 | 0 | 133 | 390 | 0 |

</p>
</details>
</details>

<details><summary>After the change: +0 -0 violations, +786 -0 fixes in 19 projects; 36 projects unchanged</summary>
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +786 -0 fixes in 19 projects; 36 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/d847f505441d7ccf1ec4b63645ba8eae2cdc8882/tests/test_utils.py#L782'>tests/test_utils.py:782:25:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/DisnakeDev/disnake/blob/d847f505441d7ccf1ec4b63645ba8eae2cdc8882/tests/test_utils.py#L782'>tests/test_utils.py:782:25:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
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
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/55c7741b449bc81d923675b6220474fbfdda9d95/airflow/api_fastapi/common/parameters.py#L641'>airflow/api_fastapi/common/parameters.py:641:22:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/55c7741b449bc81d923675b6220474fbfdda9d95/airflow/api_fastapi/common/parameters.py#L641'>airflow/api_fastapi/common/parameters.py:641:22:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/55c7741b449bc81d923675b6220474fbfdda9d95/airflow/auth/managers/base_auth_manager.py#L448'>airflow/auth/managers/base_auth_manager.py:448:36:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/55c7741b449bc81d923675b6220474fbfdda9d95/airflow/auth/managers/base_auth_manager.py#L448'>airflow/auth/managers/base_auth_manager.py:448:36:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/55c7741b449bc81d923675b6220474fbfdda9d95/airflow/cli/commands/remote_commands/task_command.py#L250'>airflow/cli/commands/remote_commands/task_command.py:250:71:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/55c7741b449bc81d923675b6220474fbfdda9d95/airflow/cli/commands/remote_commands/task_command.py#L250'>airflow/cli/commands/remote_commands/task_command.py:250:71:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/55c7741b449bc81d923675b6220474fbfdda9d95/airflow/cli/commands/remote_commands/task_command.py#L336'>airflow/cli/commands/remote_commands/task_command.py:336:46:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/55c7741b449bc81d923675b6220474fbfdda9d95/airflow/cli/commands/remote_commands/task_command.py#L336'>airflow/cli/commands/remote_commands/task_command.py:336:46:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/55c7741b449bc81d923675b6220474fbfdda9d95/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:23:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/55c7741b449bc81d923675b6220474fbfdda9d95/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:23:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/55c7741b449bc81d923675b6220474fbfdda9d95/airflow/decorators/__init__.pyi#L118'>airflow/decorators/__init__.pyi:118:25:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/55c7741b449bc81d923675b6220474fbfdda9d95/airflow/decorators/__init__.pyi#L118'>airflow/decorators/__init__.pyi:118:25:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/55c7741b449bc81d923675b6220474fbfdda9d95/airflow/decorators/__init__.pyi#L124'>airflow/decorators/__init__.pyi:124:21:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/airflow/blob/55c7741b449bc81d923675b6220474fbfdda9d95/airflow/decorators/__init__.pyi#L124'>airflow/decorators/__init__.pyi:124:21:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/airflow/blob/55c7741b449bc81d923675b6220474fbfdda9d95/airflow/decorators/__init__.pyi#L125'>airflow/decorators/__init__.pyi:125:26:</a> RUF036 [*] `None` not at the end of the type annotation.
... 227 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +10 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/config.py#L1126'>superset/config.py:1126:5:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/config.py#L1126'>superset/config.py:1126:5:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/config.py#L1728'>superset/config.py:1728:43:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/config.py#L1728'>superset/config.py:1728:43:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/config.py#L713'>superset/config.py:713:5:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/config.py#L713'>superset/config.py:713:5:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/db_engine_specs/gsheets.py#L166'>superset/db_engine_specs/gsheets.py:166:26:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/db_engine_specs/gsheets.py#L166'>superset/db_engine_specs/gsheets.py:166:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/jinja_context.py#L84'>superset/jinja_context.py:84:16:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/apache/superset/blob/e311bc1ca5f4bea9407012bd1fd278aa06a988f6/superset/jinja_context.py#L84'>superset/jinja_context.py:84:16:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
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
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
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
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
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
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/c70d44c45e7f96761d1d3808c7d7401eb65d5afe/pymilvus/client/abstract.py#L810'>pymilvus/client/abstract.py:810:40:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/milvus-io/pymilvus/blob/c70d44c45e7f96761d1d3808c7d7401eb65d5afe/pymilvus/client/abstract.py#L810'>pymilvus/client/abstract.py:810:40:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +114 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/_libs/json.pyi#L14'>pandas/_libs/json.pyi:14:22:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/_libs/json.pyi#L14'>pandas/_libs/json.pyi:14:22:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/_libs/tslibs/timedeltas.pyi#L147'>pandas/_libs/tslibs/timedeltas.pyi:147:36:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/_libs/tslibs/timedeltas.pyi#L147'>pandas/_libs/tslibs/timedeltas.pyi:147:36:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/_libs/tslibs/timestamps.pyi#L30'>pandas/_libs/tslibs/timestamps.pyi:30:41:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/_libs/tslibs/timestamps.pyi#L30'>pandas/_libs/tslibs/timestamps.pyi:30:41:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/core/arrays/datetimelike.py#L1040'>pandas/core/arrays/datetimelike.py:1040:45:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/core/arrays/datetimelike.py#L1040'>pandas/core/arrays/datetimelike.py:1040:45:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/core/dtypes/cast.py#L182'>pandas/core/dtypes/cast.py:182:38:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/core/dtypes/cast.py#L182'>pandas/core/dtypes/cast.py:182:38:</a> RUF036 `None` not at the end of the type annotation.
... 104 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/9358823214b4b995427de88228a15ba7a857c678/cibuildwheel/options.py#L753'>cibuildwheel/options.py:753:41:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/pypa/cibuildwheel/blob/9358823214b4b995427de88228a15ba7a857c678/cibuildwheel/options.py#L753'>cibuildwheel/options.py:753:41:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -0 violations, +276 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/_interpreters.pyi#L26'>stdlib/_interpreters.pyi:26:6:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/_interpreters.pyi#L26'>stdlib/_interpreters.pyi:26:6:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/_ssl.pyi#L79'>stdlib/_ssl.pyi:79:58:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/_ssl.pyi#L79'>stdlib/_ssl.pyi:79:58:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1743'>stdlib/ast.pyi:1743:26:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1743'>stdlib/ast.pyi:1743:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1753'>stdlib/ast.pyi:1753:26:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1753'>stdlib/ast.pyi:1753:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1763'>stdlib/ast.pyi:1763:26:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1763'>stdlib/ast.pyi:1763:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1773'>stdlib/ast.pyi:1773:26:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1773'>stdlib/ast.pyi:1773:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1782'>stdlib/ast.pyi:1782:26:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1782'>stdlib/ast.pyi:1782:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1791'>stdlib/ast.pyi:1791:26:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1791'>stdlib/ast.pyi:1791:26:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/ast.pyi#L1800'>stdlib/ast.pyi:1800:26:</a> RUF036 [*] `None` not at the end of the type annotation.
... 259 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/utils/env/env_manager.py#L117'>src/poetry/utils/env/env_manager.py:117:35:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/utils/env/env_manager.py#L117'>src/poetry/utils/env/env_manager.py:117:35:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/utils/env/env_manager.py#L142'>src/poetry/utils/env/env_manager.py:142:13:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/utils/env/env_manager.py#L142'>src/poetry/utils/env/env_manager.py:142:13:</a> RUF036 `None` not at the end of the type annotation.
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/utils/env/env_manager.py#L96'>src/poetry/utils/env/env_manager.py:96:44:</a> RUF036 [*] `None` not at the end of the type annotation.
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/utils/env/env_manager.py#L96'>src/poetry/utils/env/env_manager.py:96:44:</a> RUF036 `None` not at the end of the type annotation.
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF036 | 786 | 0 | 0 | 786 | 0 |

</p>
</details>
</details>



---

_Comment by @github-actions[bot] on 2025-01-05 09:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `ci` added by @MichaReiser on 2025-01-06 09:00_

---

_Comment by @MichaReiser on 2025-01-06 09:19_

Thanks. 

I patched the ecosystem check locally to get a sense of how the violations change between *not specifying `no_fix`* and *setting `no_fix`* This is the output

ℹ️ ecosystem check **detected linter changes**. (+20 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+20 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/argparse.pyi#L395'>stdlib/argparse.pyi:395:63:</a> PYI011 [*] Only simple default values allowed for typed arguments
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/argparse.pyi#L396'>stdlib/argparse.pyi:396:48:</a> PYI011 [*] Only simple default values allowed for typed arguments
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/argparse.pyi#L399'>stdlib/argparse.pyi:399:57:</a> PYI011 [*] Only simple default values allowed for typed arguments
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/argparse.pyi#L420'>stdlib/argparse.pyi:420:63:</a> PYI011 [*] Only simple default values allowed for typed arguments
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/argparse.pyi#L421'>stdlib/argparse.pyi:421:48:</a> PYI011 [*] Only simple default values allowed for typed arguments
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/argparse.pyi#L424'>stdlib/argparse.pyi:424:57:</a> PYI011 [*] Only simple default values allowed for typed arguments
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/builtins.pyi#L119'>stdlib/builtins.pyi:119:9:</a> PYI029 [*] Defining `__str__` in a stub is almost always redundant
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/builtins.pyi#L120'>stdlib/builtins.pyi:120:9:</a> PYI029 [*] Defining `__repr__` in a stub is almost always redundant
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/builtins.pyi#L1687'>stdlib/builtins.pyi:1687:1:</a> PYI026 [*] Use `typing_extensions.TypeAlias` for type alias, e.g., `_SupportsSomeKindOfPow: TypeAlias = _SupportsPow2[Any, Any] | _SupportsPow3NoneOnly[Any, Any] | _SupportsPow3[Any, Any, Any]`
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/builtins.pyi#L229'>stdlib/builtins.pyi:229:1:</a> PYI026 [*] Use `typing_extensions.TypeAlias` for type alias, e.g., `_LiteralInteger: TypeAlias = _PositiveInteger | _NegativeInteger | Literal[0]`
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/distutils/version.pyi#L26'>stdlib/distutils/version.pyi:26:9:</a> PYI029 [*] Defining `__str__` in a stub is almost always redundant
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stdlib/distutils/version.pyi#L35'>stdlib/distutils/version.pyi:35:9:</a> PYI029 [*] Defining `__str__` in a stub is almost always redundant
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stubs/jsonschema/jsonschema/exceptions.pyi#L32'>stubs/jsonschema/jsonschema/exceptions.pyi:32:34:</a> PYI011 [*] Only simple default values allowed for typed arguments
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stubs/jsonschema/jsonschema/exceptions.pyi#L36'>stubs/jsonschema/jsonschema/exceptions.pyi:36:40:</a> PYI011 [*] Only simple default values allowed for typed arguments
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stubs/jsonschema/jsonschema/exceptions.pyi#L37'>stubs/jsonschema/jsonschema/exceptions.pyi:37:33:</a> PYI011 [*] Only simple default values allowed for typed arguments
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stubs/jsonschema/jsonschema/exceptions.pyi#L38'>stubs/jsonschema/jsonschema/exceptions.pyi:38:52:</a> PYI011 [*] Only simple default values allowed for typed arguments
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stubs/jsonschema/jsonschema/exceptions.pyi#L41'>stubs/jsonschema/jsonschema/exceptions.pyi:41:45:</a> PYI011 [*] Only simple default values allowed for typed arguments
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stubs/olefile/olefile/olefile.pyi#L187'>stubs/olefile/olefile/olefile.pyi:187:37:</a> PYI011 [*] Only simple default values allowed for typed arguments
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stubs/olefile/olefile/olefile.pyi#L193'>stubs/olefile/olefile/olefile.pyi:193:82:</a> PYI011 [*] Only simple default values allowed for typed arguments
+ <a href='https://github.com/python/typeshed/blob/9f28171658b9ca6c32a7cb93fbb99fc92b17858b/stubs/tensorflow/tensorflow/autodiff.pyi#L16'>stubs/tensorflow/tensorflow/autodiff.pyi:16:81:</a> PYI011 [*] Only simple default values allowed for typed arguments
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI011 | 14 | 14 | 0 | 0 | 0 |
| PYI029 | 4 | 4 | 0 | 0 | 0 |
| PYI026 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>


```patch
Index: python/ruff-ecosystem/ruff_ecosystem/check.py
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/python/ruff-ecosystem/ruff_ecosystem/check.py b/python/ruff-ecosystem/ruff_ecosystem/check.py
--- a/python/ruff-ecosystem/ruff_ecosystem/check.py	(revision e5270e2ac2beada3ab141c66fa465dd6d115d8ae)
+++ b/python/ruff-ecosystem/ruff_ecosystem/check.py	(date 1736155061054)
@@ -515,6 +515,9 @@
                     options=options,
                 ),
             )
+            import dataclasses
+            options = dataclasses.replace(options, no_fix=True)
+
             comparison_task = tg.create_task(
                 ruff_check(
                     executable=ruff_comparison_executable.resolve(),
Index: python/ruff-ecosystem/ruff_ecosystem/projects.py
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/python/ruff-ecosystem/ruff_ecosystem/projects.py b/python/ruff-ecosystem/ruff_ecosystem/projects.py
--- a/python/ruff-ecosystem/ruff_ecosystem/projects.py	(revision e5270e2ac2beada3ab141c66fa465dd6d115d8ae)
+++ b/python/ruff-ecosystem/ruff_ecosystem/projects.py	(date 1736154397660)
@@ -198,6 +198,8 @@
     # Generating fixes is slow and verbose
     show_fixes: bool = False
 
+    no_fix: bool = False
+
     # Limit the number of reported lines per rule
     max_lines_per_rule: int | None = 50
 
@@ -222,6 +224,8 @@
             args.extend(["--exclude", self.exclude])
         if self.show_fixes:
             args.extend(["--show-fixes"])
+        if self.no_fix:
+            args.extend(["--no-fix"])
         return args
 
 

```

---

_Merged by @MichaReiser on 2025-01-06 09:21_

---

_Closed by @MichaReiser on 2025-01-06 09:21_

---
