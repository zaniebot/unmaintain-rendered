```yaml
number: 9300
title: "Respect `unused-noqa` via `per-file-ignores`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/generic
created_at: 2023-12-28T14:05:26Z
updated_at: 2023-12-28T14:22:41Z
url: https://github.com/astral-sh/ruff/pull/9300
synced_at: 2026-01-12T15:55:28Z
```

# Respect `unused-noqa` via `per-file-ignores`

---

_@charliermarsh_

## Summary

If `RUF100` is ignored via `per-file-ignores`, we need to avoid raising it. `RUF100` has special "self-ignore" logic, since the rule itself deals with `# noqa` directives. This PR wires up `per-file-ignores` to that "self-ignore" logic.

Closes https://github.com/astral-sh/ruff/issues/9297.


---

_Label `bug` added by @charliermarsh on 2023-12-28 14:05_

---

_Merged by @charliermarsh on 2023-12-28 14:15_

---

_Closed by @charliermarsh on 2023-12-28 14:15_

---

_Branch deleted on 2023-12-28 14:15_

---

_Comment by @github-actions[bot] on 2023-12-28 14:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: 'quote-style = preserve' is a preview only feature. Run with '--preview' to enable it.
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
