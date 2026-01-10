```yaml
number: 3436
title: "Add basic `uv sync` and `uv lock` commands"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/sync
created_at: 2024-05-07T19:16:12Z
updated_at: 2024-05-08T14:51:53Z
url: https://github.com/astral-sh/uv/pull/3436
synced_at: 2026-01-10T14:37:54Z
```

# Add basic `uv sync` and `uv lock` commands

---

_Pull request opened by @charliermarsh on 2024-05-07 19:16_

## Summary

These aren't intended for production use; instead, I'm just trying to frame out the overall data flows and code-sharing for these commands. We now have `uv sync` (sync the environment to match the lockfile, without refreshing or resolving) and `uv lock` (generate the lockfile). Both _require_ a virtual environment to exist (something we should change). `uv sync`, `uv run`, and `uv lock` all share code for the underlying subroutines (resolution and installation), so the commands themselves are relatively small (~100 lines) and mostly consist of reading arguments and such.

`uv lock` and `uv sync` don't actually really work yet, because we have no way to include the project itself in the lockfile (that's a TODO in the lockfile implementation).

Closes https://github.com/astral-sh/uv/issues/3432.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-07 19:16_

---

_Review requested from @zanieb by @charliermarsh on 2024-05-07 19:16_

---

_Review requested from @konstin by @charliermarsh on 2024-05-07 19:16_

---

_@charliermarsh reviewed on 2024-05-07 19:17_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/workspace/run.rs`:268 on 2024-05-07 19:17_

We could DRY this up but not sure if it's worth it.

---

_@charliermarsh reviewed on 2024-05-07 19:17_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/workspace/run.rs`:31 on 2024-05-07 19:17_

This is just the existing `run.rs`, but moved into the `workspace` module, and the `resolve` and `install` commands were pulled into standalone methods.

---

_Review comment by @konstin on `crates/uv/tests/pip_sync.rs`:79 on 2024-05-08 09:14_

That should move to the pip_install tests

---

_@konstin approved on 2024-05-08 09:17_

---

_@BurntSushi approved on 2024-05-08 13:38_

---

_Merged by @charliermarsh on 2024-05-08 14:51_

---

_Closed by @charliermarsh on 2024-05-08 14:51_

---

_Branch deleted on 2024-05-08 14:51_

---
