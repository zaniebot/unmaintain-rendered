```yaml
number: 15329
title: "[ruff-0.9] Stabilise `slice-to-remove-prefix-or-suffix` (`FURB188`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: ruff-0.9
head: alex/stabilise-furb188
created_at: 2025-01-07T19:51:43Z
updated_at: 2025-01-07T20:03:08Z
url: https://github.com/astral-sh/ruff/pull/15329
synced_at: 2026-01-10T20:34:00Z
```

# [ruff-0.9] Stabilise `slice-to-remove-prefix-or-suffix` (`FURB188`)

---

_Pull request opened by @AlexWaygood on 2025-01-07 19:51_

## Summary

Stabilise [`slice-to-remove-prefix-or-suffix`](https://docs.astral.sh/ruff/rules/slice-to-remove-prefix-or-suffix/) (`FURB188`) for the Ruff 0.9 release.

This is a stylistic rule, but I think it's a pretty uncontroversial one. There are no open issues or PRs regarding it and it's been in preview for a while now.

## Test Plan

Ecosystem check on this PR


---

_Label `rule` added by @AlexWaygood on 2025-01-07 19:51_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-07 19:51_

---

_Review requested from @dylwil3 by @AlexWaygood on 2025-01-07 19:51_

---

_@dylwil3 approved on 2025-01-07 19:57_

---

_Comment by @github-actions[bot] on 2025-01-07 19:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+25 -0 violations, +0 -0 fixes in 8 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/configuration.py#L1313'>airflow/configuration.py:1313:17:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/utils/log/action_logger.py#L22'>airflow/utils/log/action_logger.py:22:5:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/dev/breeze/src/airflow_breeze/utils/cdxgen.py#L589'>dev/breeze/src/airflow_breeze/utils/cdxgen.py:589:9:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/dev/breeze/src/airflow_breeze/utils/reproducible.py#L142'>dev/breeze/src/airflow_breeze/utils/reproducible.py:142:25:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/providers/src/airflow/providers/elasticsearch/log/es_response.py#L101'>providers/src/airflow/providers/elasticsearch/log/es_response.py:101:14:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/providers/src/airflow/providers/google/cloud/operators/dataproc_metastore.py#L709'>providers/src/airflow/providers/google/cloud/operators/dataproc_metastore.py:709:16:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/providers/src/airflow/providers/opensearch/log/os_response.py#L101'>providers/src/airflow/providers/opensearch/log/os_response.py:101:14:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/providers/tests/elasticsearch/log/test_es_response.py#L186'>providers/tests/elasticsearch/log/test_es_response.py:186:13:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/providers/tests/opensearch/log/test_os_response.py#L107'>providers/tests/opensearch/log/test_os_response.py:107:13:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L511'>src/bokeh/util/compiler.py:511:9:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/file_server.py#L63'>tests/support/plugins/file_server.py:63:9:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/langchain_core/language_models/llms.py#L352'>libs/core/langchain_core/language_models/llms.py:352:9:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/snakemake/single_task_snakemake.py#L365'>src/latch_cli/snakemake/single_task_snakemake.py:365:12:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/utils/__init__.py#L106'>src/latch_cli/utils/__init__.py:106:5:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/08d9fbf9bc5846c4f60737abcd8d8bf1d134ad96/reflex/components/component.py#L664'>reflex/components/component.py:664:17:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
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
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/io/fits/hdu/compressed/_tiled_compression.py#L259'>astropy/io/fits/hdu/compressed/_tiled_compression.py:259:5:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/io/fits/header.py#L1646'>astropy/io/fits/header.py:1646:9:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/io/votable/converters.py#L1056'>astropy/io/votable/converters.py:1056:13:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/io/votable/converters.py#L1058'>astropy/io/votable/converters.py:1058:13:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/io/votable/converters.py#L326'>astropy/io/votable/converters.py:326:13:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/table/meta.py#L211'>astropy/table/meta.py:211:5:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB188 | 21 | 21 | 0 | 0 | 0 |
| FA100 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2532 -1599 violations, +4 -0 fixes in 30 projects; 25 projects unchanged)

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
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+487 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/api/auth/backend/deny_all.py#L34'>airflow/api/auth/backend/deny_all.py:34:24:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/api_connexion/parameters.py#L87'>airflow/api_connexion/parameters.py:87:24:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/api_connexion/parameters.py#L90'>airflow/api_connexion/parameters.py:90:52:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/api_connexion/parameters.py#L90'>airflow/api_connexion/parameters.py:90:78:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/api_connexion/security.py#L114'>airflow/api_connexion/security.py:114:6:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/api_connexion/security.py#L161'>airflow/api_connexion/security.py:161:54:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 478 additional changes omitted for rule UP006
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/utils/log/logging_mixin.py#L175'>airflow/utils/log/logging_mixin.py:175:9:</a> PLR6301 Method `writable` could be a function, class method, or static method
- <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/utils/log/logging_mixin.py#L175'>airflow/utils/log/logging_mixin.py:175:9:</a> PLR6301 Method `writable` could be a function, class method, or static method
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/utils/log/logging_mixin.py#L1'>airflow/utils/log/logging_mixin.py:1:1:</a> CPY001 Missing copyright notice at top of file
- <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/utils/log/logging_mixin.py#L1'>airflow/utils/log/logging_mixin.py:1:1:</a> CPY001 Missing copyright notice at top of file
... 488 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+246 -80 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/7f72b062d123e50edf1bc224f8ee02ad3f2e027f/scripts/check-env.py#L37'>scripts/check-env.py:37:40:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/superset/blob/7f72b062d123e50edf1bc224f8ee02ad3f2e027f/superset/advanced_data_type/types.py#L58'>superset/advanced_data_type/types.py:58:21:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/superset/blob/7f72b062d123e50edf1bc224f8ee02ad3f2e027f/superset/advanced_data_type/types.py#L59'>superset/advanced_data_type/types.py:59:23:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/apache/superset/blob/7f72b062d123e50edf1bc224f8ee02ad3f2e027f/superset/async_events/async_query_manager.py#L106'>superset/async_events/async_query_manager.py:106:22:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/apache/superset/blob/7f72b062d123e50edf1bc224f8ee02ad3f2e027f/superset/async_events/async_query_manager.py#L106'>superset/async_events/async_query_manager.py:106:22:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/apache/superset/blob/7f72b062d123e50edf1bc224f8ee02ad3f2e027f/superset/async_events/async_query_manager.py#L108'>superset/async_events/async_query_manager.py:108:29:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/apache/superset/blob/7f72b062d123e50edf1bc224f8ee02ad3f2e027f/superset/async_events/async_query_manager.py#L108'>superset/async_events/async_query_manager.py:108:29:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/apache/superset/blob/7f72b062d123e50edf1bc224f8ee02ad3f2e027f/superset/async_events/async_query_manager.py#L109'>superset/async_events/async_query_manager.py:109:38:</a> UP007 Use `X | Y` for type annotations
- <a href='https://github.com/apache/superset/blob/7f72b062d123e50edf1bc224f8ee02ad3f2e027f/superset/async_events/async_query_manager.py#L109'>superset/async_events/async_query_manager.py:109:38:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/apache/superset/blob/7f72b062d123e50edf1bc224f8ee02ad3f2e027f/superset/async_events/async_query_manager.py#L112'>superset/async_events/async_query_manager.py:112:34:</a> UP007 Use `X | Y` for type annotations
... 316 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+250 -0 violations, +0 -0 fixes)</summary>
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
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/pipeline.py#L24'>release/pipeline.py:24:12:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/pipeline.py#L34'>release/pipeline.py:34:31:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/ui.py#L103'>release/ui.py:103:33:</a> UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/ui.py#L24'>release/ui.py:24:17:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 240 additional changes omitted for project
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
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+299 -1149 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/langchain_core/_api/beta_decorator.py#L128'>libs/core/langchain_core/_api/beta_decorator.py:128:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/langchain_core/_api/beta_decorator.py#L186'>libs/core/langchain_core/_api/beta_decorator.py:186:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/langchain_core/_api/beta_decorator.py#L201'>libs/core/langchain_core/_api/beta_decorator.py:201:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/langchain_core/_api/beta_decorator.py#L30'>libs/core/langchain_core/_api/beta_decorator.py:30:30:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/langchain_core/_api/beta_decorator.py#L39'>libs/core/langchain_core/_api/beta_decorator.py:39:6:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/langchain_core/_api/deprecation.py#L202'>libs/core/langchain_core/_api/deprecation.py:202:35:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 294 additional changes omitted for rule UP006
- <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/langchain_core/caches.py#L149'>libs/core/langchain_core/caches.py:149:36:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/langchain_core/caches.py#L167'>libs/core/langchain_core/caches.py:167:55:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/langchain_core/caches.py#L200'>libs/core/langchain_core/caches.py:200:62:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/langchain_core/caches.py#L52'>libs/core/langchain_core/caches.py:52:55:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/langchain_core/caches.py#L97'>libs/core/langchain_core/caches.py:97:62:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/langchain_core/callbacks/base.py#L104'>libs/core/langchain_core/callbacks/base.py:104:24:</a> UP045 Use `X | None` for type annotations
... 1143 additional changes omitted for rule UP045
- <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/tests/unit_tests/tracers/test_langchain.py#L103'>libs/core/tests/unit_tests/tracers/test_langchain.py:103:5:</a> B903 Class could be dataclass or namedtuple
... 1435 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+13 -13 violations, +0 -0 fixes)</summary>
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
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+47 -208 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/app.py#L342'>lnbits/app.py:342:46:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/app.py#L352'>lnbits/app.py:352:47:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L102'>lnbits/core/models.py:102:15:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L107'>lnbits/core/models.py:107:20:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L108'>lnbits/core/models.py:108:15:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L109'>lnbits/core/models.py:109:15:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L110'>lnbits/core/models.py:110:12:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L111'>lnbits/core/models.py:111:19:</a> UP045 Use `X | None` for type annotations
... 203 additional changes omitted for rule UP045
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L332'>lnbits/core/models.py:332:30:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/models.py#L333'>lnbits/core/models.py:333:31:</a> UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
... 245 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (11 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP006 | 2431 | 2431 | 0 | 0 | 0 |
| UP045 | 1552 | 0 | 1552 | 0 | 0 |
| UP007 | 92 | 92 | 0 | 0 | 0 |
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

_Comment by @AlexWaygood on 2025-01-07 20:01_

heh, there's even a TODO next to this ecosystem hit! https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L512

---

_Comment by @AlexWaygood on 2025-01-07 20:02_

no false positives in that ecosystem report, from what I can see 🚀

---

_Merged by @AlexWaygood on 2025-01-07 20:03_

---

_Closed by @AlexWaygood on 2025-01-07 20:03_

---

_Branch deleted on 2025-01-07 20:03_

---
