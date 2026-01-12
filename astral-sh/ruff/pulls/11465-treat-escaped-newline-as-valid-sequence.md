```yaml
number: 11465
title: Treat escaped newline as valid sequence
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/w
created_at: 2024-05-19T03:27:40Z
updated_at: 2024-05-19T03:41:50Z
url: https://github.com/astral-sh/ruff/pull/11465
synced_at: 2026-01-12T15:55:38Z
```

# Treat escaped newline as valid sequence

---

_@charliermarsh_

## Summary

We weren't treating the escaped newline as a valid condition to trigger the safer fix (add an extra backslash before each invalid escape sequence).

Closes https://github.com/astral-sh/ruff/issues/11461.


---

_Label `bug` added by @charliermarsh on 2024-05-19 03:28_

---

_Merged by @charliermarsh on 2024-05-19 03:32_

---

_Closed by @charliermarsh on 2024-05-19 03:32_

---

_Branch deleted on 2024-05-19 03:32_

---

_Comment by @zanieb on 2024-05-19 03:37_

Nice!

---

_Comment by @github-actions[bot] on 2024-05-19 03:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (error)</summary>
<p>

```
Failed to clone commaai/openpilot: fatal: active `post-checkout` hook found during `git clone`:
	/home/runner/work/ruff/ruff/checkouts/commaai:openpilot/.git/hooks/post-checkout
For security reasons, this is disallowed by default.
If this is intentional and the hook should actually be run, please
run the command again with `GIT_CLONE_PROTECTION_ACTIVE=false`
warning: Clone succeeded, but checkout failed.
You can inspect what was checked out with 'git status'
and retry with 'git restore --source=HEAD :/'
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
