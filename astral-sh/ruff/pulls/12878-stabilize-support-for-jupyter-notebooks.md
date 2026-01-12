```yaml
number: 12878
title: Stabilize support for Jupyter Notebooks
type: pull_request
state: merged
author: dhruvmanila
labels:
  - breaking
  - configuration
  - notebook
assignees: []
merged: true
base: ruff-0.6
head: dhruv/include-notebooks-by-default
created_at: 2024-08-14T05:45:39Z
updated_at: 2024-08-14T16:02:29Z
url: https://github.com/astral-sh/ruff/pull/12878
synced_at: 2026-01-12T15:55:42Z
```

# Stabilize support for Jupyter Notebooks

---

_@dhruvmanila_

## Summary

This PR stabilizes the support for Jupyter Notebooks and adds them to Ruff's default inclusion set.

Closes: #12456
Closes: https://github.com/astral-sh/ruff-vscode/issues/546

## Test Plan

- [x] Make sure all tests pass, updated snapshots are valid
- [x] Ecosystem checks?
- [x] Run Ruff in a repository containing both Python files and Jupyter notebooks

---

_Label `breaking` added by @dhruvmanila on 2024-08-14 05:45_

---

_Label `notebook` added by @dhruvmanila on 2024-08-14 05:45_

---

_Added to milestone `v0.6` by @MichaReiser on 2024-08-14 06:45_

---

_Review comment by @T-256 on `docs/configuration.md`:423 on 2024-08-14 09:58_

I think we won't need this section anymore. we can move this section to another place that applies for all file types not just .ipynb

---

_@T-256 reviewed on 2024-08-14 09:58_

---

_Marked ready for review by @dhruvmanila on 2024-08-14 10:21_

---

_@dhruvmanila reviewed on 2024-08-14 10:23_

---

_Review comment by @dhruvmanila on `docs/configuration.md`:345 on 2024-08-14 10:23_

Any suggestion on what can be use for this example? I'm worried that it _might_ signal to the users that we support that file extension. Using `*.ipynb` was ok because we do support it. This is the reason I've removed this example.

---

_Comment by @dhruvmanila on 2024-08-14 10:24_

There are couple more references in the docs that require updating which I'm doing right now.

---

_Comment by @github-actions[bot] on 2024-08-14 10:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1109 -0 violations, +0 -0 fixes in 8 projects; 46 projects unchanged)

<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ docs/source/guides/time_series.ipynb:cell 1:4:1: E402 Module level import not at top of cell
+ docs/source/guides/time_series.ipynb:cell 1:6:1: E402 Module level import not at top of cell
+ docs/source/guides/time_series.ipynb:cell 1:7:1: E402 Module level import not at top of cell
+ docs/source/guides/time_series.ipynb:cell 1:8:1: E402 Module level import not at top of cell
+ docs/source/resources/frequently_asked_questions.ipynb:cell 101:5:8: E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+170 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ docs/notebooks/analysis/fit_functions.ipynb:cell 12:3:39: NPY002 Replace legacy `np.random.normal` call with `np.random.Generator`
+ docs/notebooks/analysis/nullpoint.ipynb:cell 11:10:1: T201 `print` found
+ docs/notebooks/analysis/nullpoint.ipynb:cell 11:9:1: T201 `print` found
+ docs/notebooks/analysis/nullpoint.ipynb:cell 14:10:1: T201 `print` found
+ docs/notebooks/analysis/nullpoint.ipynb:cell 14:11:1: T201 `print` found
+ docs/notebooks/analysis/nullpoint.ipynb:cell 9:1:5: D103 Missing docstring in public function
+ docs/notebooks/analysis/swept_langmuir/find_floating_potential.ipynb:cell 24:29:42: FBT003 Boolean positional value in function call
+ docs/notebooks/analysis/swept_langmuir/find_floating_potential.ipynb:cell 24:29:48: FBT003 Boolean positional value in function call
+ docs/notebooks/analysis/swept_langmuir/find_floating_potential.ipynb:cell 24:30:42: FBT003 Boolean positional value in function call
+ docs/notebooks/analysis/swept_langmuir/find_floating_potential.ipynb:cell 24:30:48: FBT003 Boolean positional value in function call
... 160 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+31 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ dev/stats/explore_pr_candidates.ipynb:cell 1:1:1: F821 Undefined name `sys`
+ dev/stats/explore_pr_candidates.ipynb:cell 1:1:1: I002 [*] Missing required import: `from __future__ import annotations`
+ dev/stats/explore_pr_candidates.ipynb:cell 1:3:1: E402 Module level import not at top of cell
+ dev/stats/explore_pr_candidates.ipynb:cell 1:3:1: I001 [*] Import block is un-sorted or un-formatted
+ dev/stats/explore_pr_candidates.ipynb:cell 1:4:1: E402 Module level import not at top of cell
+ dev/stats/explore_pr_candidates.ipynb:cell 1:4:8: TID253 `pandas` is banned at the module level
+ dev/stats/explore_pr_candidates.ipynb:cell 1:5:1: E402 Module level import not at top of cell
+ dev/stats/explore_pr_candidates.ipynb:cell 1:5:41: F401 [*] `get_important_pr_candidates.PrStat` imported but unused
+ dev/stats/explore_pr_candidates.ipynb:cell 2:1:8: PTH123 `open()` should be replaced by `Path.open()`
+ dev/stats/explore_pr_candidates.ipynb:cell 2:1:8: SIM115 Use context handler for opening files
... 21 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+668 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 10:3:15: Q000 [*] Single quotes found but double quotes preferred
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 10:3:1: T201 `print` found
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 10:3:38: Q000 [*] Single quotes found but double quotes preferred
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 11:1:1: PD901 Avoid using the generic variable name `df` for DataFrames
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 11:1:32: Q000 [*] Single quotes found but double quotes preferred
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 11:1:53: Q000 [*] Single quotes found but double quotes preferred
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 13:100:3: Q000 [*] Single quotes found but double quotes preferred
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 13:101:3: Q000 [*] Single quotes found but double quotes preferred
... 482 additional changes omitted for rule Q000
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 13:10:14: W291 [*] Trailing whitespace
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 13:237:13: COM812 [*] Trailing comma missing
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 13:254:19: COM812 [*] Trailing comma missing
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 13:258:89: E501 Line too long (145 > 88)
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 13:261:3: T201 `print` found
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 13:30:89: E501 Line too long (121 > 88)
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 13:49:89: E501 Line too long (92 > 88)
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 13:50:3: ERA001 Found commented-out code
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 13:50:89: E501 Line too long (99 > 88)
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 13:72:89: E501 Line too long (94 > 88)
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 13:81:3: ERA001 Found commented-out code
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 13:82:11: W291 [*] Trailing whitespace
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 15:3:89: E501 Line too long (95 > 88)
... 39 additional changes omitted for rule E501
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 15:7:22: COM812 [*] Trailing comma missing
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 16:11:9: T201 `print` found
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 16:19:5: T201 `print` found
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 16:1:13: ANN001 Missing type annotation for function argument `country`
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 16:1:5: ANN201 Missing return type annotation for public function `get_gdf`
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 16:1:5: D103 Missing docstring in public function
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 16:30:69: W291 [*] Trailing whitespace
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 16:9:24: ANN001 Missing type annotation for function argument `countries`
+ superset-frontend/plugins/legacy-plugin-chart-country-map/scripts/Country Map GeoJSON Generator.ipynb:cell 16:9:35: ANN001 Missing type annotation for function argument `subplot_width`
... 638 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+160 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ examples/models/structure/ModelStructureExample.ipynb:cell 11:2:5: T201 `print` found
+ examples/models/structure/ModelStructureExample.ipynb:cell 13:1:1: B018 Found useless expression. Either assign it to a variable or remove it.
+ examples/models/structure/ModelStructureExample.ipynb:cell 13:2:17: Q000 [*] Single quotes found but double quotes preferred
+ examples/models/structure/ModelStructureExample.ipynb:cell 13:2:40: Q000 [*] Single quotes found but double quotes preferred
+ examples/models/structure/ModelStructureExample.ipynb:cell 13:2:59: Q000 [*] Single quotes found but double quotes preferred
+ examples/models/structure/ModelStructureExample.ipynb:cell 13:2:64: Q000 [*] Single quotes found but double quotes preferred
+ examples/models/structure/ModelStructureExample.ipynb:cell 14:1:15: Q000 [*] Single quotes found but double quotes preferred
+ examples/models/structure/ModelStructureExample.ipynb:cell 14:1:1: F821 Undefined name `pd`
+ examples/models/structure/ModelStructureExample.ipynb:cell 2:1:1: I001 [*] Import block is un-sorted or un-formatted
+ examples/models/structure/ModelStructureExample.ipynb:cell 2:1:8: F401 [*] `bokeh` imported but unused
... 150 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+31 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ libs/cli/langchain_cli/integration_template/docs/chat.ipynb:cell 11:4:89: E501 Line too long (102 > 88)
+ libs/cli/langchain_cli/integration_template/docs/chat.ipynb:cell 12:1:1: T201 `print` found
+ libs/cli/langchain_cli/integration_template/docs/chat.ipynb:cell 14:7:89: E501 Line too long (97 > 88)
+ libs/cli/langchain_cli/integration_template/docs/chat.ipynb:cell 3:5:89: E501 Line too long (98 > 88)
+ libs/cli/langchain_cli/integration_template/docs/document_loaders.ipynb:cell 12:1:1: T201 `print` found
+ libs/cli/langchain_cli/integration_template/docs/document_loaders.ipynb:cell 3:4:89: E501 Line too long (94 > 88)
+ libs/cli/langchain_cli/integration_template/docs/llms.ipynb:cell 3:5:89: E501 Line too long (98 > 88)
+ libs/cli/langchain_cli/integration_template/docs/provider.ipynb:cell 2:1:1: I001 [*] Import block is un-sorted or un-formatted
+ libs/cli/langchain_cli/integration_template/docs/provider.ipynb:cell 2:1:29: F401 [*] `__module_name__.Chat__ModuleName__` imported but unused
+ libs/cli/langchain_cli/integration_template/docs/provider.ipynb:cell 2:2:29: F401 [*] `__module_name__.__ModuleName__LLM` imported but unused
... 21 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ examples/hello_milvus.ipynb:cell 14:13:9: T201 `print` found
+ examples/hello_milvus.ipynb:cell 14:14:1: T201 `print` found
+ examples/hello_milvus.ipynb:cell 16:5:1: T201 `print` found
+ examples/hello_milvus.ipynb:cell 16:6:1: T201 `print` found
+ examples/hello_milvus.ipynb:cell 18:7:9: T201 `print` found
+ examples/hello_milvus.ipynb:cell 18:8:1: T201 `print` found
... 5 additional changes omitted for rule T201
+ examples/hello_milvus.ipynb:cell 2:1:1: I001 [*] Import block is un-sorted or un-formatted
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+33 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ doc/source/user_guide/style.ipynb:cell 113:31:89: E501 Line too long (90 > 88)
+ doc/source/user_guide/style.ipynb:cell 113:33:89: E501 Line too long (96 > 88)
+ doc/source/user_guide/style.ipynb:cell 123:3:56: E741 Ambiguous variable name: `l`
+ doc/source/user_guide/style.ipynb:cell 125:2:13: C408 Unnecessary `dict` call (rewrite as a literal)
+ doc/source/user_guide/style.ipynb:cell 125:4:13: C408 Unnecessary `dict` call (rewrite as a literal)
+ doc/source/user_guide/style.ipynb:cell 125:6:13: C408 Unnecessary `dict` call (rewrite as a literal)
+ doc/source/user_guide/style.ipynb:cell 125:8:13: C408 Unnecessary `dict` call (rewrite as a literal)
+ doc/source/user_guide/style.ipynb:cell 126:1:1: NPY002 Replace legacy `np.random.seed` call with `np.random.Generator`
+ doc/source/user_guide/style.ipynb:cell 126:2:1: PLW0127 Self-assignment of variable `cmap`
+ doc/source/user_guide/style.ipynb:cell 126:2:8: PLW0128 Redeclared variable `cmap` in assignment
... 23 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (70 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| Q000 | 539 | 539 | 0 | 0 | 0 |
| T201 | 151 | 151 | 0 | 0 | 0 |
| E501 | 74 | 74 | 0 | 0 | 0 |
| ANN001 | 62 | 62 | 0 | 0 | 0 |
| NPY002 | 32 | 32 | 0 | 0 | 0 |
| D103 | 25 | 25 | 0 | 0 | 0 |
| W293 | 23 | 23 | 0 | 0 | 0 |
| ANN201 | 22 | 22 | 0 | 0 | 0 |
| FBT003 | 17 | 17 | 0 | 0 | 0 |
| I001 | 12 | 12 | 0 | 0 | 0 |
| F401 | 12 | 12 | 0 | 0 | 0 |
| W291 | 11 | 11 | 0 | 0 | 0 |
| COM812 | 11 | 11 | 0 | 0 | 0 |
| E402 | 7 | 7 | 0 | 0 | 0 |
| PTH123 | 6 | 6 | 0 | 0 | 0 |
| C408 | 6 | 6 | 0 | 0 | 0 |
| F821 | 5 | 5 | 0 | 0 | 0 |
| ERA001 | 5 | 5 | 0 | 0 | 0 |
| ANN202 | 5 | 5 | 0 | 0 | 0 |
| E701 | 5 | 5 | 0 | 0 | 0 |
| PD011 | 4 | 4 | 0 | 0 | 0 |
| PD901 | 4 | 4 | 0 | 0 | 0 |
| PLR2004 | 4 | 4 | 0 | 0 | 0 |
| UP031 | 4 | 4 | 0 | 0 | 0 |
| B007 | 3 | 3 | 0 | 0 | 0 |
| T203 | 3 | 3 | 0 | 0 | 0 |
| SLF001 | 3 | 3 | 0 | 0 | 0 |
| RUF001 | 3 | 3 | 0 | 0 | 0 |
| PTH110 | 3 | 3 | 0 | 0 | 0 |
| E741 | 2 | 2 | 0 | 0 | 0 |
| PLR1736 | 2 | 2 | 0 | 0 | 0 |
| PLW2901 | 2 | 2 | 0 | 0 | 0 |
| I002 | 2 | 2 | 0 | 0 | 0 |
| PD002 | 2 | 2 | 0 | 0 | 0 |
| PLW0602 | 2 | 2 | 0 | 0 | 0 |
| ARG001 | 2 | 2 | 0 | 0 | 0 |
| E721 | 1 | 1 | 0 | 0 | 0 |
| B008 | 1 | 1 | 0 | 0 | 0 |
| TID253 | 1 | 1 | 0 | 0 | 0 |
| SIM115 | 1 | 1 | 0 | 0 | 0 |
| S301 | 1 | 1 | 0 | 0 | 0 |
| PLR0913 | 1 | 1 | 0 | 0 | 0 |
| BLE001 | 1 | 1 | 0 | 0 | 0 |
| RUF010 | 1 | 1 | 0 | 0 | 0 |
| PD008 | 1 | 1 | 0 | 0 | 0 |
| S113 | 1 | 1 | 0 | 0 | 0 |
| PTH111 | 1 | 1 | 0 | 0 | 0 |
| PTH202 | 1 | 1 | 0 | 0 | 0 |
| PTH102 | 1 | 1 | 0 | 0 | 0 |
| FBT001 | 1 | 1 | 0 | 0 | 0 |
| D400 | 1 | 1 | 0 | 0 | 0 |
| D415 | 1 | 1 | 0 | 0 | 0 |
| PTH107 | 1 | 1 | 0 | 0 | 0 |
| C405 | 1 | 1 | 0 | 0 | 0 |
| B018 | 1 | 1 | 0 | 0 | 0 |
| N803 | 1 | 1 | 0 | 0 | 0 |
| C901 | 1 | 1 | 0 | 0 | 0 |
| SIM910 | 1 | 1 | 0 | 0 | 0 |
| E711 | 1 | 1 | 0 | 0 | 0 |
| RUF005 | 1 | 1 | 0 | 0 | 0 |
| D205 | 1 | 1 | 0 | 0 | 0 |
| D212 | 1 | 1 | 0 | 0 | 0 |
| F841 | 1 | 1 | 0 | 0 | 0 |
| SIM108 | 1 | 1 | 0 | 0 | 0 |
| UP030 | 1 | 1 | 0 | 0 | 0 |
| UP032 | 1 | 1 | 0 | 0 | 0 |
| S506 | 1 | 1 | 0 | 0 | 0 |
| PLW0127 | 1 | 1 | 0 | 0 | 0 |
| PLW0128 | 1 | 1 | 0 | 0 | 0 |
| F811 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `configuration` added by @dhruvmanila on 2024-08-14 10:31_

---

_Comment by @MichaReiser on 2024-08-14 11:06_

Can you rebase/re-run the ecosystem checks. The augmented assignment adds a lot of noise.

---

_Review comment by @MichaReiser on `docs/configuration.md`:345 on 2024-08-14 11:08_

`extend-include` has an example with `.pyw` files. Although I think the example isn't needed because we already link to `extend-include` which has an example.

---

_Review comment by @MichaReiser on `docs/configuration.md`:372 on 2024-08-14 11:10_

I wonder if we should change the note to: For versions older than `0.6.0` linting and formatting requires explicit opt-in by adding `*.ipynb` to [`extend-include`]

And we may want to add an example of how to exclude notebooks altogether.

---

_@MichaReiser approved on 2024-08-14 11:11_

---

_Comment by @MichaReiser on 2024-08-14 11:11_

There are quiet a large number of violations and some violations seem less appropriate for notebooks (e.g. denying the use of `print`).

---

_Comment by @dhruvmanila on 2024-08-14 11:13_

> Can you rebase/re-run the ecosystem checks. The augmented assignment adds a lot of noise.

It it up-to date with `ruff-0.6` branch.

---

_Comment by @MichaReiser on 2024-08-14 11:16_

Let's see if re-triggering the ecosystem checks help

---

_Closed by @MichaReiser on 2024-08-14 11:16_

---

_Reopened by @MichaReiser on 2024-08-14 11:16_

---

_Comment by @dhruvmanila on 2024-08-14 11:31_

> Let's see if re-triggering the ecosystem checks help

Oh, I see what the problem is. The `ruff-0.6` branch needs to be rebased on `main` to remove the merge conflict. The CI is paused in that branch which means it never ran after the reverted commit.

---

_@MichaReiser reviewed on 2024-08-14 11:32_

---

_Review comment by @MichaReiser on `docs/configuration.md`:372 on 2024-08-14 11:32_

Looking at the ecosystem results, an example for how to disable some rules for notebooks only might help adoption.

---

_@dhruvmanila reviewed on 2024-08-14 11:36_

---

_Review comment by @dhruvmanila on `docs/configuration.md`:372 on 2024-08-14 11:36_

These are good suggestions. Let me make some edits.

---

_@dhruvmanila reviewed on 2024-08-14 11:38_

---

_Review comment by @dhruvmanila on `docs/configuration.md`:345 on 2024-08-14 11:38_

Yeah, I think not having an example here is fine.

---

_Comment by @dhruvmanila on 2024-08-14 12:19_

@MichaReiser I've made some edits as per your suggestions. Thanks.

I've also updated some rule documentation to reflect the different behavior for Jupyter Notebooks.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/not_missing.rs`:13 on 2024-08-14 13:18_

Should this be under the *What it does section*?

---

_Review comment by @MichaReiser on `docs/configuration.md`:371 on 2024-08-14 13:20_

The back reference of the second subsentence is unclear. It should refer to Jupyter files but the first sentence only mentions notebook support
```suggestion
    Jupyter Notebook support is marked as stable from Ruff version `0.6.0` onwards and Jupyter Notebooks are linted and formatted by default.
```

---

_Review comment by @MichaReiser on `docs/configuration.md`:408 on 2024-08-14 13:20_

```suggestion
[`extend-exclude`](settings.md#extend-exclude) setting:
```

---

_Review comment by @MichaReiser on `docs/configuration.md`:440 on 2024-08-14 13:21_

```suggestion
Some rules that have different behavior when applied to Jupyter Notebook files. For
```

---

_@MichaReiser approved on 2024-08-14 13:21_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-08-14 13:21_

---

_@dhruvmanila reviewed on 2024-08-14 13:24_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pydocstyle/rules/not_missing.rs`:13 on 2024-08-14 13:24_

I tried it in that section but found that including it at the top would be better mainly because it's not relevant to any existing section.

<img width="1395" alt="Screenshot 2024-08-14 at 18 53 58" src="https://github.com/user-attachments/assets/483b1bdf-4ee6-4cc9-a646-728e53e6909e">


---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-14 13:32_

---

_@MichaReiser reviewed on 2024-08-14 13:52_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/not_missing.rs`:13 on 2024-08-14 13:52_

Makes sense. I think it's a bit too prominent. We could introduce a new `Limitations` section maybe also with an explanation why the rule is ignored.

---

_@AlexWaygood reviewed on 2024-08-14 14:53_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pydocstyle/rules/not_missing.rs`:13 on 2024-08-14 14:53_

I've added a new `Notebook behavior` section to several rules for when they have different Jupyter notebook behavior.

---

_Comment by @AlexWaygood on 2024-08-14 14:59_

@MichaReiser, I pushed a few small updates. Would you mind having another quick look?

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-14 14:59_

---

_@MichaReiser reviewed on 2024-08-14 15:02_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:246 on 2024-08-14 15:02_

This is only true for one version. Seems a bit of a stretch :) I would probably remove the section because we normally only document the current behavior. 

---

_@AlexWaygood reviewed on 2024-08-14 15:03_

---

_Review comment by @AlexWaygood on `crates/ruff_workspace/src/options.rs`:246 on 2024-08-14 15:03_

makes sense

---

_@MichaReiser approved on 2024-08-14 15:03_

Looks good except the options documentation is a bit of a stretch ;) 

---

_Closed by @MichaReiser on 2024-08-14 15:37_

---

_Reopened by @MichaReiser on 2024-08-14 15:37_

---

_Merged by @AlexWaygood on 2024-08-14 16:01_

---

_Closed by @AlexWaygood on 2024-08-14 16:01_

---

_Branch deleted on 2024-08-14 16:01_

---
