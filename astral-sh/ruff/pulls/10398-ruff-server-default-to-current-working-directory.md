```yaml
number: 10398
title: "`ruff server` default to current working directory in environments without any root directories or workspaces"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/root-uri
created_at: 2024-03-14T00:15:05Z
updated_at: 2024-03-18T15:46:45Z
url: https://github.com/astral-sh/ruff/pull/10398
synced_at: 2026-01-12T15:55:32Z
```

# `ruff server` default to current working directory in environments without any root directories or workspaces

---

_@snowsignal_

## Summary

Fixes #10324

This removes an overeager failure case where we would exit early if no root directory or workspace folders were provided on server initialization. We now fall-back to the current working directory as a workspace for that file.

## Test Plan
N/A


---

_Label `server` added by @snowsignal on 2024-03-14 00:15_

---

_Review requested from @dhruvmanila by @snowsignal on 2024-03-14 00:15_

---

_Comment by @github-actions[bot] on 2024-03-14 00:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dhruvmanila on 2024-03-14 04:16_

Testing this out...

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server.rs`:48 on 2024-03-14 04:19_

If I understand this correctly, then the workspaces will be empty, right? Should we instead default to using the current working directory? In `ruff-lsp`, we default to using the current working directory as the workspace with the default settings. You might be more aware of how that will affect other parts of the code.

---

_@dhruvmanila reviewed on 2024-03-14 04:19_

---

_Comment by @dhruvmanila on 2024-03-14 04:28_

It's not giving me any diagnostics. The LSP logs have this error:

```
[ERROR][2024-03-14 09:56:07] .../vim/lsp/rpc.lua:789	"rpc"	"/Users/dhruv/work/astral/ruff-test/target/release/ruff"	"stderr"	"┐ruff_server::server::api::notifications::did_open::run{file=file:///Users/dhruv/playground/python/animation.py}\n┘\n"
```

I suspect this is because there's no workspace associated with that document, so the `did_open` notification is failing?

---

_@dhruvmanila reviewed on 2024-03-14 04:28_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server.rs`:48 on 2024-03-14 04:28_

I think we should use the current working directory as the default workspace if not provided.

---

_@snowsignal reviewed on 2024-03-15 21:33_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:48 on 2024-03-15 21:33_

You're right, we should try to use the current working directory. Let me know if that works!

---

_Comment by @snowsignal on 2024-03-15 21:35_

@dhruvmanila Right, it's failing to open because the file isn't associated with any workspace. The assumption that the server has made up until now is that any file being opened by the editor would have an associated workspace folder the client would have previously told us about, but this is clearly not the case.

---

_Comment by @snowsignal on 2024-03-15 21:37_

Actually, maybe what we need to do is just always create a new workspace in the session as a fallback mechanism if an file being opened isn't in a workspace path already.

---

_@dhruvmanila approved on 2024-03-16 02:30_

Thank you! I tested out using diagnostics, code actions, formatting and range formatting. It works well. LGTM!

---

_Renamed from "`ruff server` now works in environments without any root directories or workspaces" to "`ruff server` default to current working directory in environments without any root directories or workspaces" by @MichaReiser on 2024-03-18 08:00_

---

_Merged by @snowsignal on 2024-03-18 15:46_

---

_Closed by @snowsignal on 2024-03-18 15:46_

---

_Branch deleted on 2024-03-18 15:46_

---
