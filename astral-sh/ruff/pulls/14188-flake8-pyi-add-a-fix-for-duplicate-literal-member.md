```yaml
number: 14188
title: "[`flake8-pyi`] Add a fix for `duplicate-literal-member`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
assignees: []
merged: true
base: main
head: charlie/dupe
created_at: 2024-11-08T03:35:13Z
updated_at: 2024-11-08T03:54:29Z
url: https://github.com/astral-sh/ruff/pull/14188
synced_at: 2026-01-12T15:55:47Z
```

# [`flake8-pyi`] Add a fix for `duplicate-literal-member`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ruff/issues/14187.


---

_Review requested from @AlexWaygood by @charliermarsh on 2024-11-08 03:35_

---

_Label `fixes` added by @charliermarsh on 2024-11-08 03:35_

---

_Merged by @charliermarsh on 2024-11-08 03:45_

---

_Closed by @charliermarsh on 2024-11-08 03:45_

---

_Branch deleted on 2024-11-08 03:45_

---

_Comment by @github-actions[bot] on 2024-11-08 03:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +2 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L569'>src/bokeh/events.py:569:36:</a> PYI062 Duplicate literal member `-1`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L569'>src/bokeh/events.py:569:36:</a> PYI062 [*] Duplicate literal member `-1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI062 | 2 | 0 | 0 | 2 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +2 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L569'>src/bokeh/events.py:569:36:</a> PYI062 Duplicate literal member `-1`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L569'>src/bokeh/events.py:569:36:</a> PYI062 [*] Duplicate literal member `-1`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI062 | 2 | 0 | 0 | 2 | 0 |

</p>
</details>




---
