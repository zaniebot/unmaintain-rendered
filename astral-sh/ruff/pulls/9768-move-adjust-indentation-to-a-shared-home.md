```yaml
number: 9768
title: "Move `adjust_indentation` to a shared home"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/h
created_at: 2024-02-02T00:48:29Z
updated_at: 2024-02-02T01:02:44Z
url: https://github.com/astral-sh/ruff/pull/9768
synced_at: 2026-01-10T22:57:09Z
```

# Move `adjust_indentation` to a shared home

---

_Pull request opened by @charliermarsh on 2024-02-02 00:48_

Now that this method is used in multiple linters, it should be moved out of the `pyupgrade` module.

---

_Label `internal` added by @charliermarsh on 2024-02-02 00:48_

---

_Merged by @charliermarsh on 2024-02-02 00:53_

---

_Closed by @charliermarsh on 2024-02-02 00:53_

---

_Branch deleted on 2024-02-02 00:54_

---

_Comment by @github-actions[bot] on 2024-02-02 01:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
