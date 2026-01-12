```yaml
number: 11735
title: "Update `NPY001` rule for NumPy 2.0"
type: pull_request
state: merged
author: mtsokol
labels:
  - rule
assignees: []
merged: true
base: main
head: update-npy001
created_at: 2024-06-04T09:33:20Z
updated_at: 2024-06-04T19:33:41Z
url: https://github.com/astral-sh/ruff/pull/11735
synced_at: 2026-01-12T15:55:38Z
```

# Update `NPY001` rule for NumPy 2.0

---

_@mtsokol_

Hi!

This PR addresses https://github.com/astral-sh/ruff/issues/11093.

It skips `np.bool` and `np.long` replacements as both of these names were reintroduced in NumPy 2.0 with a different meaning (https://github.com/numpy/numpy/pull/24922, https://github.com/numpy/numpy/pull/25080).
With this change `NPY001` will no longer conflict with `NPY201`. For projects using NumPy 1.x `np.bool` and `np.long` has been deprecated and removed long time ago, and accessing them yields an informative error message. 

---

_Comment by @github-actions[bot] on 2024-06-04 09:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/selection_histogram.py#L88'>examples/server/app/selection_histogram.py:88:42:</a> NPY001 [*] Type alias `np.bool` is deprecated, replace with builtin type
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| NPY001 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/selection_histogram.py#L88'>examples/server/app/selection_histogram.py:88:42:</a> NPY001 [*] Type alias `np.bool` is deprecated, replace with builtin type
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| NPY001 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-04 15:43_

---

_@charliermarsh approved on 2024-06-04 19:20_

Thanks!

---

_Label `rule` added by @charliermarsh on 2024-06-04 19:20_

---

_Merged by @charliermarsh on 2024-06-04 19:23_

---

_Closed by @charliermarsh on 2024-06-04 19:23_

---

_Branch deleted on 2024-06-04 19:24_

---

_Comment by @codspeed-hq[bot] on 2024-06-04 19:24_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtsokol:update-npy001)

### Merging #11735 will **degrade performances by 7.28%**

<sub>Comparing <code>mtsokol:update-npy001</code> (aaf4983) with <code>mtsokol:update-npy001</code> (16738d5)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/mtsokol:update-npy001)._

### Benchmarks breakdown

|     | Benchmark | `mtsokol:update-npy001` | `mtsokol:update-npy001` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-with-preview-rules[numpy/globals.py]` | 799.5 µs | 862.2 µs | -7.28% |


---
