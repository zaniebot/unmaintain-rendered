```yaml
number: 4554
title: Sync all packages when syncing workspaces
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/sync
created_at: 2024-06-26T16:52:22Z
updated_at: 2024-09-23T23:31:11Z
url: https://github.com/astral-sh/uv/pull/4554
synced_at: 2026-01-10T12:53:31Z
```

# Sync all packages when syncing workspaces

---

_Pull request opened by @charliermarsh on 2024-06-26 16:52_

## Summary

For consistency, syncing a workspace should involve syncing all members. Perhaps we revisit this in the future -- it may be possible to skip work here when you're operating on a single package.

## Test Plan

Run `cargo run sync` from `./scripts/workspaces/albatross-root-workspace/packages/seeds`. Verify that all packages are installed, rather than just `seeds`.


---

_Label `preview` added by @charliermarsh on 2024-06-26 16:52_

---

_Review requested from @zanieb by @charliermarsh on 2024-06-26 16:52_

---

_Review requested from @konstin by @charliermarsh on 2024-06-26 16:52_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-06-26 16:52_

---

_Comment by @charliermarsh on 2024-06-26 16:57_

It seems like this behavior might be intentional because there are some tests for it. I'll wait for @konstin to review before merging.

---

_Comment by @charliermarsh on 2024-06-26 17:02_

My guess is that the intent with the current behavior is to enforce package isolation... Like, if Package A doesn't depend on Package B, don't install Package B into the environment. Otherwise, Package A will be able to import it, and the dependencies between the projects become meaningless.

---

_Comment by @charliermarsh on 2024-06-26 17:09_

I'll try to sketch out an example of why it might matter...

Assume we have Package A and Package B in the workspace. Package A doesn't depend on Package B.

Package A depends on WerkZeug, and Package B depends on Flask.

We do uv run --package package-b, and it installs Package B and Flask.

We then bump the WerkZeug version in the pyproject.toml.

We do uv run --package package-a, and the lockfile upgrades WerkZeug; upgrading WerkZeug then forces us to upgrade Flask (let's assume).

So now we have a virtual environment with... Package A, Package B, the new WerkZeug, and the old Flask. Package B and its dependencies are out of sync.

---

_Comment by @charliermarsh on 2024-06-26 17:11_

Rye has this behavior: https://rye.astral.sh/guide/workspaces/. It syncs the entire workspace. \cc @mitsuhiko 

---

_Comment by @charliermarsh on 2024-06-26 17:13_

I guess this also makes `uv run --package` redundant, since there's no concept of running a command "in a package" -- it's always running in the full workspace.

---

_Label `preview` removed by @zanieb on 2024-08-20 19:05_

---

_Comment by @charliermarsh on 2024-09-23 23:31_

We opted not to do this.

---

_Closed by @charliermarsh on 2024-09-23 23:31_

---
