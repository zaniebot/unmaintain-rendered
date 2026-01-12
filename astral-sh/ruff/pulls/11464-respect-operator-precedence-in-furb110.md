```yaml
number: 11464
title: "Respect operator precedence in `FURB110`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/furb
created_at: 2024-05-19T03:13:24Z
updated_at: 2024-05-27T09:09:01Z
url: https://github.com/astral-sh/ruff/pull/11464
synced_at: 2026-01-12T15:55:38Z
```

# Respect operator precedence in `FURB110`

---

_@charliermarsh_

## Summary

Ensures that we parenthesize expressions (if necessary) to preserve operator precedence in `FURB110`.

Closes https://github.com/astral-sh/ruff/issues/11398.


---

_Label `bug` added by @charliermarsh on 2024-05-19 03:13_

---

_Merged by @charliermarsh on 2024-05-19 03:17_

---

_Closed by @charliermarsh on 2024-05-19 03:17_

---

_Branch deleted on 2024-05-19 03:17_

---

_Comment by @github-actions[bot] on 2024-05-19 03:26_

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

_@MichaReiser reviewed on 2024-05-27 09:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/if_exp_instead_of_or_operator.rs`:99 on 2024-05-27 09:09_

Not specific to this PR but I think we should build out some infrastructure that given an `Expr` returns a comparable `Precedence`. This would allow updating the precedence once instead of in many places (generator, parser, here, other places?)

---
