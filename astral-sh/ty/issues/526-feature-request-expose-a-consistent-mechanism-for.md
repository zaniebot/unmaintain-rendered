```yaml
number: 526
title: "feature request: expose a consistent mechanism for python environment discovery"
type: issue
state: closed
author: strangemonad
labels: []
assignees: []
created_at: 2025-05-27T23:44:29Z
updated_at: 2025-05-28T02:11:34Z
url: https://github.com/astral-sh/ty/issues/526
synced_at: 2026-01-12T15:54:23Z
```

# feature request: expose a consistent mechanism for python environment discovery

---

_@strangemonad_

Sorry for the cross project issue spam but I'd like to attempt to plug https://github.com/astral-sh/uv/issues/2813 again. 

At a high level, I think python venv and providing py-project context continues to be a large pain point for python dev. To be overly simplistic, the behavior I'd like is, given a path (file or dir) provide the best python project context and venv in a way that ty, uv, ruff and editors can all use consistent settings.

Currently, ty has a python path resolution scheme, uv needs to manage a venv and possible python downloads. Editors like vscode and zed rely on Python Environment Tool (PET) https://github.com/microsoft/python-environment-tools to list global python installs and other venvs if workspaces are also queried. Editors like zed further augment and wrap this python toolchain information https://github.com/zed-industries/zed/blob/506beafe1080aa1394be72d9c5794335b034213f/crates/languages/src/python.rs#L515

In practice, keeping all the various configs in sync means that things rarely "just work" ie it requires manual effort and care to configure the editor LSP to show the same errors as the type checker or linter invoked on the command like. Likewise, editor task definitions and terminal venv activation are often disconnected from the logic of `uv run ...`


---

_Comment by @zanieb on 2025-05-27 23:49_

I don't think this is in scope for ty, it will leverage uv for advanced Python discovery capabilities.

That said, it means uv will be getting more machine readable Python discovery output as you desire :)

---

_Closed by @zanieb on 2025-05-27 23:49_

---

_Comment by @zanieb on 2025-05-27 23:50_

(I don't mean to discourage you, but I want to close this to keep the issue tracker actionable and I think the general desire here is captured by the uv issue already)

---

_Comment by @strangemonad on 2025-05-28 02:11_

yep, fair to close :)  I had just noticed that ty had both documented behavior for and implemented some python path and module resolution so I figured I'd make sure this was still on the radar.

---
