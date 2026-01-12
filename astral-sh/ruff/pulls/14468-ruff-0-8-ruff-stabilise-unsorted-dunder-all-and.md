```yaml
number: 14468
title: "[ruff-0.8] [`ruff`] Stabilise `unsorted-dunder-all` and `unsorted-dunder-slots`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: ruff-0.8
head: sorting-stabilise
created_at: 2024-11-19T18:31:07Z
updated_at: 2024-11-20T07:34:22Z
url: https://github.com/astral-sh/ruff/pull/14468
synced_at: 2026-01-12T15:55:47Z
```

# [ruff-0.8] [`ruff`] Stabilise `unsorted-dunder-all` and `unsorted-dunder-slots`

---

_@AlexWaygood_

## Summary

These rules were implemented in January, have been very stable, and have no open issues about them. They were highly requested by the community prior to being implemented. Let's stabilise them!

## Test Plan

Ecosystem check on this PR.


---

_Label `rule` added by @AlexWaygood on 2024-11-19 18:31_

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-11-19 18:31_

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-11-19 18:31_

---

_Comment by @codspeed-hq[bot] on 2024-11-19 18:39_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/sorting-stabilise)

### Merging #14468 will **degrade performances by 5.04%**

<sub>Comparing <code>sorting-stabilise</code> (9d1557c) with <code>ruff-0.8</code> (f0cdc24)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/sorting-stabilise)._

### Benchmarks breakdown

|     | Benchmark | `ruff-0.8` | `sorting-stabilise` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 889.1 µs | 936.2 µs | -5.04% |


---

_Comment by @github-actions[bot] on 2024-11-19 18:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+364 -0 violations, +0 -0 fixes in 22 projects; 32 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+168 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/abc.py#L205'>disnake/abc.py:205:17:</a> RUF023 [*] `_Overwrites.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/abc.py#L49'>disnake/abc.py:49:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/activity.py#L13'>disnake/activity.py:13:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/activity.py#L289'>disnake/activity.py:289:17:</a> RUF023 [*] `Activity.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/activity.py#L530'>disnake/activity.py:530:17:</a> RUF023 [*] `Streaming.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/activity.py#L619'>disnake/activity.py:619:17:</a> RUF023 [*] `Spotify.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/activity.py#L803'>disnake/activity.py:803:17:</a> RUF023 [*] `CustomActivity.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/app_commands.py#L1017'>disnake/app_commands.py:1017:17:</a> RUF023 [*] `ApplicationCommandPermissions.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/app_commands.py#L1075'>disnake/app_commands.py:1075:17:</a> RUF023 [*] `GuildApplicationCommandPermissions.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/app_commands.py#L241'>disnake/app_commands.py:241:17:</a> RUF023 [*] `Option.__slots__` is not sorted
... 111 additional changes omitted for rule RUF023
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/app_commands.py#L49'>disnake/app_commands.py:49:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/appinfo.py#L23'>disnake/appinfo.py:23:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/audit_logs.py#L34'>disnake/audit_logs.py:34:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/automod.py#L51'>disnake/automod.py:51:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/channel.py#L53'>disnake/channel.py:53:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/client.py#L100'>disnake/client.py:100:11:</a> RUF022 [*] `__all__` is not sorted
... 42 additional changes omitted for rule RUF022
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/webhook/async_.py#L645'>disnake/webhook/async_.py:645:38:</a> B039 Do not use mutable data structures for `ContextVar` defaults
... 151 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/stubs/aio_pika/__init__.pyi#L23'>stubs/aio_pika/__init__.pyi:23:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/stubs/sanic.pyi#L11'>stubs/sanic.pyi:11:11:</a> RUF022 [*] `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b8f11cb46078eacb8cf33a5758fbe8f74b178ff/src/plasmapy/particles/_special_particles.py#L260'>src/plasmapy/particles/_special_particles.py:260:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b8f11cb46078eacb8cf33a5758fbe8f74b178ff/src/plasmapy/particles/decorators.py#L677'>src/plasmapy/particles/decorators.py:677:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b8f11cb46078eacb8cf33a5758fbe8f74b178ff/src/plasmapy/particles/ionization_state_collection.py#L479'>src/plasmapy/particles/ionization_state_collection.py:479:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b8f11cb46078eacb8cf33a5758fbe8f74b178ff/src/plasmapy/plasma/grids.py#L769'>src/plasmapy/plasma/grids.py:769:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+25 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/81a910db9af72db0c7d12c33bb186cb8b117322e/airflow/__init__.py#L63'>airflow/__init__.py:63:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/81a910db9af72db0c7d12c33bb186cb8b117322e/airflow/decorators/__init__.py#L38'>airflow/decorators/__init__.py:38:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/81a910db9af72db0c7d12c33bb186cb8b117322e/airflow/decorators/__init__.pyi#L46'>airflow/decorators/__init__.pyi:46:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/81a910db9af72db0c7d12c33bb186cb8b117322e/airflow/models/__init__.py#L23'>airflow/models/__init__.py:23:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/81a910db9af72db0c7d12c33bb186cb8b117322e/airflow/providers_manager.py#L104'>airflow/providers_manager.py:104:17:</a> RUF023 [*] `LazyDictWithCache.__slots__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/81a910db9af72db0c7d12c33bb186cb8b117322e/airflow/secrets/__init__.py#L30'>airflow/secrets/__init__.py:30:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/81a910db9af72db0c7d12c33bb186cb8b117322e/airflow/typing_compat.py#L22'>airflow/typing_compat.py:22:11:</a> RUF022 [*] `__all__` is not sorted
... 9 additional changes omitted for rule RUF022
+ <a href='https://github.com/apache/airflow/blob/81a910db9af72db0c7d12c33bb186cb8b117322e/airflow/utils/hashlib_wrapper.py#L27'>airflow/utils/hashlib_wrapper.py:27:9:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/apache/airflow/blob/81a910db9af72db0c7d12c33bb186cb8b117322e/airflow/www/views.py#L2228'>airflow/www/views.py:2228:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/81a910db9af72db0c7d12c33bb186cb8b117322e/docs/conf.py#L468'>docs/conf.py:468:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
... 15 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/RELEASING/changelog.py#L227'>RELEASING/changelog.py:227:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/superset/examples/data_loading.py#L36'>superset/examples/data_loading.py:36:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/superset/jinja_context.py#L483'>superset/jinja_context.py:483:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/superset/utils/pandas_postprocessing/__init__.py#L43'>superset/utils/pandas_postprocessing/__init__.py:43:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/superset/views/__init__.py#L30'>superset/views/__init__.py:30:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/tests/integration_tests/reports/api_tests.py#L303'>tests/integration_tests/reports/api_tests.py:303:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/219c6afeeff5384ae59e9fc2d3748748022a1f48/tests/integration/pipeline/test_bootstrap_command.py#L229'>tests/integration/pipeline/test_bootstrap_command.py:229:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+55 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/checks.py#L24'>release/checks.py:24:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L201'>src/bokeh/application/handlers/code.py:201:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/__init__.py#L46'>src/bokeh/client/__init__.py:46:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/client/states.py#L40'>src/bokeh/client/states.py:40:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/__init__.py#L37'>src/bokeh/colors/__init__.py:37:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/enums.py#L91'>src/bokeh/core/enums.py:91:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L81'>src/bokeh/core/has_props.py:81:11:</a> RUF022 [*] `__all__` is not sorted
... 48 additional changes omitted for rule RUF022
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/wrappers.py#L470'>src/bokeh/core/property/wrappers.py:470:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
... 47 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+39 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/e43d986230ab5cb769bc0c3bd5272743844a332c/ibis/backends/impala/udf.py#L24'>ibis/backends/impala/udf.py:24:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/e43d986230ab5cb769bc0c3bd5272743844a332c/ibis/backends/sql/compilers/__init__.py#L3'>ibis/backends/sql/compilers/__init__.py:3:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/e43d986230ab5cb769bc0c3bd5272743844a332c/ibis/backends/sql/compilers/base.py#L178'>ibis/backends/sql/compilers/base.py:178:17:</a> RUF023 [*] `FuncGen.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/e43d986230ab5cb769bc0c3bd5272743844a332c/ibis/backends/sql/compilers/base.py#L83'>ibis/backends/sql/compilers/base.py:83:21:</a> RUF023 [*] `_Accessor.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/e43d986230ab5cb769bc0c3bd5272743844a332c/ibis/common/annotations.py#L105'>ibis/common/annotations.py:105:17:</a> RUF023 [*] `Annotation.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/e43d986230ab5cb769bc0c3bd5272743844a332c/ibis/common/annotations.py#L207'>ibis/common/annotations.py:207:17:</a> RUF023 [*] `Argument.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/e43d986230ab5cb769bc0c3bd5272743844a332c/ibis/common/annotations.py#L42'>ibis/common/annotations.py:42:17:</a> RUF023 [*] `AttributeValidationError.__slots__` is not sorted
+ <a href='https://github.com/ibis-project/ibis/blob/e43d986230ab5cb769bc0c3bd5272743844a332c/ibis/common/annotations.py#L54'>ibis/common/annotations.py:54:17:</a> RUF023 [*] `ReturnValidationError.__slots__` is not sorted
... 32 additional changes omitted for rule RUF023
... 31 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/197b885911764fc71adccdc21ac71d85d7eac776/libs/core/langchain_core/runnables/config.py#L114'>libs/core/langchain_core/runnables/config.py:114:38:</a> B039 Do not use mutable data structures for `ContextVar` defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/functions/operators.py#L183'>src/latch/functions/operators.py:183:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/functions/operators.py#L37'>src/latch/functions/operators.py:37:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/functions/operators.py#L48'>src/latch/functions/operators.py:48:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/functions/operators.py#L59'>src/latch/functions/operators.py:59:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/functions/operators.py#L68'>src/latch/functions/operators.py:68:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/functions/operators.py#L73'>src/latch/functions/operators.py:73:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/lnurl.py#L59'>lnbits/lnurl.py:59:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/requestvars.py#L5'>lnbits/requestvars.py:5:31:</a> B039 Do not use mutable data structures for `ContextVar` defaults
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/wallets/__init__.py#L56'>lnbits/wallets/__init__.py:56:11:</a> RUF022 [*] `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/24cba21ea49f75bbcfd3e85a261412329345e8ea/pymilvus/__init__.py#L74'>pymilvus/__init__.py:74:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/milvus-io/pymilvus/blob/24cba21ea49f75bbcfd3e85a261412329345e8ea/pymilvus/bulk_writer/buffer.py#L96'>pymilvus/bulk_writer/buffer.py:96:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/_typing.py#L276'>pandas/_typing.py:276:20:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/_typing.py#L291'>pandas/_typing.py:291:20:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/_typing.py#L297'>pandas/_typing.py:297:21:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/io/stata.py#L2209'>pandas/io/stata.py:2209:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/build/blob/eeea375c410e0f874f1bd09172e468ce6e68f365/src/build/__init__.py#L23'>src/build/__init__.py:23:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/pypa/build/blob/eeea375c410e0f874f1bd09172e468ce6e68f365/src/build/_ctx.py#L91'>src/build/_ctx.py:91:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/pypa/build/blob/eeea375c410e0f874f1bd09172e468ce6e68f365/src/build/env.py#L369'>src/build/env.py:369:11:</a> RUF022 [*] `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/8f21eb17be321024854494f69d8b45610e791c65/cibuildwheel/_compat/typing.py#L10'>cibuildwheel/_compat/typing.py:10:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/pypa/cibuildwheel/blob/8f21eb17be321024854494f69d8b45610e791c65/cibuildwheel/typing.py#L8'>cibuildwheel/typing.py:8:11:</a> RUF022 [*] `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/9e20ec612893541d253674e688269e2495be0a12/skbuild/__init__.py#L16'>skbuild/__init__.py:16:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build/blob/9e20ec612893541d253674e688269e2495be0a12/skbuild/_compat/typing.py#L10'>skbuild/_compat/typing.py:10:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build/blob/9e20ec612893541d253674e688269e2495be0a12/tests/samples/issue-707-nested-packages/hello_nested/__init__.py#L6'>tests/samples/issue-707-nested-packages/hello_nested/__init__.py:6:11:</a> RUF022 [*] `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/6862c651d8a235af8c5d514ea5c12b2ea1f6e4fc/src/scikit_build_core/_compat/importlib/metadata.py#L24'>src/scikit_build_core/_compat/importlib/metadata.py:24:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/6862c651d8a235af8c5d514ea5c12b2ea1f6e4fc/src/scikit_build_core/_logging.py#L25'>src/scikit_build_core/_logging.py:25:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/6862c651d8a235af8c5d514ea5c12b2ea1f6e4fc/src/scikit_build_core/ast/ast.py#L14'>src/scikit_build_core/ast/ast.py:14:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/6862c651d8a235af8c5d514ea5c12b2ea1f6e4fc/src/scikit_build_core/ast/ast.py#L23'>src/scikit_build_core/ast/ast.py:23:17:</a> RUF023 [*] `Node.__slots__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/6862c651d8a235af8c5d514ea5c12b2ea1f6e4fc/src/scikit_build_core/ast/tokenizer.py#L48'>src/scikit_build_core/ast/tokenizer.py:48:17:</a> RUF023 [*] `Token.__slots__` is not sorted
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/6862c651d8a235af8c5d514ea5c12b2ea1f6e4fc/src/scikit_build_core/build/__init__.py#L112'>src/scikit_build_core/build/__init__.py:112:16:</a> RUF022 [*] `__all__` is not sorted
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (5 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF023 | 167 | 167 | 0 | 0 | 0 |
| RUF022 | 145 | 145 | 0 | 0 | 0 |
| PLC0206 | 43 | 43 | 0 | 0 | 0 |
| PYI063 | 5 | 5 | 0 | 0 | 0 |
| B039 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-11-19 18:57_

lots of ecosystem hits, but I can't see any false positives from a quick skim; I think the rules are working as intended.

---

_@MichaReiser approved on 2024-11-20 07:29_

---

_Merged by @AlexWaygood on 2024-11-20 07:34_

---

_Closed by @AlexWaygood on 2024-11-20 07:34_

---

_Branch deleted on 2024-11-20 07:34_

---
