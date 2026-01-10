```yaml
number: 13388
title: Respect word boundaries when detecting function signature in docs
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: charlie/b
created_at: 2024-09-18T03:50:27Z
updated_at: 2024-09-18T04:06:18Z
url: https://github.com/astral-sh/ruff/pull/13388
synced_at: 2026-01-10T21:30:32Z
```

# Respect word boundaries when detecting function signature in docs

---

_Pull request opened by @charliermarsh on 2024-09-18 03:50_

## Summary

Closes https://github.com/astral-sh/ruff/issues/13242.


---

_Label `bug` added by @charliermarsh on 2024-09-18 03:50_

---

_Label `docstring` added by @charliermarsh on 2024-09-18 03:50_

---

_Merged by @charliermarsh on 2024-09-18 04:01_

---

_Closed by @charliermarsh on 2024-09-18 04:01_

---

_Branch deleted on 2024-09-18 04:01_

---

_Comment by @github-actions[bot] on 2024-09-18 04:06_

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
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/logconfig.py#L60'>src/bokeh/util/logconfig.py:60:5:</a> D402 First line should not be the function's signature
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D402 | 1 | 0 | 1 | 0 | 0 |

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
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/logconfig.py#L60'>src/bokeh/util/logconfig.py:60:5:</a> D402 First line should not be the function's signature
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D402 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---
