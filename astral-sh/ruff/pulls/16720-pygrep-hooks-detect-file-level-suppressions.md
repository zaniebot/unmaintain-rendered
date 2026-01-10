```yaml
number: 16720
title: "[`pygrep-hooks`]: Detect file-level suppressions comments without rul…"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
  - breaking
assignees: []
merged: true
base: main
head: micha/blanked-noqa-file-level2
created_at: 2025-03-14T07:27:06Z
updated_at: 2025-03-14T08:37:18Z
url: https://github.com/astral-sh/ruff/pull/16720
synced_at: 2026-01-10T19:49:02Z
```

# [`pygrep-hooks`]: Detect file-level suppressions comments without rul…

---

_Pull request opened by @MichaReiser on 2025-03-14 07:27_

## Summary

I accidentially dropped this commit from the Ruff 0.10 release. See https://github.com/astral-sh/ruff/pull/16699



---

_Label `rule` added by @MichaReiser on 2025-03-14 07:27_

---

_Comment by @github-actions[bot] on 2025-03-14 07:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+5 -0 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/f701b8bbacd8517be14453932048889416570722/docs/exts/exampleinclude.py#L1'>docs/exts/exampleinclude.py:1:1:</a> PGH004 Use specific rule codes when using `ruff: noqa`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/e8ad096173c415b50965cc571ba4f24da6055cbc/tests/integration_tests/superset_test_config_sqllab_backend_persist_off.py#L17'>tests/integration_tests/superset_test_config_sqllab_backend_persist_off.py:17:1:</a> PGH004 Use specific rule codes when using `ruff: noqa`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/ec87085035996b718af8dc680caa461f0e49b854/testing/code/test_source.py#L2'>testing/code/test_source.py:2:1:</a> PGH004 Use specific rule codes when using `ruff: noqa`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pdm-project/pdm">pdm-project/pdm</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pdm-project/pdm/blob/d33544cba96c025973f1d3f2637824b8142de399/src/pdm/installers/__init__.py#L1'>src/pdm/installers/__init__.py:1:1:</a> PGH004 Use specific rule codes when using `ruff: noqa`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/301736fda6034a8f2b33e9140524102fa411c993/astropy/io/ascii/__init__.py#L3'>astropy/io/ascii/__init__.py:3:1:</a> PGH004 Use specific rule codes when using `ruff: noqa`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PGH004 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `breaking` added by @MichaReiser on 2025-03-14 08:37_

---

_Merged by @MichaReiser on 2025-03-14 08:37_

---

_Closed by @MichaReiser on 2025-03-14 08:37_

---

_Branch deleted on 2025-03-14 08:37_

---
