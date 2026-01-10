```yaml
number: 15328
title: "Spruce up docs for `slice-to-remove-prefix-or-suffix` (`FURB188`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
assignees: []
merged: true
base: main
head: alex/furb188-docs
created_at: 2025-01-07T19:39:33Z
updated_at: 2025-01-07T19:58:36Z
url: https://github.com/astral-sh/ruff/pull/15328
synced_at: 2026-01-10T20:34:00Z
```

# Spruce up docs for `slice-to-remove-prefix-or-suffix` (`FURB188`)

---

_Pull request opened by @AlexWaygood on 2025-01-07 19:39_

## Summary

Spruce up the docs for this rule, in preparation for possible stabilisation


---

_Label `documentation` added by @AlexWaygood on 2025-01-07 19:40_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-07 19:40_

---

_Review requested from @dylwil3 by @AlexWaygood on 2025-01-07 19:40_

---

_Comment by @github-actions[bot] on 2025-01-07 19:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+21 -21 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+9 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/configuration.py#L1313'>airflow/configuration.py:1313:17:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/configuration.py#L1313'>airflow/configuration.py:1313:17:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/utils/log/action_logger.py#L22'>airflow/utils/log/action_logger.py:22:5:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/airflow/utils/log/action_logger.py#L22'>airflow/utils/log/action_logger.py:22:5:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/dev/breeze/src/airflow_breeze/utils/cdxgen.py#L589'>dev/breeze/src/airflow_breeze/utils/cdxgen.py:589:9:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/dev/breeze/src/airflow_breeze/utils/cdxgen.py#L589'>dev/breeze/src/airflow_breeze/utils/cdxgen.py:589:9:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/dev/breeze/src/airflow_breeze/utils/reproducible.py#L142'>dev/breeze/src/airflow_breeze/utils/reproducible.py:142:25:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/dev/breeze/src/airflow_breeze/utils/reproducible.py#L142'>dev/breeze/src/airflow_breeze/utils/reproducible.py:142:25:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/providers/src/airflow/providers/elasticsearch/log/es_response.py#L101'>providers/src/airflow/providers/elasticsearch/log/es_response.py:101:14:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/providers/src/airflow/providers/elasticsearch/log/es_response.py#L101'>providers/src/airflow/providers/elasticsearch/log/es_response.py:101:14:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/providers/src/airflow/providers/google/cloud/operators/dataproc_metastore.py#L709'>providers/src/airflow/providers/google/cloud/operators/dataproc_metastore.py:709:16:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/providers/src/airflow/providers/google/cloud/operators/dataproc_metastore.py#L709'>providers/src/airflow/providers/google/cloud/operators/dataproc_metastore.py:709:16:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/providers/src/airflow/providers/opensearch/log/os_response.py#L101'>providers/src/airflow/providers/opensearch/log/os_response.py:101:14:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/providers/src/airflow/providers/opensearch/log/os_response.py#L101'>providers/src/airflow/providers/opensearch/log/os_response.py:101:14:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/providers/tests/elasticsearch/log/test_es_response.py#L186'>providers/tests/elasticsearch/log/test_es_response.py:186:13:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/providers/tests/elasticsearch/log/test_es_response.py#L186'>providers/tests/elasticsearch/log/test_es_response.py:186:13:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
- <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/providers/tests/opensearch/log/test_os_response.py#L107'>providers/tests/opensearch/log/test_os_response.py:107:13:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/apache/airflow/blob/4c5d85aeec5382d4da6505f33581076b526c37de/providers/tests/opensearch/log/test_os_response.py#L107'>providers/tests/opensearch/log/test_os_response.py:107:13:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L511'>src/bokeh/util/compiler.py:511:9:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L511'>src/bokeh/util/compiler.py:511:9:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/file_server.py#L63'>tests/support/plugins/file_server.py:63:9:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/file_server.py#L63'>tests/support/plugins/file_server.py:63:9:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/langchain_core/language_models/llms.py#L352'>libs/core/langchain_core/language_models/llms.py:352:9:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/langchain-ai/langchain/blob/ce9e9f93149ca264ac92e7481f42b6e4cdafae6e/libs/core/langchain_core/language_models/llms.py#L352'>libs/core/langchain_core/language_models/llms.py:352:9:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/snakemake/single_task_snakemake.py#L365'>src/latch_cli/snakemake/single_task_snakemake.py:365:12:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/snakemake/single_task_snakemake.py#L365'>src/latch_cli/snakemake/single_task_snakemake.py:365:12:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/utils/__init__.py#L106'>src/latch_cli/utils/__init__.py:106:5:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/utils/__init__.py#L106'>src/latch_cli/utils/__init__.py:106:5:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/reflex-dev/reflex/blob/08d9fbf9bc5846c4f60737abcd8d8bf1d134ad96/reflex/components/component.py#L664'>reflex/components/component.py:664:17:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/reflex-dev/reflex/blob/08d9fbf9bc5846c4f60737abcd8d8bf1d134ad96/reflex/components/component.py#L664'>reflex/components/component.py:664:17:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/io/fits/hdu/compressed/_tiled_compression.py#L259'>astropy/io/fits/hdu/compressed/_tiled_compression.py:259:5:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/io/fits/hdu/compressed/_tiled_compression.py#L259'>astropy/io/fits/hdu/compressed/_tiled_compression.py:259:5:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
- <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/io/fits/header.py#L1646'>astropy/io/fits/header.py:1646:9:</a> FURB188 [*] Prefer `removeprefix` over conditionally replacing with slice.
+ <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/io/fits/header.py#L1646'>astropy/io/fits/header.py:1646:9:</a> FURB188 [*] Prefer `str.removeprefix()` over conditionally replacing with slice.
- <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/io/votable/converters.py#L1056'>astropy/io/votable/converters.py:1056:13:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/io/votable/converters.py#L1056'>astropy/io/votable/converters.py:1056:13:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
- <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/io/votable/converters.py#L1058'>astropy/io/votable/converters.py:1058:13:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/io/votable/converters.py#L1058'>astropy/io/votable/converters.py:1058:13:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
- <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/io/votable/converters.py#L326'>astropy/io/votable/converters.py:326:13:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/io/votable/converters.py#L326'>astropy/io/votable/converters.py:326:13:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
- <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/table/meta.py#L211'>astropy/table/meta.py:211:5:</a> FURB188 [*] Prefer `removesuffix` over conditionally replacing with slice.
+ <a href='https://github.com/astropy/astropy/blob/954300c11198c46c5f364b1947534294bca88ca3/astropy/table/meta.py#L211'>astropy/table/meta.py:211:5:</a> FURB188 [*] Prefer `str.removesuffix()` over conditionally replacing with slice.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB188 | 42 | 21 | 21 | 0 | 0 |

</p>
</details>




---

_@dylwil3 approved on 2025-01-07 19:56_

Much improved!

It looks like there is some inconsistency in the lint messages across Ruff around whether to refer to a method with or without parentheses - but that sounds like an issue for another day.

---

_Merged by @AlexWaygood on 2025-01-07 19:58_

---

_Closed by @AlexWaygood on 2025-01-07 19:58_

---

_Branch deleted on 2025-01-07 19:58_

---
