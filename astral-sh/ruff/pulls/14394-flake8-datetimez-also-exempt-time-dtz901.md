```yaml
number: 14394
title: "[`flake8-datetimez`] Also exempt `.time()` (`DTZ901`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
assignees: []
merged: true
base: main
head: DTZ901
created_at: 2024-11-17T01:26:32Z
updated_at: 2024-11-18T02:32:33Z
url: https://github.com/astral-sh/ruff/pull/14394
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-datetimez`] Also exempt `.time()` (`DTZ901`)

---

_Pull request opened by @InSyncWithFoo on 2024-11-17 01:26_

## Summary

Resolves #14378.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-11-17 01:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/daylight.py#L83'>examples/models/daylight.py:83:12:</a> DTZ901 Use of `datetime.datetime.min` without timezone information
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DTZ901 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_@charliermarsh approved on 2024-11-18 02:18_

---

_Label `bug` added by @charliermarsh on 2024-11-18 02:18_

---

_Merged by @charliermarsh on 2024-11-18 02:24_

---

_Closed by @charliermarsh on 2024-11-18 02:24_

---

_Branch deleted on 2024-11-18 02:24_

---
