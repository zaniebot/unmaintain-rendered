```yaml
number: 9776
title: "[flake8-bugbear][B006] remove outdated comment"
type: pull_request
state: merged
author: mikaelarguedas
labels:
  - documentation
assignees: []
merged: true
base: main
head: b006_fix_application_order
created_at: 2024-02-02T07:38:22Z
updated_at: 2024-02-02T14:32:53Z
url: https://github.com/astral-sh/ruff/pull/9776
synced_at: 2026-01-10T22:57:09Z
```

# [flake8-bugbear][B006] remove outdated comment

---

_Pull request opened by @mikaelarguedas on 2024-02-02 07:38_

I noticed that the comment doesn't match the behavior:
- zip function is not used anymore
- parameters are not scanned in reverse

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

No need


---

_Comment by @github-actions[bot] on 2024-02-02 07:52_

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

_@charliermarsh approved on 2024-02-02 14:32_

---

_Label `documentation` added by @charliermarsh on 2024-02-02 14:32_

---

_Merged by @charliermarsh on 2024-02-02 14:32_

---

_Closed by @charliermarsh on 2024-02-02 14:32_

---

_Comment by @charliermarsh on 2024-02-02 14:32_

Thanks :)

---
