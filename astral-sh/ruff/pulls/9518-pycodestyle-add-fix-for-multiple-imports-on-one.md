```yaml
number: 9518
title: "[`pycodestyle`] Add fix for `multiple-imports-on-one-line` (`E401`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: add-autofix-for-E401
created_at: 2024-01-15T01:52:12Z
updated_at: 2024-01-21T20:33:39Z
url: https://github.com/astral-sh/ruff/pull/9518
synced_at: 2026-01-12T15:55:29Z
```

# [`pycodestyle`] Add fix for `multiple-imports-on-one-line` (`E401`)

---

_@diceroll123_

## Summary

Add autofix for `multiple_imports_on_one_line`, `E401`

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-01-15 02:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +2 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/setup.py#L10'>setup.py:10:1:</a> E401 Multiple imports on one line
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/setup.py#L10'>setup.py:10:1:</a> E401 [*] Multiple imports on one line
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E401 | 2 | 0 | 0 | 2 | 0 |

</p>
</details>




---

_@charliermarsh reviewed on 2024-01-15 05:06_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/imports.rs`:125 on 2024-01-15 05:06_

Can you add test cases for, e.g.:

```python
x = 1; import re as regex, string

import re as regex, string; x = 1
```

---

_Comment by @codspeed-hq[bot] on 2024-01-15 05:20_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/diceroll123:add-autofix-for-E401)

### Merging #9518 will **not alter performance**

<sub>Comparing <code>diceroll123:add-autofix-for-E401</code> (92df930) with <code>main</code> (b64aa1e)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Review requested from @charliermarsh by @charliermarsh on 2024-01-21 16:01_

---

_Label `autofix` added by @charliermarsh on 2024-01-21 16:01_

---

_Label `preview` added by @charliermarsh on 2024-01-21 16:01_

---

_Renamed from "[`pycodestyle`] - add fix for `multiple_imports_on_one_line` (`E401`)" to "[`pycodestyle`] Add fix for `multiple-imports-on-one-line` (`E401`)" by @charliermarsh on 2024-01-21 16:01_

---

_@charliermarsh approved on 2024-01-21 19:45_

Thanks! I had to apply some changes to fix cases like: `if True: import re as regex, string`

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/multiple_imports_on_one_line.rs`:92 on 2024-01-21 19:46_

@diceroll123 - I decided to just `; `-join these for now, since it's hard to know if we're _allowed_ to split them.

---

_@charliermarsh reviewed on 2024-01-21 19:46_

---

_Merged by @charliermarsh on 2024-01-21 20:33_

---

_Closed by @charliermarsh on 2024-01-21 20:33_

---
