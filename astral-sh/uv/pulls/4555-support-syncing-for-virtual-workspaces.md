```yaml
number: 4555
title: Support syncing for virtual workspaces
type: pull_request
state: closed
author: charliermarsh
labels:
  - preview
assignees: []
base: charlie/sync
head: charlie/virtual
created_at: 2024-06-26T17:01:10Z
updated_at: 2024-07-29T14:10:19Z
url: https://github.com/astral-sh/uv/pull/4555
synced_at: 2026-01-12T16:06:18Z
```

# Support syncing for virtual workspaces

---

_@charliermarsh_

## Summary

Right now, `uv sync` and `uv run` fail when run from a virtual workspace. This PR adds support by syncing the entire workspace environment.

Closes https://github.com/astral-sh/uv/issues/4541.


---

_Comment by @charliermarsh on 2024-06-26 17:01_

Does this even make sense? A virtual workspace doesn't have any "dependencies". So what does it mean to run a command in a virtual workspace?


---

_Label `preview` added by @charliermarsh on 2024-06-26 17:01_

---

_Comment by @charliermarsh on 2024-06-26 17:14_

I guess that's the crucial question: should you be able to `uv run` in a virtual workspace. The virtual workspace has no package, so it has no dependencies. It's not represented in the lockfile at all. So how would we sync it, if not by syncing the entire workspace?

---

_Comment by @mjclarke94 on 2024-06-26 23:39_

Could the run command make sense in the context of integration/cross-package tests?

Say you had a virtual workspace containing several namespace packages with their own isolated unit test suites. You could come up with a set of tests which only make sense in the context of having several of these packages installed at once.

---

_Comment by @konstin on 2024-06-27 08:26_

The original design for workspaces to be centered around `uv run`, so that when you run an entrypoint from a package or a command in a package you would get only the dependencies from that package. My preferred way of handling this would be:
* Add `uv sync --package` (work like `--extra`, also `--all-packages`), that works like `uv run --package`
* Add a `tool.uv.default-packages = ["...", "..."]` setting (https://github.com/astral-sh/uv/issues/3625) for virtual workspaces that sets the default packages to install when running `uv sync` or `uv run`

---

_Comment by @charliermarsh on 2024-07-29 14:10_

This got solved separately.

---

_Closed by @charliermarsh on 2024-07-29 14:10_

---
