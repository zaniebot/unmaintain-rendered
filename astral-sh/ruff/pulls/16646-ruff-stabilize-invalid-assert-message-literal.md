```yaml
number: 16646
title: "[`ruff`] Stabilize `invalid-assert-message-literal-argument` (`RUF040`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/ruf040-0.10
created_at: 2025-03-11T19:47:30Z
updated_at: 2025-03-12T12:11:31Z
url: https://github.com/astral-sh/ruff/pull/16646
synced_at: 2026-01-12T15:55:55Z
```

# [`ruff`] Stabilize `invalid-assert-message-literal-argument` (`RUF040`)

---

_@ntBre_

Summary
--

Stabilizes RUF040 and fixes a very minor typo in the docs. The tests are already in the right place.

Test Plan
--

0 issues or PRs


---

_Label `rule` added by @ntBre on 2025-03-11 19:47_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-11 19:47_

---

_Comment by @codspeed-hq[bot] on 2025-03-11 19:52_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fruf040-0.10)

### Merging #16646 will **degrade performances by 12.73%**

<sub>Comparing <code>brent/ruf040-0.10</code> (d4c46ba) with <code>micha/ruff-0.10</code> (85f7871)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Fruf040-0.10)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.8 ms | 5.5 ms | -12.73% |


---

_Comment by @github-actions[bot] on 2025-03-11 19:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_state.py#L47'>tests/unit/bokeh/io/test_state.py:47:43:</a> RUF040 Non-string literal used as assert message
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/40a81803e707a053af2612e67a7752fe6f6fa1b4/pandas/tests/series/accessors/test_cat_accessor.py#L43'>pandas/tests/series/accessors/test_cat_accessor.py:43:37:</a> RUF040 Non-string literal used as assert message
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/d149ab353adcfa67693f692e3ee235888971e744/testing/_py/test_local.py#L218'>testing/_py/test_local.py:218:26:</a> RUF040 Non-string literal used as assert message
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF040 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-03-11 20:08_

The ecosystem checks look like true positives to me. This might catch some bugs right away.

---

_@MichaReiser approved on 2025-03-12 07:37_

---

_Merged by @ntBre on 2025-03-12 12:11_

---

_Closed by @ntBre on 2025-03-12 12:11_

---

_Branch deleted on 2025-03-12 12:11_

---
