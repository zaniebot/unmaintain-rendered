```yaml
number: 10404
title: "Automatic configuration reloading for `ruff server`"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/configuration/reload
created_at: 2024-03-14T02:47:04Z
updated_at: 2024-03-21T20:22:20Z
url: https://github.com/astral-sh/ruff/pull/10404
synced_at: 2026-01-12T15:55:32Z
```

# Automatic configuration reloading for `ruff server`

---

_@snowsignal_

## Summary

Fixes #10366.

`ruff server` now registers a file watcher on the client side using the LSP protocol, and listen for events on configuration files. On such an event, it reloads the configuration in the 'nearest' workspace to the file that was changed.

## Test Plan

N/A


---

_Label `server` added by @snowsignal on 2024-03-14 02:47_

---

_Renamed from "Automatic configuration reload for `ruff server`" to "Automatic configuration reloading for `ruff server`" by @snowsignal on 2024-03-14 02:47_

---

_Comment by @github-actions[bot] on 2024-03-14 03:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-03-17 21:47_

Cool.

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session.rs`:257 on 2024-03-17 21:49_

How does configuration work here exactly? Do we find one configuration for the workspace, and then use that for all files?

---

_@charliermarsh reviewed on 2024-03-17 21:49_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:72 on 2024-03-18 07:45_

Do we need to check if the client supports dynamic `didChangeWatchedFiles` requests https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#didChangeWatchedFilesClientCapabilities

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:67 on 2024-03-18 07:48_

Hardcoding 1 here seems a bit odd. It might be overkill for now, but should we use an `AtomicUsize` to get the next request id (that can be used when sending other requests too)

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:95 on 2024-03-18 07:50_

Do you mean response from `client`?
```suggestion
        // Flush response from the client (to avoid an unexpected response appearing in the event loop)
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:274 on 2024-03-18 07:56_

Nit: I couldn't guess the return value of `workspace_for_url` from the call site (I had no idea what the first tuple element should be). I had to navigate to the method declaration and read the code. 

Maybe rename to `entry_for_url` and `entry_for_url_mut` but keep `workspace_for_url` and `workspace_for_url_mut` that use `entry_for_url` internally. I think having `workspace_for_url` and `workspace_for_url_mut` that returns a `&Workspace` (or `&mut Workspace`) does make sense and avoids destructuring or indexing in some call sites. 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:98 on 2024-03-18 07:57_

 Do you have ideas on how you want to support this in the future for other requests from the server that possibly happen inside of the event loop (where it isn't guaranteed that the client won't send any other requests).

---

_@MichaReiser reviewed on 2024-03-18 07:57_

---

_@charliermarsh reviewed on 2024-03-18 15:16_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session.rs`:257 on 2024-03-18 15:16_

(I'm asking because I'd like to understand how it works if a user has subdirectories with distinct `ruff.toml` configurations that should only be applied to files in each respective subdirectory.)

---

_@snowsignal reviewed on 2024-03-18 17:25_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session.rs`:257 on 2024-03-18 17:25_

Good question! Right now, we only process one configuration file per workspace, but supporting this sub-folder configuration is on the roadmap. We do support this in a limited capacity at the moment - if a workspace is a subfolder of another workspace, we use the configuration file found in that workspace folder instead of the one in the parent workspace.

---

_@charliermarsh reviewed on 2024-03-18 17:29_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session.rs`:257 on 2024-03-18 17:29_

How do you envision structuring / supporting it?

---

_@snowsignal reviewed on 2024-03-18 17:32_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:72 on 2024-03-18 17:32_

We do, good catch.

---

_Review comment by @snowsignal on `crates/ruff_server/src/session.rs`:274 on 2024-03-18 17:33_

Okay! I was worried that having separate methods would be 'too much' but I think we can justify it here.

---

_@snowsignal reviewed on 2024-03-18 17:33_

---

_@snowsignal reviewed on 2024-03-18 17:37_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:98 on 2024-03-18 17:37_

Responses from the client are received the same way as requests/notifications, so we'd handle them as part of the event loop - the scheduler would dispatch 'callback' tasks if the response is significant.

---

_@snowsignal reviewed on 2024-03-18 18:30_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session.rs`:257 on 2024-03-18 18:30_

We'd have a 'configuration index' in each workspace that would point to all valid configuration files and their locations. We could probably even cache the resolved configuration for this file path as part of the index. When taking a snapshot of a document, we'd find the corresponding resolved configuration from this index using the file path.

---

_Review requested from @MichaReiser by @snowsignal on 2024-03-18 20:49_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:87 on 2024-03-21 17:11_

Nit: Could we use `fetch_add` here? the next request would probably need a new id anyway?

---

_@MichaReiser approved on 2024-03-21 17:11_

---

_Merged by @snowsignal on 2024-03-21 20:17_

---

_Closed by @snowsignal on 2024-03-21 20:17_

---

_Branch deleted on 2024-03-21 20:17_

---
