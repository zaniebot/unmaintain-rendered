```yaml
number: 9495
title: Limit inplace diagnostics to methods that accept inplace
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pd
created_at: 2024-01-12T18:36:45Z
updated_at: 2024-01-12T19:12:55Z
url: https://github.com/astral-sh/ruff/pull/9495
synced_at: 2026-01-12T15:55:29Z
```

# Limit inplace diagnostics to methods that accept inplace

---

_@charliermarsh_

## Summary

This should reduce false positives like https://github.com/astral-sh/ruff/issues/9491, by ignoring methods that are clearly not on a DataFrame.

Closes https://github.com/astral-sh/ruff/issues/9491.


---

_Label `bug` added by @charliermarsh on 2024-01-12 18:36_

---

_Comment by @codspeed-hq[bot] on 2024-01-12 18:43_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/pd)

### Merging #9495 will **improve performances by 4.55%**

<sub>Comparing <code>charlie/pd</code> (7d9728b) with <code>main</code> (d16c4a2)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/pd` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/ctypeslib.py]` | 12.8 ms | 12.2 ms | +4.55% |


---

_Comment by @github-actions[bot] on 2024-01-12 18:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/serialization.py#L666'>src/bokeh/core/serialization.py:666:34:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD002 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/serialization.py#L666'>src/bokeh/core/serialization.py:666:34:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD002 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-01-12 19:12_

---

_Closed by @charliermarsh on 2024-01-12 19:12_

---

_Branch deleted on 2024-01-12 19:12_

---
