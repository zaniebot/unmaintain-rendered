```yaml
number: 20269
title: "[`flake8-no-pep420`] Stabilize allowing nested roots in `implicit-namespace-package` (`INP001`)"
type: pull_request
state: closed
author: dylwil3
labels: []
assignees: []
base: brent/0.13.0
head: dylan/stabilize-nested-root
created_at: 2025-09-05T18:42:29Z
updated_at: 2025-09-05T19:23:05Z
url: https://github.com/astral-sh/ruff/pull/20269
synced_at: 2026-01-10T17:46:21Z
```

# [`flake8-no-pep420`] Stabilize allowing nested roots in `implicit-namespace-package` (`INP001`)

---

_Pull request opened by @dylwil3 on 2025-09-05 18:42_

Stabilizes the behavior from #14236

This was never documented, so we did not update documentation for the rule.


---

_Comment by @github-actions[bot] on 2025-09-05 18:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e10966b33b77dc86035ea3bbeb5da8603f542615/airflow-core/tests/unit/dags/test_zip/test_zip_module/__init__.py#L1'>airflow-core/tests/unit/dags/test_zip/test_zip_module/__init__.py:1:1:</a> INP001 File `airflow-core/tests/unit/dags/test_zip/test_zip_module/__init__.py` declares a package, but is nested under an implicit namespace package. Add an `__init__.py` to `airflow-core/tests/unit/dags/test_zip`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/embed/ext_package_no_main/__init__.py#L1'>tests/unit/bokeh/embed/ext_package_no_main/__init__.py:1:1:</a> INP001 File `tests/unit/bokeh/embed/ext_package_no_main/__init__.py` declares a package, but is nested under an implicit namespace package. Add an `__init__.py` to `tests/unit/bokeh/embed`.
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/embed/latex_label/__init__.py#L1'>tests/unit/bokeh/embed/latex_label/__init__.py:1:1:</a> INP001 File `tests/unit/bokeh/embed/latex_label/__init__.py` declares a package, but is nested under an implicit namespace package. Add an `__init__.py` to `tests/unit/bokeh/embed`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/4e39c164bb55ba5f877beb555a1797fc55c31baa/libs/langchain/tests/integration_tests/retrievers/document_compressors/__init__.py#L1'>libs/langchain/tests/integration_tests/retrievers/document_compressors/__init__.py:1:1:</a> INP001 File `libs/langchain/tests/integration_tests/retrievers/document_compressors/__init__.py` declares a package, but is nested under an implicit namespace package. Add an `__init__.py` to `libs/langchain/tests/integration_tests/retrievers`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/51f9f2c2f52cac4d66c07683a12bc0237311b6be/reflex/.templates/apps/blank/code/__init__.py#L1'>reflex/.templates/apps/blank/code/__init__.py:1:1:</a> INP001 File `reflex/.templates/apps/blank/code/__init__.py` declares a package, but is nested under an implicit namespace package. Add an `__init__.py` to `reflex/.templates/apps/blank`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/2c5291e5478d8f3975d7a1b1eba307e72f545577/astropy/utils/tests/data/test_package/__init__.py#L1'>astropy/utils/tests/data/test_package/__init__.py:1:1:</a> INP001 File `astropy/utils/tests/data/test_package/__init__.py` declares a package, but is nested under an implicit namespace package. Add an `__init__.py` to `astropy/utils/tests/data`.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| INP001 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dylwil3 on 2025-09-05 19:22_

Hmm. Looking at the ecosystem report, I guess this exacerbates the problem from https://github.com/astral-sh/ruff/issues/6474 so maybe better to resolve that first.

---

_Closed by @dylwil3 on 2025-09-05 19:22_

---

_Branch deleted on 2025-09-05 19:23_

---
