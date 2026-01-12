```yaml
number: 18848
title: "Stabilize `pytest-warns-without-warning (PT029)`"
type: pull_request
state: closed
author: harupy
labels: []
assignees: []
base: main
head: mark-PT029-as-stable
created_at: 2025-06-21T08:07:41Z
updated_at: 2025-06-23T12:11:20Z
url: https://github.com/astral-sh/ruff/pull/18848
synced_at: 2026-01-12T15:56:26Z
```

# Stabilize `pytest-warns-without-warning (PT029)`

---

_@harupy_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

PT030 and PT031 (added by #15444) were stabilized in v0.12. PT029 also should be stabilized. Noticed this while running ruff v0.12.0

## Test Plan

<!-- How was it tested? -->

Existing checks


---

_Comment by @github-actions[bot] on 2025-06-21 08:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+10 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/fdd9fc1c536cc8e08cf40174081467a739859237/airflow-core/tests/unit/datasets/test_dataset.py#L76'>airflow-core/tests/unit/datasets/test_dataset.py:76:10:</a> PT029 Set the expected warning in `pytest.warns()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/592a41a1a504af38f235557b2a1b16228d8eda0a/pandas/tests/copy_view/test_chained_assignment_deprecation.py#L20'>pandas/tests/copy_view/test_chained_assignment_deprecation.py:20:10:</a> PT029 Set the expected warning in `pytest.warns()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/io/ascii/tests/test_c_reader.py#L1175'>astropy/io/ascii/tests/test_c_reader.py:1175:15:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/io/ascii/tests/test_c_reader.py#L1281'>astropy/io/ascii/tests/test_c_reader.py:1281:15:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/io/ascii/tests/test_c_reader.py#L1288'>astropy/io/ascii/tests/test_c_reader.py:1288:19:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/io/ascii/tests/test_c_reader.py#L1352'>astropy/io/ascii/tests/test_c_reader.py:1352:10:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/io/ascii/tests/test_c_reader.py#L1363'>astropy/io/ascii/tests/test_c_reader.py:1363:10:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/io/fits/hdu/compressed/tests/test_compressed.py#L505'>astropy/io/fits/hdu/compressed/tests/test_compressed.py:505:18:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/wcs/tests/test_profiling.py#L72'>astropy/wcs/tests/test_profiling.py:72:10:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/wcs/tests/test_utils.py#L802'>astropy/wcs/tests/test_utils.py:802:15:</a> PT029 Set the expected warning in `pytest.warns()`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT029 | 10 | 10 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Added to milestone `v0.13` by @MichaReiser on 2025-06-21 15:28_

---

_Comment by @MichaReiser on 2025-06-21 15:28_

Thank you, but we can't merge this before v0.13

---

_Comment by @harupy on 2025-06-22 08:41_

@MichaReiser  Thanks for the comment.

> Thank you, but we can't merge this before v0.13

because rule stabilization is only allowed in a minor version release?

---

_Comment by @MichaReiser on 2025-06-22 13:14_

Yes, we can't stabilize rules in patch releases 

---

_Comment by @ntBre on 2025-06-23 11:43_

We considered stabilizing PT029 in 0.12, but realized it might be outdated and actually better to deprecate it. See https://github.com/astral-sh/ruff/issues/18597 and https://github.com/astral-sh/ruff/pull/18567.

---

_Comment by @harupy on 2025-06-23 12:11_

@ntBre Deprecating PT029 makes sense :) Let me close this PR.

---

_Closed by @harupy on 2025-06-23 12:11_

---
