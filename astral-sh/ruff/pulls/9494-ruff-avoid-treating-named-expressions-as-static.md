```yaml
number: 9494
title: "[`ruff`] Avoid treating named expressions as static keys (`RUF011`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/static
created_at: 2024-01-12T18:20:26Z
updated_at: 2024-01-12T19:33:46Z
url: https://github.com/astral-sh/ruff/pull/9494
synced_at: 2026-01-10T22:57:09Z
```

# [`ruff`] Avoid treating named expressions as static keys (`RUF011`)

---

_Pull request opened by @charliermarsh on 2024-01-12 18:20_

Closes https://github.com/astral-sh/ruff/issues/9487.

---

_Label `bug` added by @charliermarsh on 2024-01-12 18:20_

---

_Marked ready for review by @charliermarsh on 2024-01-12 18:20_

---

_Comment by @codspeed-hq[bot] on 2024-01-12 18:27_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/static)

### Merging #9494 will **degrade performances by 4.35%**

<sub>Comparing <code>charlie/static</code> (439da72) with <code>main</code> (fee64b5)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/static)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/static` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.35% |


---

_Comment by @github-actions[bot] on 2024-01-12 18:33_

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

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Converted to draft by @charliermarsh on 2024-01-12 18:35_

---

_Marked ready for review by @charliermarsh on 2024-01-12 18:46_

---

_Merged by @charliermarsh on 2024-01-12 19:33_

---

_Closed by @charliermarsh on 2024-01-12 19:33_

---

_Branch deleted on 2024-01-12 19:33_

---
