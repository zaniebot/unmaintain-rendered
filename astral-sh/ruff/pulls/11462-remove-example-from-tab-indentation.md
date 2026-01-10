```yaml
number: 11462
title: "Remove example from `tab-indentation`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/w
created_at: 2024-05-17T21:28:08Z
updated_at: 2024-05-17T21:49:17Z
url: https://github.com/astral-sh/ruff/pull/11462
synced_at: 2026-01-10T22:05:26Z
```

# Remove example from `tab-indentation`

---

_Pull request opened by @charliermarsh on 2024-05-17 21:28_

## Summary

I think the example is more confusing than helpful, since there's no visual difference between the tab and space here (even if it rendered properly).

Closes https://github.com/astral-sh/ruff/issues/11460#issuecomment-2118397278.

---

_Label `documentation` added by @charliermarsh on 2024-05-17 21:28_

---

_Comment by @github-actions[bot] on 2024-05-17 21:47_

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

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (error)</summary>
<p>

```
Failed to commit: Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: empty ident name (for <runner@fv-az1200-840.e1fplksw42iexmyiu2m33ibpoe.ex.internal.cloudapp.net>) not allowed
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
Failed to commit: Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: empty ident name (for <runner@fv-az1200-840.e1fplksw42iexmyiu2m33ibpoe.ex.internal.cloudapp.net>) not allowed
```

</p>
</details>




---

_Merged by @charliermarsh on 2024-05-17 21:49_

---

_Closed by @charliermarsh on 2024-05-17 21:49_

---

_Branch deleted on 2024-05-17 21:49_

---
