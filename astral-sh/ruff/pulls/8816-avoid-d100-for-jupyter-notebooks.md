```yaml
number: 8816
title: "Avoid `D100` for Jupyter Notebooks"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
assignees: []
merged: true
base: main
head: dhruv/notebook-d100
created_at: 2023-11-21T22:24:59Z
updated_at: 2023-11-22T15:32:51Z
url: https://github.com/astral-sh/ruff/pull/8816
synced_at: 2026-01-10T23:40:55Z
```

# Avoid `D100` for Jupyter Notebooks

---

_Pull request opened by @dhruvmanila on 2023-11-21 22:24_

This PR avoids triggering `D100` for Jupyter Notebooks.

Part of #8669


---

_Comment by @dhruvmanila on 2023-11-21 22:25_

Current dependencies on/for this PR:
* main
  * **PR #8816** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8816?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8816?utm_source=stack-comment).

---

_Label `rule` added by @dhruvmanila on 2023-11-21 22:25_

---

_Comment by @github-actions[bot] on 2023-11-21 22:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2023-11-21 23:33_

Should we add a test for this? I think I added a Jupyter test in the past, if you grep for `F821_22.ipynb`.

---

_Merged by @dhruvmanila on 2023-11-22 15:26_

---

_Closed by @dhruvmanila on 2023-11-22 15:26_

---

_Branch deleted on 2023-11-22 15:26_

---
