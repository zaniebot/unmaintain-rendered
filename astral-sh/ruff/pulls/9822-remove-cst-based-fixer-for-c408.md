```yaml
number: 9822
title: "Remove CST-based fixer for `C408`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
assignees: []
merged: true
base: main
head: charlie/lcst
created_at: 2024-02-05T02:33:41Z
updated_at: 2024-02-05T03:26:52Z
url: https://github.com/astral-sh/ruff/pull/9822
synced_at: 2026-01-10T22:57:09Z
```

# Remove CST-based fixer for `C408`

---

_Pull request opened by @charliermarsh on 2024-02-05 02:33_

## Summary

We have to keep the fixer for a specific case: `dict` calls that include keyword-argument members.

---

_Label `fixes` added by @charliermarsh on 2024-02-05 02:33_

---

_Marked ready for review by @charliermarsh on 2024-02-05 02:33_

---

_Comment by @github-actions[bot] on 2024-02-05 02:47_

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

_Comment by @charliermarsh on 2024-02-05 03:26_

Last one for now.

---

_Merged by @charliermarsh on 2024-02-05 03:26_

---

_Closed by @charliermarsh on 2024-02-05 03:26_

---

_Branch deleted on 2024-02-05 03:26_

---
