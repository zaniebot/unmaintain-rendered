```yaml
number: 11456
title: Offset syntax error for each cell
type: pull_request
state: closed
author: dhruvmanila
labels:
  - bug
assignees: []
draft: true
base: main
head: dhruv/syntax-error-offset
created_at: 2024-05-17T08:08:15Z
updated_at: 2025-03-04T15:57:27Z
url: https://github.com/astral-sh/ruff/pull/11456
synced_at: 2026-01-12T15:55:38Z
```

# Offset syntax error for each cell

---

_@dhruvmanila_

## Summary

This PR fixes the syntax error location for the `ruff format` command.

fixes: #11453

## Test Plan

<!-- How was it tested? -->


---

_Comment by @dhruvmanila on 2024-05-17 08:08_

I don't think this is an ideal solution, I might look at it in more detail later.

---

_Comment by @github-actions[bot] on 2024-05-17 08:21_

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

_Label `bug` added by @dhruvmanila on 2025-01-20 15:02_

---

_Comment by @dhruvmanila on 2025-03-04 15:57_

Closing in favor of https://github.com/astral-sh/ruff/pull/16499

---

_Closed by @dhruvmanila on 2025-03-04 15:57_

---
