```yaml
number: 16699
title: "[`pygrep-hooks`]: Detect file-level suppressions comments without rule codes (`PGH004`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: micha/blanked-noqa-file-level
created_at: 2025-03-13T08:26:05Z
updated_at: 2025-03-14T07:24:26Z
url: https://github.com/astral-sh/ruff/pull/16699
synced_at: 2026-01-12T15:55:56Z
```

# [`pygrep-hooks`]: Detect file-level suppressions comments without rule codes (`PGH004`)

---

_@MichaReiser_

## Summary

This PR stabilizes the preview behavior of `PGH004` to also detect blanked file-level noqa comments (and not just line level comments).

This preview behavior was first released with Ruff 0.4.8 (June 2025). There are no open issues or PRs open related to this behavior.




---

_Label `rule` added by @MichaReiser on 2025-03-13 08:26_

---

_Review requested from @dylwil3 by @MichaReiser on 2025-03-13 08:26_

---

_Review requested from @ntBre by @MichaReiser on 2025-03-13 08:26_

---

_Comment by @codspeed-hq[bot] on 2025-03-13 08:31_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fblanked-noqa-file-level)

### Merging #16699 will **degrade performances by 4.6%**

<sub>Comparing <code>micha/blanked-noqa-file-level</code> (c1c0d1b) with <code>micha/ruff-0.10</code> (d622137)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Fblanked-noqa-file-level)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 5.2 ms | 5.5 ms | -4.6% |


---

_Comment by @github-actions[bot] on 2025-03-13 08:32_

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
+ <a href='https://github.com/apache/airflow/blob/5fc18124341a0a0e34c342ca8387bcbdb392d06d/docs/exts/exampleinclude.py#L1'>docs/exts/exampleinclude.py:1:1:</a> PGH004 Use specific rule codes when using `ruff: noqa`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/29b4c40e439997d13aa5fcd883f8fd099d78a1df/tests/integration_tests/superset_test_config_sqllab_backend_persist_off.py#L17'>tests/integration_tests/superset_test_config_sqllab_backend_persist_off.py:17:1:</a> PGH004 Use specific rule codes when using `ruff: noqa`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/d149ab353adcfa67693f692e3ee235888971e744/testing/code/test_source.py#L2'>testing/code/test_source.py:2:1:</a> PGH004 Use specific rule codes when using `ruff: noqa`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pdm-project/pdm">pdm-project/pdm</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pdm-project/pdm/blob/1e36d77b17e4e4eb2d06ab581b3ad6c6cf65420a/src/pdm/installers/__init__.py#L1'>src/pdm/installers/__init__.py:1:1:</a> PGH004 Use specific rule codes when using `ruff: noqa`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/516097d0655104e6155d64afba05333f60d79731/astropy/io/ascii/__init__.py#L3'>astropy/io/ascii/__init__.py:3:1:</a> PGH004 Use specific rule codes when using `ruff: noqa`
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

_Added to milestone `v0.10` by @MichaReiser on 2025-03-13 08:45_

---

_Comment by @MichaReiser on 2025-03-13 08:55_

The ecosystem checks are all true-positives

---

_@dylwil3 approved on 2025-03-13 12:01_

Nice! I do think that we should have a followup issue/PR where we add the feature that this rule has for inline suppressions where it tries to guess if you just forgot a colon

---

_Merged by @MichaReiser on 2025-03-13 12:46_

---

_Closed by @MichaReiser on 2025-03-13 12:46_

---

_Branch deleted on 2025-03-13 12:46_

---

_Branch restored on 2025-03-14 07:24_

---
