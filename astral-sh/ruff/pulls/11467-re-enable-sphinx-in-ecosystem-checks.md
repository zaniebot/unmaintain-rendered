```yaml
number: 11467
title: Re-enable Sphinx in ecosystem checks
type: pull_request
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
base: main
head: charlie/sphinx
created_at: 2024-05-19T03:31:24Z
updated_at: 2024-05-19T03:50:18Z
url: https://github.com/astral-sh/ruff/pull/11467
synced_at: 2026-01-10T22:05:26Z
```

# Re-enable Sphinx in ecosystem checks

---

_Pull request opened by @charliermarsh on 2024-05-19 03:31_

Just curious if this works now.

---

_Label `internal` added by @charliermarsh on 2024-05-19 03:31_

---

_@zanieb approved on 2024-05-19 03:36_

---

_Comment by @charliermarsh on 2024-05-19 03:44_

Looks like this is still an issue.

---

_Closed by @charliermarsh on 2024-05-19 03:44_

---

_Comment by @github-actions[bot] on 2024-05-19 03:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 2 project errors)

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
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rule `PLC1901` without the `--preview` flag is not allowed.
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 2 project errors)

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

fatal: empty ident name (for <runner@fv-az1149-422.p0yfcspwqgdenibuqhnf5ysfnc.dx.internal.cloudapp.net>) not allowed
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff format --no-preview --exclude tests/roots/test-pycode/cp_1251_coded.py</pre>
</p>
<p>

```
ruff failed
  Cause: Selection of unstable rule `PLC1901` without the `--preview` flag is not allowed.
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

fatal: empty ident name (for <runner@fv-az1149-422.p0yfcspwqgdenibuqhnf5ysfnc.dx.internal.cloudapp.net>) not allowed
```

</p>
</details>




---
