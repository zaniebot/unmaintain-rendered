```yaml
number: 16649
title: "[`ruff`] Stabilize `unnecessary-cast-to-int` (`RUF046`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/ruf046-0.10
created_at: 2025-03-11T21:56:41Z
updated_at: 2025-03-12T12:14:11Z
url: https://github.com/astral-sh/ruff/pull/16649
synced_at: 2026-01-12T15:55:55Z
```

# [`ruff`] Stabilize `unnecessary-cast-to-int` (`RUF046`)

---

_@ntBre_

Summary
--

Stabilizes RUF046 and moves its test to the right place. The docs look good.

Test Plan
--

2 closed newline/whitespace issues from early January and 1 closed issue about really being multiple rules, but otherwise no recent issues or PRs.

---

_Label `rule` added by @ntBre on 2025-03-11 21:56_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-11 21:56_

---

_Comment by @codspeed-hq[bot] on 2025-03-11 22:01_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fruf046-0.10)

### Merging #16649 will **degrade performances by 10.81%**

<sub>Comparing <code>brent/ruf046-0.10</code> (149591e) with <code>micha/ruff-0.10</code> (b8f1284)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Fruf046-0.10)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.9 ms | 5.5 ms | -10.81% |


---

_Comment by @github-actions[bot] on 2025-03-11 22:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+16 -0 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/training_data/loading.py#L86'>rasa/shared/core/training_data/loading.py:86:15:</a> RUF046 Value being cast to `int` is already an integer
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/8e021b0c8221f8ab22b2a40289c1c96ecbd5a1b2/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L197'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:197:40:</a> RUF046 Value being cast to `int` is already an integer
+ <a href='https://github.com/apache/superset/blob/8e021b0c8221f8ab22b2a40289c1c96ecbd5a1b2/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L199'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:199:29:</a> RUF046 Value being cast to `int` is already an integer
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/crazy_functions/pdf_fns/parse_pdf.py#L117'>crazy_functions/pdf_fns/parse_pdf.py:117:21:</a> RUF046 Value being cast to `int` is already an integer
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/main.py#L107'>examples/server/app/spectrogram/main.py:107:13:</a> RUF046 Value being cast to `int` is already an integer
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L338'>src/bokeh/colors/color.py:338:60:</a> RUF046 Value being cast to `int` is already an integer
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1530'>src/bokeh/palettes.py:1530:27:</a> RUF046 Value being cast to `int` is already an integer
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1568'>src/bokeh/palettes.py:1568:10:</a> RUF046 Value being cast to `int` is already an integer
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1569'>src/bokeh/palettes.py:1569:10:</a> RUF046 Value being cast to `int` is already an integer
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/40a81803e707a053af2612e67a7752fe6f6fa1b4/pandas/tests/dtypes/cast/test_maybe_box_native.py#L20'>pandas/tests/dtypes/cast/test_maybe_box_native.py:20:10:</a> RUF046 [*] Value being cast to `int` is already an integer
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/f3e1013293a9a51d935a570b42ab33741d2611ab/astropy/io/fits/hdu/compressed/tests/test_compressed.py#L375'>astropy/io/fits/hdu/compressed/tests/test_compressed.py:375:26:</a> RUF046 Value being cast to `int` is already an integer
+ <a href='https://github.com/astropy/astropy/blob/f3e1013293a9a51d935a570b42ab33741d2611ab/astropy/io/fits/hdu/compressed/utils.py#L102'>astropy/io/fits/hdu/compressed/utils.py:102:13:</a> RUF046 Value being cast to `int` is already an integer
+ <a href='https://github.com/astropy/astropy/blob/f3e1013293a9a51d935a570b42ab33741d2611ab/astropy/io/fits/tests/test_image.py#L1010'>astropy/io/fits/tests/test_image.py:1010:26:</a> RUF046 Value being cast to `int` is already an integer
+ <a href='https://github.com/astropy/astropy/blob/f3e1013293a9a51d935a570b42ab33741d2611ab/astropy/io/votable/validator/html.py#L246'>astropy/io/votable/validator/html.py:246:14:</a> RUF046 Value being cast to `int` is already an integer
+ <a href='https://github.com/astropy/astropy/blob/f3e1013293a9a51d935a570b42ab33741d2611ab/astropy/modeling/_fitting_parallel.py#L194'>astropy/modeling/_fitting_parallel.py:194:22:</a> RUF046 Value being cast to `int` is already an integer
+ <a href='https://github.com/astropy/astropy/blob/f3e1013293a9a51d935a570b42ab33741d2611ab/astropy/utils/console.py#L365'>astropy/utils/console.py:365:21:</a> RUF046 Value being cast to `int` is already an integer
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF046 | 16 | 16 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-03-11 22:33_

These all look like true positives to me.

---

_@MichaReiser approved on 2025-03-12 07:36_

---

_Merged by @ntBre on 2025-03-12 12:14_

---

_Closed by @ntBre on 2025-03-12 12:14_

---

_Branch deleted on 2025-03-12 12:14_

---
