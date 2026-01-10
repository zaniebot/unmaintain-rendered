```yaml
number: 18568
title: "Stabilize `pytest-warns-too-broad` (`PT030`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-pt030
created_at: 2025-06-08T19:22:05Z
updated_at: 2025-06-09T21:22:27Z
url: https://github.com/astral-sh/ruff/pull/18568
synced_at: 2026-01-10T18:45:04Z
```

# Stabilize `pytest-warns-too-broad` (`PT030`)

---

_Pull request opened by @dylwil3 on 2025-06-08 19:22_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-08 19:22_

---

_Label `rule` added by @dylwil3 on 2025-06-08 19:22_

---

_Comment by @github-actions[bot] on 2025-06-08 19:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+34 -0 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_embeds.py#L473'>tests/test_embeds.py:473:23:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_embeds.py#L475'>tests/test_embeds.py:475:23:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_embeds.py#L477'>tests/test_embeds.py:477:23:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/airflow-core/tests/unit/core/test_configuration.py#L1043'>airflow-core/tests/unit/core/test_configuration.py:1043:31:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/airflow-core/tests/unit/core/test_configuration.py#L1047'>airflow-core/tests/unit/core/test_configuration.py:1047:31:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/airflow-core/tests/unit/core/test_configuration.py#L1112'>airflow-core/tests/unit/core/test_configuration.py:1112:35:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/providers/celery/tests/unit/celery/executors/test_celery_executor.py#L263'>providers/celery/tests/unit/celery/executors/test_celery_executor.py:263:31:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/providers/cncf/kubernetes/tests/unit/cncf/kubernetes/executors/test_kubernetes_executor.py#L1262'>providers/cncf/kubernetes/tests/unit/cncf/kubernetes/executors/test_kubernetes_executor.py:1262:27:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/task-sdk/tests/task_sdk/bases/test_operator.py#L253'>task-sdk/tests/task_sdk/bases/test_operator.py:253:27:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/task-sdk/tests/task_sdk/definitions/test_asset.py#L120'>task-sdk/tests/task_sdk/definitions/test_asset.py:120:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_plots.py#L81'>tests/unit/bokeh/models/test_plots.py:81:27:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L765'>tests/unit/bokeh/models/test_sources.py:765:27:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L771'>tests/unit/bokeh/models/test_sources.py:771:27:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L777'>tests/unit/bokeh/models/test_sources.py:777:27:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L783'>tests/unit/bokeh/models/test_sources.py:783:27:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test_graph.py#L122'>tests/unit/bokeh/plotting/test_graph.py:122:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test_graph.py#L131'>tests/unit/bokeh/plotting/test_graph.py:131:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test_graph.py#L141'>tests/unit/bokeh/plotting/test_graph.py:141:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test_graph.py#L175'>tests/unit/bokeh/plotting/test_graph.py:175:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/util/test_token.py#L217'>tests/unit/bokeh/util/test_token.py:217:27:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/util/test_token.py#L232'>tests/unit/bokeh/util/test_token.py:232:31:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/tests/unit_tests/prompts/test_few_shot.py#L269'>libs/core/tests/unit_tests/prompts/test_few_shot.py:269:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/tests/unit_tests/prompts/test_few_shot.py#L287'>libs/core/tests/unit_tests/prompts/test_few_shot.py:287:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/tests/unit_tests/prompts/test_few_shot.py#L314'>libs/core/tests/unit_tests/prompts/test_few_shot.py:314:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/tests/unit_tests/prompts/test_prompt.py#L512'>libs/core/tests/unit_tests/prompts/test_prompt.py:512:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/tests/unit_tests/prompts/test_prompt.py#L529'>libs/core/tests/unit_tests/prompts/test_prompt.py:529:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/tests/unit_tests/prompts/test_prompt.py#L546'>libs/core/tests/unit_tests/prompts/test_prompt.py:546:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/68c7cfc8e042b3cafb56ffce773191c959bf4c3c/astropy/coordinates/tests/test_velocity_corrs.py#L293'>astropy/coordinates/tests/test_velocity_corrs.py:293:23:</a> PT030 `pytest.warns(Warning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/astropy/astropy/blob/68c7cfc8e042b3cafb56ffce773191c959bf4c3c/astropy/io/ascii/tests/test_qdp.py#L222'>astropy/io/ascii/tests/test_qdp.py:222:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/astropy/astropy/blob/68c7cfc8e042b3cafb56ffce773191c959bf4c3c/astropy/io/fits/hdu/compressed/tests/test_compressed.py#L500'>astropy/io/fits/hdu/compressed/tests/test_compressed.py:500:31:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/astropy/astropy/blob/68c7cfc8e042b3cafb56ffce773191c959bf4c3c/astropy/io/fits/tests/test_header.py#L195'>astropy/io/fits/tests/test_header.py:195:22:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/astropy/astropy/blob/68c7cfc8e042b3cafb56ffce773191c959bf4c3c/astropy/table/tests/test_table.py#L2334'>astropy/table/tests/test_table.py:2334:27:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/astropy/astropy/blob/68c7cfc8e042b3cafb56ffce773191c959bf4c3c/astropy/units/tests/test_quantity_non_ufuncs.py#L2029'>astropy/units/tests/test_quantity_non_ufuncs.py:2029:27:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/astropy/astropy/blob/68c7cfc8e042b3cafb56ffce773191c959bf4c3c/astropy/wcs/wcsapi/tests/test_fitswcs.py#L1151'>astropy/wcs/wcsapi/tests/test_fitswcs.py:1151:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT030 | 34 | 34 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-06-08 19:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-08 20:27_

---

_@ntBre approved on 2025-06-09 21:12_

---

_Merged by @dylwil3 on 2025-06-09 21:16_

---

_Closed by @dylwil3 on 2025-06-09 21:16_

---

_Branch deleted on 2025-06-09 21:16_

---
