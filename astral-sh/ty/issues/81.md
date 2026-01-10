```yaml
number: 81
title: Workspace diagnostics
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-03-14T10:28:54Z
updated_at: 2025-07-15T05:55:57Z
url: https://github.com/astral-sh/ty/issues/81
synced_at: 2026-01-10T02:07:35Z
```

# Workspace diagnostics

---

_Issue opened by @MichaReiser on 2025-03-14 10:28_

Add support for workspace-wide diagnostics in addition to showing diagnostics for open files only. Default to workspace-wide diagnostics, but with an option to switch to open file diagnostics.

## Work items

- [x] Initial support for workspace diagnostics using `workspace/diagnostic` endpoint (https://github.com/astral-sh/ruff/pull/18939)
- [x] Fix VS Code not clearing diagnostics in non-workspace mode (https://github.com/astral-sh/ruff/pull/18939 > "Test Plan" > "VS Code") (https://github.com/astral-sh/ruff/pull/19273)
- [x] Consider open virtual files (_might_ requires #619) (https://github.com/astral-sh/ruff/pull/19322)
- [ ] Add support using publish diagnostics notification (_low priority_) [^1]
- [ ] Consider Jupyter Notebooks (_low priority_)

[^1]: This is a low priority mainly because clients like VS Code, Zed and (soon) Neovim supports the `workspace/diagnostic` (pull diagnostic) endpoint.

---

_Label `server` added by @MichaReiser on 2025-03-14 10:28_

---

_Renamed from "[red-knot] Open file or project level diagnostics" to "Open file or project level diagnostics" by @MichaReiser on 2025-05-07 15:13_

---

_Renamed from "Open file or project level diagnostics" to "Workspace diagnostics" by @MichaReiser on 2025-05-28 11:19_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-06-04 06:19_

---

_Comment by @dhruvmanila on 2025-07-15 05:55_

> Add support using publish diagnostics notification (low priority)

I think we should only do this if there are user requests for it otherwise it might get complicated to support both the push and pull model for workspace diagnostics.

> Consider Jupyter Notebooks (low priority)

Merging this into #173 

---

Closing this as completed. I've opened a separate issue for the one remaining task which is related to performance.

---

_Closed by @dhruvmanila on 2025-07-15 05:55_

---
