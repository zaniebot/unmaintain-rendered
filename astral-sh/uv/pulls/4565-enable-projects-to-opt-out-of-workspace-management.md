```yaml
number: 4565
title: Enable projects to opt-out of workspace management
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/workspace-opt-out
created_at: 2024-06-26T20:08:43Z
updated_at: 2024-07-01T20:17:44Z
url: https://github.com/astral-sh/uv/pull/4565
synced_at: 2026-01-10T13:48:28Z
```

# Enable projects to opt-out of workspace management

---

_Pull request opened by @charliermarsh on 2024-06-26 20:08_

## Summary

You can now add `managed = false` under `[tool.uv]` in a `pyproject.toml` to explicitly opt out of the project and workspace APIs.

If a project sets `managed = false`, we will (1) _not_ discover it as a workspace root, and (2) _not_ discover it as a workspace member (similar to using `exclude` in the workspace parent).

Closes https://github.com/astral-sh/uv/issues/4551.


---

_Review requested from @zanieb by @charliermarsh on 2024-06-26 20:08_

---

_Review requested from @konstin by @charliermarsh on 2024-06-26 20:08_

---

_Comment by @charliermarsh on 2024-06-26 20:08_

Adding tests now.

---

_Label `preview` added by @charliermarsh on 2024-06-26 20:08_

---

_Comment by @charliermarsh on 2024-06-26 20:31_

> The latter behavior is somewhat debatable since it creates two mechanisms for excluding packages from workspaces, and it precludes us from supporting: (1) workspace at `parent`, (2) project at `child` that is an implicit workspace but isn't part of `parent`.

I think I spoke too soon. That can still be accomplished by adding `child` to the `exclude` list in `parent`.

So `workspace = false` is stronger than `exclude`: it means the project is opted out of being a workspace or part of a workspace.

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:306 on 2024-06-26 21:57_

Does this mean `anyio` won't be available unless a virtual environment is explicitly created? Maybe we should be printing the path to the sys executable or importing anyio here instead of printing the Python version so the snapshot demonstrates more behavior.

---

_@zanieb reviewed on 2024-06-26 21:57_

---

_@zanieb approved on 2024-06-26 21:58_

---

_Comment by @konstin on 2024-06-27 07:35_

What problem is that solving? How are other ecosystems handling this?

---

_Comment by @zanieb on 2024-06-27 11:05_

@konstin see discussion at https://github.com/astral-sh/uv/issues/3836

---

_Comment by @charliermarsh on 2024-06-28 21:06_

@konstin -- It's not an _ecosystem_ issue, it's a `uv` issue: because we support two different APIs (project vs. pip), but we want `uv run` to useable by folks outside of projects, we need some way to indicate that a package is explicitly _not_ managed by uv.

---

_Comment by @konstin on 2024-07-01 07:27_

I see, what about calling it `managed = false` instead? It is workspace discovery in our implementation, but for a user it's just a single project they don't want uv to touch; the workspace part is an implementation detail we should not leak, while `managed = true` indicated that uv will do less (invasive) things. 

---

_Comment by @charliermarsh on 2024-07-01 13:56_

Not strongly opposed, but what would it mean if `managed = false` was set alongside a `tool.uv.workspace` definition in the same `pyproject.toml`?

---

_Comment by @konstin on 2024-07-01 14:20_

Unless you have a specific use case in mind, i think we should warn as with other conflicting options

---

_Comment by @charliermarsh on 2024-07-01 19:35_

Alright, I'm ok with it.

---

_@konstin approved on 2024-07-01 20:13_

---

_Merged by @charliermarsh on 2024-07-01 20:17_

---

_Closed by @charliermarsh on 2024-07-01 20:17_

---

_Branch deleted on 2024-07-01 20:17_

---
