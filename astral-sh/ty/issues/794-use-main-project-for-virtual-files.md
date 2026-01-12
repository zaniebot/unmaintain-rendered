```yaml
number: 794
title: Use main project for virtual files
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-07-09T20:23:30Z
updated_at: 2025-07-15T06:57:21Z
url: https://github.com/astral-sh/ty/issues/794
synced_at: 2026-01-12T15:54:24Z
```

# Use main project for virtual files

---

_@MichaReiser_

We currently always use the default project for virtual files. That's unlikely what the user expects. Instead, we should use the primary workspace and project. How we define primary is up for debate: the easiest is if there's a single workspace with a single project, we should then use those. It's less clear what the right heuristics are if there are multiple workspaces or projects. We can either use the default in those cases or always use the outer most project and workspace

---

_Label `server` added by @MichaReiser on 2025-07-09 20:23_

---

_Comment by @dhruvmanila on 2025-07-15 06:29_

We now add all virtual files to the primary workspace and project but currently we only support a single workspace containing a single project.

These are the TODOs that needs to be resolved to close this issue: https://github.com/astral-sh/ruff/blob/93a44b3b2b1f97b8532adf41cdd9dd82ee9a65bb/crates/ty_server/src/session.rs#L166-L207

This can only be done once multiple workspaces and projects are supported in the server.

---

_Closed by @MichaReiser on 2025-07-15 06:57_

---
