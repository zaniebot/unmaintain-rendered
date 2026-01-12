```yaml
number: 3447
title: "Use environment layering for `uv run --with`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/layer
created_at: 2024-05-08T02:03:10Z
updated_at: 2024-05-08T21:24:25Z
url: https://github.com/astral-sh/uv/pull/3447
synced_at: 2026-01-12T16:05:39Z
```

# Use environment layering for `uv run --with`

---

_@charliermarsh_

## Summary

This PR takes a different approach to `--with` for `uv run`. Now, instead of merging the requirements and re-resolving, we have two phases: (1) sync the workspace requirements to the workspace environment; then (2) sync the ephemeral `--with` requirements to an ephemeral environment. The two environments are then layered by setting the `PATH` and `PYTHONPATH` variables appropriately.

I think this approach simplifies a few things:

1. Once we have a lockfile, the semantics are much clearer, and we can actually reuse it for the workspace. If we had to add arbitrary dependencies via `--with`, then it's not really clear how the lockfile would/should behave.
2. Caching becomes simpler, because we can just cache the ephemeral environment based on the requirements.

The current version of this PR loses a few behaviors though that I need to restore:

- `--python` support -- but I'm not yet sure how this is supposed to behave within projects? It's also left unclear in `uv sync` and `uv lock`.
- The "reuse the workspace environment if it already satisfies the ephemeral requirements" behavior.

Closes #3411.


---

_Label `preview` added by @charliermarsh on 2024-05-08 02:03_

---

_Review requested from @zanieb by @charliermarsh on 2024-05-08 02:03_

---

_Review requested from @konstin by @charliermarsh on 2024-05-08 02:03_

---

_Review comment by @zanieb on `crates/uv/src/commands/workspace/run.rs`:74 on 2024-05-08 02:08_

Just throwing this out here to think about... wouldn't it be annoying if we exposed a default environment at `.venv` and every time you used `uv run` any manual changes you had made were undone?

---

_@zanieb reviewed on 2024-05-08 02:08_

---

_@charliermarsh reviewed on 2024-05-08 02:12_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/workspace/run.rs`:74 on 2024-05-08 02:12_

Yeah, though this doesn't uninstall anything, it just makes sure it's in-sync with the (in-the-future) lockfile. Does that seem right?

---

_Review comment by @konstin on `crates/uv/src/commands/workspace/run.rs`:78 on 2024-05-08 09:24_

That's curious, how is that implemented in rust, does this insert a conditional drop?

---

_@konstin approved on 2024-05-08 09:29_

Do we know how resource discovery interacts with `PYTHONPATH`, e.g. for finding jupyter kernels?

---

_@zanieb reviewed on 2024-05-08 13:35_

---

_Review comment by @zanieb on `crates/uv/src/commands/workspace/run.rs`:74 on 2024-05-08 13:35_

Ah we'll need to be careful with "in sync" because sync implies an exact match (no extra packages) to me. Seems ideal not to remove packages unless requested? Like.. `uv sync --strict`?

---

_@charliermarsh reviewed on 2024-05-08 13:37_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/workspace/run.rs`:74 on 2024-05-08 13:37_

This is in uv run, not uv sync. For now, neither one is removing packages, but I don’t feel strongly about the behavior.

---

_@zanieb reviewed on 2024-05-08 13:39_

---

_Review comment by @zanieb on `crates/uv/src/commands/workspace/run.rs`:74 on 2024-05-08 13:39_

I understand that, I'm talking more generally.

`uv run` should not probably not remove extraneous packages (and I understand that is the current behavior).

---

_@zanieb approved on 2024-05-08 20:53_

---

_Merged by @charliermarsh on 2024-05-08 21:24_

---

_Closed by @charliermarsh on 2024-05-08 21:24_

---

_Branch deleted on 2024-05-08 21:24_

---
