```yaml
number: 12368
title: "Avoid dropping extra boolean operations in `repeated-equality-comparison`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/PLR1714
created_at: 2024-07-17T15:19:09Z
updated_at: 2024-07-17T15:49:29Z
url: https://github.com/astral-sh/ruff/pull/12368
synced_at: 2026-01-12T15:55:41Z
```

# Avoid dropping extra boolean operations in `repeated-equality-comparison`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ruff/issues/12062.


---

_Label `bug` added by @charliermarsh on 2024-07-17 15:19_

---

_Comment by @github-actions[bot] on 2024-07-17 15:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_sampledata_xref.py#L180'>src/bokeh/sphinxext/bokeh_sampledata_xref.py:180:16:</a> PLR1714 Consider merging multiple comparisons: `node.subfolder in ('all', sp[-2])`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1714 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokeh_sampledata_xref.py#L180'>src/bokeh/sphinxext/bokeh_sampledata_xref.py:180:16:</a> PLR1714 Consider merging multiple comparisons: `node.subfolder in ('all', sp[-2])`. Use a `set` if the elements are hashable.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1714 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-07-17 15:49_

---

_Closed by @charliermarsh on 2024-07-17 15:49_

---

_Branch deleted on 2024-07-17 15:49_

---
