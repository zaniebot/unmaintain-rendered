```yaml
number: 89
title: "LSP: Support Workspace folders"
type: issue
state: open
author: dhruvmanila
labels:
  - server
assignees: []
created_at: 2024-08-22T10:54:15Z
updated_at: 2026-01-09T03:46:33Z
url: https://github.com/astral-sh/ty/issues/89
synced_at: 2026-01-12T15:54:22Z
```

# LSP: Support Workspace folders

---

_@dhruvmanila_

https://github.com/astral-sh/ruff/pull/13041 restricts the red knot server to only work when there's only one workspace opened in the editor. We should add support for multi-root workspaces.

The challenge here is that which workspace should the untitled / non-workspace files belong to.

Currently, we've made an assumption that any untitled files or non-workspace files are part of the only available workspace. When multi-root workspaces are supported, we need to make sure the user experience is still good and find a way to provide intellisense for non-workspace / untitled files when there are multiple workspaces.

One solution is to implement an ad-hoc database where these files would go but this limits the user experience because we won't be able to resolve the virtual environment for a workspace thus not able to provide any intellisense features for third-party libraries.

The implementation should also consider the fact that workspaces can be added / removed while the server is active. This is supported currently in the Ruff native server and is done via the [`didChangeWorkspaceFolders`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspace_didChangeWorkspaceFolders) notification.

---

_Label `server` added by @dhruvmanila on 2024-08-22 10:54_

---

_Renamed from "Support multi-root workspaces in Red Knot server" to "[red-knot] multi-root workspaces in server" by @carljm on 2025-03-27 19:00_

---

_Renamed from "[red-knot] multi-root workspaces in server" to "multi-root workspaces in server" by @MichaReiser on 2025-05-07 15:17_

---

_Added to milestone `beta` by @MichaReiser on 2025-05-07 15:58_

---

_Comment by @MichaReiser on 2025-06-11 13:55_

I'm not proposing that we should do this but it seems to me that RustRover scans the project for any `Cargo.toml` when it is first opened and then asks the users which projects they want to "attach". 

What I find interesting about this approach (not saying we should open a dialog) is that I believe this approach works for mono repositories (for as long as all projects use the same ty version) and multifolder workspaces. The main downside is that it requires an extra traversal to find all projects

---

_Assigned to @MichaReiser by @dhruvmanila on 2025-07-01 03:02_

---

_Removed from milestone `Beta` by @MichaReiser on 2025-08-15 12:01_

---

_Added to milestone `GA` by @MichaReiser on 2025-08-15 12:01_

---

_Unassigned @MichaReiser by @MichaReiser on 2025-11-14 09:45_

---

_Renamed from "multi-root workspaces in server" to "LSP: Support Workspace folders" by @MichaReiser on 2025-11-14 09:46_

---

_Comment by @MichaReiser on 2025-12-05 12:31_

This is probably a must have if we want to become the default type checker in any VS Code based editor

---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-31 16:37_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:37_

---

_Assigned to @Gankra by @Gankra on 2026-01-09 03:46_

---
