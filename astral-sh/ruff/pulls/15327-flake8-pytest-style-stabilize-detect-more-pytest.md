```yaml
number: 15327
title: "[`flake8-pytest-style`] Stabilize \"Detect more `pytest.mark.parametrize` calls\" (`PT006`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
assignees: []
merged: true
base: ruff-0.9
head: PT006
created_at: 2025-01-07T19:39:26Z
updated_at: 2025-01-08T08:35:28Z
url: https://github.com/astral-sh/ruff/pull/15327
synced_at: 2026-01-10T20:34:00Z
```

# [`flake8-pytest-style`] Stabilize "Detect more `pytest.mark.parametrize` calls" (`PT006`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-07 19:39_

## Summary

Resolves #15324. Stabilizes the behavior changes introduced in #14515.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-07 19:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+75 -21 violations, +0 -0 fixes in 8 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+57 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/configuration.py#L1313'>airflow/configuration.py:1313:17:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/configuration.py#L1313'>airflow/configuration.py:1313:17:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/utils/log/action_logger.py#L22'>airflow/utils/log/action_logger.py:22:5:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/utils/log/action_logger.py#L22'>airflow/utils/log/action_logger.py:22:5:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/dev/breeze/src/airflow_breeze/utils/cdxgen.py#L589'>dev/breeze/src/airflow_breeze/utils/cdxgen.py:589:9:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/dev/breeze/src/airflow_breeze/utils/cdxgen.py#L589'>dev/breeze/src/airflow_breeze/utils/cdxgen.py:589:9:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/dev/breeze/src/airflow_breeze/utils/reproducible.py#L142'>dev/breeze/src/airflow_breeze/utils/reproducible.py:142:25:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/dev/breeze/src/airflow_breeze/utils/reproducible.py#L142'>dev/breeze/src/airflow_breeze/utils/reproducible.py:142:25:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/helm_tests/airflow_aux/test_annotations.py#L249'>helm_tests/airflow_aux/test_annotations.py:249:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/src/airflow/providers/elasticsearch/log/es_response.py#L101'>providers/src/airflow/providers/elasticsearch/log/es_response.py:101:14:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/src/airflow/providers/elasticsearch/log/es_response.py#L101'>providers/src/airflow/providers/elasticsearch/log/es_response.py:101:14:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/src/airflow/providers/google/cloud/operators/dataproc_metastore.py#L709'>providers/src/airflow/providers/google/cloud/operators/dataproc_metastore.py:709:16:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/src/airflow/providers/google/cloud/operators/dataproc_metastore.py#L709'>providers/src/airflow/providers/google/cloud/operators/dataproc_metastore.py:709:16:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
... 7 additional changes omitted for rule FURB188
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/tests/dbt/cloud/hooks/test_dbt.py#L157'>providers/tests/dbt/cloud/hooks/test_dbt.py:157:18:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/tests/dbt/cloud/hooks/test_dbt.py#L168'>providers/tests/dbt/cloud/hooks/test_dbt.py:168:18:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/tests/dbt/cloud/hooks/test_dbt.py#L178'>providers/tests/dbt/cloud/hooks/test_dbt.py:178:18:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/tests/dbt/cloud/hooks/test_dbt.py#L201'>providers/tests/dbt/cloud/hooks/test_dbt.py:201:18:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/tests/dbt/cloud/hooks/test_dbt.py#L216'>providers/tests/dbt/cloud/hooks/test_dbt.py:216:18:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/tests/dbt/cloud/hooks/test_dbt.py#L235'>providers/tests/dbt/cloud/hooks/test_dbt.py:235:18:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/tests/dbt/cloud/hooks/test_dbt.py#L254'>providers/tests/dbt/cloud/hooks/test_dbt.py:254:18:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/tests/dbt/cloud/hooks/test_dbt.py#L273'>providers/tests/dbt/cloud/hooks/test_dbt.py:273:18:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/tests/dbt/cloud/hooks/test_dbt.py#L294'>providers/tests/dbt/cloud/hooks/test_dbt.py:294:18:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/tests/dbt/cloud/hooks/test_dbt.py#L315'>providers/tests/dbt/cloud/hooks/test_dbt.py:315:18:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/tests/dbt/cloud/hooks/test_dbt.py#L334'>providers/tests/dbt/cloud/hooks/test_dbt.py:334:18:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
... 36 additional changes omitted for rule PT006
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/providers/tests/microsoft/azure/hooks/test_data_factory.py#L157'>providers/tests/microsoft/azure/hooks/test_data_factory.py:157:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
... 41 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L511'>src/bokeh/util/compiler.py:511:9:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L511'>src/bokeh/util/compiler.py:511:9:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/file_server.py#L63'>tests/support/plugins/file_server.py:63:9:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/file_server.py#L63'>tests/support/plugins/file_server.py:63:9:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/language_models/llms.py#L352'>libs/core/langchain_core/language_models/llms.py:352:9:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
- <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/language_models/llms.py#L352'>libs/core/langchain_core/language_models/llms.py:352:9:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/snakemake/single_task_snakemake.py#L365'>src/latch_cli/snakemake/single_task_snakemake.py:365:12:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/snakemake/single_task_snakemake.py#L365'>src/latch_cli/snakemake/single_task_snakemake.py:365:12:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/utils/__init__.py#L106'>src/latch_cli/utils/__init__.py:106:5:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/utils/__init__.py#L106'>src/latch_cli/utils/__init__.py:106:5:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/setuptools/blob/4e82e8b75c8bd8cba1232a107dc171b4fd2c588c/pkg_resources/tests/test_working_set.py#L107'>pkg_resources/tests/test_working_set.py:107:9:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/pypa/setuptools/blob/4e82e8b75c8bd8cba1232a107dc171b4fd2c588c/setuptools/tests/test_egg_info.py#L303'>setuptools/tests/test_egg_info.py:303:17:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/5d877d54d09a8ae016de1f46f43cc2528ff08a89/reflex/components/component.py#L664'>reflex/components/component.py:664:17:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
- <a href='https://github.com/reflex-dev/reflex/blob/5d877d54d09a8ae016de1f46f43cc2528ff08a89/reflex/components/component.py#L664'>reflex/components/component.py:664:17:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pdm-project/pdm">pdm-project/pdm</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pdm-project/pdm/blob/f37fb16c2459807e0b392dc3306c373e5f68cc4f/src/pdm/cli/commands/cache.py#L37'>src/pdm/cli/commands/cache.py:37:47:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Iterable`
+ <a href='https://github.com/pdm-project/pdm/blob/f37fb16c2459807e0b392dc3306c373e5f68cc4f/src/pdm/cli/commands/cache.py#L97'>src/pdm/cli/commands/cache.py:97:20:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Iterable`
+ <a href='https://github.com/pdm-project/pdm/blob/f37fb16c2459807e0b392dc3306c373e5f68cc4f/src/pdm/cli/commands/config.py#L102'>src/pdm/cli/commands/config.py:102:36:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Mapping`
+ <a href='https://github.com/pdm-project/pdm/blob/f37fb16c2459807e0b392dc3306c373e5f68cc4f/src/pdm/cli/commands/config.py#L102'>src/pdm/cli/commands/config.py:102:67:</a> FA100 Add `from __future__ import annotations` to simplify `typing.Mapping`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/ffa796eb9a00954705fd4c59dada3b94788be6e8/astropy/io/fits/hdu/compressed/_tiled_compression.py#L259'>astropy/io/fits/hdu/compressed/_tiled_compression.py:259:5:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
- <a href='https://github.com/astropy/astropy/blob/ffa796eb9a00954705fd4c59dada3b94788be6e8/astropy/io/fits/hdu/compressed/_tiled_compression.py#L259'>astropy/io/fits/hdu/compressed/_tiled_compression.py:259:5:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
+ <a href='https://github.com/astropy/astropy/blob/ffa796eb9a00954705fd4c59dada3b94788be6e8/astropy/io/fits/header.py#L1646'>astropy/io/fits/header.py:1646:9:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
- <a href='https://github.com/astropy/astropy/blob/ffa796eb9a00954705fd4c59dada3b94788be6e8/astropy/io/fits/header.py#L1646'>astropy/io/fits/header.py:1646:9:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
+ <a href='https://github.com/astropy/astropy/blob/ffa796eb9a00954705fd4c59dada3b94788be6e8/astropy/io/votable/converters.py#L1056'>astropy/io/votable/converters.py:1056:13:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
- <a href='https://github.com/astropy/astropy/blob/ffa796eb9a00954705fd4c59dada3b94788be6e8/astropy/io/votable/converters.py#L1056'>astropy/io/votable/converters.py:1056:13:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
+ <a href='https://github.com/astropy/astropy/blob/ffa796eb9a00954705fd4c59dada3b94788be6e8/astropy/io/votable/converters.py#L1058'>astropy/io/votable/converters.py:1058:13:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
- <a href='https://github.com/astropy/astropy/blob/ffa796eb9a00954705fd4c59dada3b94788be6e8/astropy/io/votable/converters.py#L1058'>astropy/io/votable/converters.py:1058:13:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
+ <a href='https://github.com/astropy/astropy/blob/ffa796eb9a00954705fd4c59dada3b94788be6e8/astropy/io/votable/converters.py#L326'>astropy/io/votable/converters.py:326:13:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
- <a href='https://github.com/astropy/astropy/blob/ffa796eb9a00954705fd4c59dada3b94788be6e8/astropy/io/votable/converters.py#L326'>astropy/io/votable/converters.py:326:13:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT006 | 49 | 49 | 0 | 0 | 0 |
| FURB188 | 42 | 21 | 21 | 0 | 0 |
| FA100 | 4 | 4 | 0 | 0 | 0 |
| PT007 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2553 -1620 violations, +4 -0 fixes in 31 projects; 24 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/training/test_interactive.py#L663'>tests/core/training/test_interactive.py:663:58:</a> RUF025 [*] Unnecessary empty iterable within a deque call
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/training/test_interactive.py#L663'>tests/core/training/test_interactive.py:663:58:</a> RUF037 [*] Unnecessary empty iterable within a deque call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+409 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L104'>aiven/client/argx.py:104:38:</a> UP006 Use `collections.abc.Iterable` instead of `Iterable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L155'>aiven/client/argx.py:155:54:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L171'>aiven/client/argx.py:171:45:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L174'>aiven/client/argx.py:174:49:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L241'>aiven/client/argx.py:241:29:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L278'>aiven/client/argx.py:278:34:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L278'>aiven/client/argx.py:278:44:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L290'>aiven/client/argx.py:290:32:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L300'>aiven/client/argx.py:300:29:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/aiven/aiven-client/blob/e73ee16e9c9934a04cc19d476434b7db96f7e09a/aiven/client/argx.py#L303'>aiven/client/argx.py:303:34:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
... 399 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8da7619b617b1eb8b4aabdb1aeb18655065cf6a0/tests/particles/test_decorators.py#L437'>tests/particles/test_decorators.py:437:5:</a> B903 Class could be dataclass or namedtuple
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8da7619b617b1eb8b4aabdb1aeb18655065cf6a0/tests/particles/test_decorators.py#L456'>tests/particles/test_decorators.py:456:5:</a> B903 Class could be dataclass or namedtuple
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8da7619b617b1eb8b4aabdb1aeb18655065cf6a0/tests/particles/test_decorators.py#L494'>tests/particles/test_decorators.py:494:5:</a> B903 Class could be dataclass or namedtuple
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8da7619b617b1eb8b4aabdb1aeb18655065cf6a0/tests/particles/test_decorators_annotations.py#L16'>tests/particles/test_decorators_annotations.py:16:1:</a> B903 Class could be dataclass or namedtuple
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+496 -20 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/api/auth/backend/deny_all.py#L34'>airflow/api/auth/backend/deny_all.py:34:24:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/api_connexion/parameters.py#L87'>airflow/api_connexion/parameters.py:87:24:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/api_connexion/parameters.py#L90'>airflow/api_connexion/parameters.py:90:52:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/api_connexion/parameters.py#L90'>airflow/api_connexion/parameters.py:90:78:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/api_connexion/security.py#L114'>airflow/api_connexion/security.py:114:6:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/api_connexion/security.py#L161'>airflow/api_connexion/security.py:161:54:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 478 additional changes omitted for rule UP006
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/configuration.py#L1313'>airflow/configuration.py:1313:17:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/configuration.py#L1313'>airflow/configuration.py:1313:17:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/utils/log/action_logger.py#L22'>airflow/utils/log/action_logger.py:22:5:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/301017de2d76ac9aeb55f966ee580481dd88e525/airflow/utils/log/action_logger.py#L22'>airflow/utils/log/action_logger.py:22:5:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
... 506 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+246 -80 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/scripts/check-env.py#L37'>scripts/check-env.py:37:40:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/advanced_data_type/types.py#L58'>superset/advanced_data_type/types.py:58:21:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/advanced_data_type/types.py#L59'>superset/advanced_data_type/types.py:59:23:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/async_events/async_query_manager.py#L106'>superset/async_events/async_query_manager.py:106:22:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/async_events/async_query_manager.py#L106'>superset/async_events/async_query_manager.py:106:22:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/async_events/async_query_manager.py#L108'>superset/async_events/async_query_manager.py:108:29:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/async_events/async_query_manager.py#L108'>superset/async_events/async_query_manager.py:108:29:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/async_events/async_query_manager.py#L109'>superset/async_events/async_query_manager.py:109:38:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/async_events/async_query_manager.py#L109'>superset/async_events/async_query_manager.py:109:38:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/apache/superset/blob/d5a48158368a2cbb0e97d9b8de194df5d9fc9bc6/superset/async_events/async_query_manager.py#L112'>superset/async_events/async_query_manager.py:112:34:</a> UP007 Use `X | Y` for type annotations
... 316 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+252 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/action.py#L27'>release/action.py:27:31:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/action.py#L27'>release/action.py:27:52:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/action.py#L35'>release/action.py:35:47:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/build.py#L144'>release/build.py:144:22:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L37'>release/credentials.py:37:22:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/credentials.py#L40'>release/credentials.py:40:38:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 245 additional changes omitted for rule UP006
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L511'>src/bokeh/util/compiler.py:511:9:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L511'>src/bokeh/util/compiler.py:511:9:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/file_server.py#L63'>tests/support/plugins/file_server.py:63:9:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/file_server.py#L63'>tests/support/plugins/file_server.py:63:9:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
... 244 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+3 -102 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/backends/bigquery/__init__.py#L847'>ibis/backends/bigquery/__init__.py:847:48:</a> UP045 [*] Use `X | None` for type annotations
- <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/graph.py#L171'>ibis/common/graph.py:171:46:</a> UP045 [*] Use `X | None` for type annotations
- <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/graph.py#L203'>ibis/common/graph.py:203:50:</a> UP045 [*] Use `X | None` for type annotations
- <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/graph.py#L275'>ibis/common/graph.py:275:41:</a> UP045 [*] Use `X | None` for type annotations
- <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/graph.py#L310'>ibis/common/graph.py:310:47:</a> UP045 [*] Use `X | None` for type annotations
- <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/graph.py#L359'>ibis/common/graph.py:359:47:</a> UP045 [*] Use `X | None` for type annotations
... 97 additional changes omitted for rule UP045
+ <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/tests/test_patterns.py#L1122'>ibis/common/tests/test_patterns.py:1122:13:</a> UP006 [*] Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/tests/test_patterns.py#L1125'>ibis/common/tests/test_patterns.py:1125:10:</a> UP006 [*] Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/common/tests/test_patterns.py#L731'>ibis/common/tests/test_patterns.py:731:31:</a> UP006 [*] Use `collections.abc.Callable` instead of `Callable` for type annotation
... 96 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+300 -1150 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/_api/beta_decorator.py#L128'>libs/core/langchain_core/_api/beta_decorator.py:128:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/_api/beta_decorator.py#L186'>libs/core/langchain_core/_api/beta_decorator.py:186:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/_api/beta_decorator.py#L201'>libs/core/langchain_core/_api/beta_decorator.py:201:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/_api/beta_decorator.py#L30'>libs/core/langchain_core/_api/beta_decorator.py:30:30:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/_api/beta_decorator.py#L39'>libs/core/langchain_core/_api/beta_decorator.py:39:6:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/_api/deprecation.py#L202'>libs/core/langchain_core/_api/deprecation.py:202:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 294 additional changes omitted for rule UP006
- <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/caches.py#L149'>libs/core/langchain_core/caches.py:149:36:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/caches.py#L167'>libs/core/langchain_core/caches.py:167:55:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/caches.py#L200'>libs/core/langchain_core/caches.py:200:62:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/caches.py#L52'>libs/core/langchain_core/caches.py:52:55:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/caches.py#L97'>libs/core/langchain_core/caches.py:97:62:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/callbacks/base.py#L104'>libs/core/langchain_core/callbacks/base.py:104:24:</a> UP045 Use `X | None` for type annotations
... 1143 additional changes omitted for rule UP045
+ <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/language_models/llms.py#L352'>libs/core/langchain_core/language_models/llms.py:352:9:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
- <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/langchain_core/language_models/llms.py#L352'>libs/core/langchain_core/language_models/llms.py:352:9:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
- <a href='https://github.com/langchain-ai/langchain/blob/b1dafaef9b2c32151acb42367012e0a7d35067e3/libs/core/tests/unit_tests/tracers/test_langchain.py#L103'>libs/core/tests/unit_tests/tracers/test_langchain.py:103:5:</a> B903 Class could be dataclass or namedtuple
... 1435 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+15 -15 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L188'>src/latch/registry/record.py:188:57:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L188'>src/latch/registry/record.py:188:57:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L190'>src/latch/registry/record.py:190:64:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L190'>src/latch/registry/record.py:190:64:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L215'>src/latch/registry/record.py:215:53:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L215'>src/latch/registry/record.py:215:53:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L217'>src/latch/registry/record.py:217:60:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L217'>src/latch/registry/record.py:217:60:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L250'>src/latch/registry/record.py:250:10:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/registry/record.py#L250'>src/latch/registry/record.py:250:10:</a> UP045 Use `X | None` for type annotations
... 20 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (12 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP006 | 2431 | 2431 | 0 | 0 | 0 |
| UP045 | 1552 | 0 | 1552 | 0 | 0 |
| UP007 | 92 | 92 | 0 | 0 | 0 |
| FURB188 | 42 | 21 | 21 | 0 | 0 |
| B903 | 40 | 0 | 40 | 0 | 0 |
| FA100 | 4 | 4 | 0 | 0 | 0 |
| FURB171 | 4 | 0 | 0 | 4 | 0 |
| RUF025 | 3 | 3 | 0 | 0 | 0 |
| RUF037 | 3 | 0 | 3 | 0 | 0 |
| PLR6301 | 2 | 1 | 1 | 0 | 0 |
| CPY001 | 2 | 1 | 1 | 0 | 0 |
| RUF100 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Comment by @dylwil3 on 2025-01-07 20:55_

I think this stabilization makes sense to me. But this PR may need to target the branch in #15238 , assuming @MichaReiser and @AlexWaygood agree

---

_Assigned to @MichaReiser by @MichaReiser on 2025-01-08 07:22_

---

_Label `breaking` added by @MichaReiser on 2025-01-08 07:22_

---

_Review requested from @carljm by @MichaReiser on 2025-01-08 07:22_

---

_Review requested from @MichaReiser by @MichaReiser on 2025-01-08 07:22_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-08 07:22_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-08 07:22_

---

_Renamed from "[`flake8-pytest-style`] Detect keyword arguments in non-preview mode (`PT006`)" to "[`flake8-pytest-style`] Stabilize "Detect more `pytest.mark.parametrize` calls (`PT006`)" by @MichaReiser on 2025-01-08 07:50_

---

_Label `breaking` removed by @MichaReiser on 2025-01-08 07:50_

---

_Label `rule` added by @MichaReiser on 2025-01-08 07:50_

---

_Review request for @carljm removed by @AlexWaygood on 2025-01-08 07:53_

---

_Review request for @sharkdp removed by @AlexWaygood on 2025-01-08 07:53_

---

_Comment by @codspeed-hq[bot] on 2025-01-08 07:54_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/InSyncWithFoo%3APT006)

### Merging #15327 will **improve performances by 4.62%**

<sub>Comparing <code>InSyncWithFoo:PT006</code> (af82bca) with <code>ruff-0.9</code> (412084e)</sub>



### Summary

`⚡ 1` improvements  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `ruff-0.9` | `InSyncWithFoo:PT006` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_check_file[cold] `` | 88.1 ms | 84.2 ms | +4.62% |


---

_Renamed from "[`flake8-pytest-style`] Stabilize "Detect more `pytest.mark.parametrize` calls (`PT006`)" to "[`flake8-pytest-style`] Stabilize "Detect more `pytest.mark.parametrize` calls" (`PT006`)" by @InSyncWithFoo on 2025-01-08 07:55_

---

_@MichaReiser approved on 2025-01-08 08:27_

Thanks

---

_Merged by @MichaReiser on 2025-01-08 08:27_

---

_Closed by @MichaReiser on 2025-01-08 08:27_

---

_Branch deleted on 2025-01-08 08:35_

---
