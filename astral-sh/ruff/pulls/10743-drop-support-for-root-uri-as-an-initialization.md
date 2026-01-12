```yaml
number: 10743
title: "Drop support for `root_uri` as an initialization parameter in `ruff_server`"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/drop-root-uri-support
created_at: 2024-04-02T21:29:56Z
updated_at: 2024-04-03T06:24:30Z
url: https://github.com/astral-sh/ruff/pull/10743
synced_at: 2026-01-12T15:55:33Z
```

# Drop support for `root_uri` as an initialization parameter in `ruff_server`

---

_@snowsignal_

## Summary

Needed for https://github.com/astral-sh/ruff/pull/10686.

We no longer support `root_uri` as an initialization parameter, relying solely on `workspace_folders` to find the working directories. This means that the minimum supported LSP version is now `0.3.6`.

## Test Plan

When opening a folder in VS Code, you shouldn't see any errors in the log which say `No workspace(s) were provided(...)`.


---

_Label `server` added by @snowsignal on 2024-04-02 21:29_

---

_@zanieb reviewed on 2024-04-02 21:34_

---

_Review comment by @zanieb on `crates/ruff_server/src/server.rs`:52 on 2024-04-02 21:34_

```suggestion
        if let Some(version) = 😲
```

---

_Review requested from @zanieb by @snowsignal on 2024-04-03 03:05_

---

_Comment by @github-actions[bot] on 2024-04-03 03:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila approved on 2024-04-03 03:47_

Makes sense 👍. I don't think we supported that in `ruff-lsp` either but not sure.

> This means that the minimum supported LSP version is now `0.3.6`.

Can you clarify if this is the dependency version or the LSP spec version?

---

_Comment by @snowsignal on 2024-04-03 03:51_

> Can you clarify if this is the dependency version or the LSP spec version?

This would be the LSP spec version.

---

_Merged by @snowsignal on 2024-04-03 03:52_

---

_Closed by @snowsignal on 2024-04-03 03:52_

---

_Branch deleted on 2024-04-03 03:52_

---

_Comment by @MichaReiser on 2024-04-03 06:10_

> We no longer support root_uri as an initialization parameter, relying solely on workspace_folders to find the working directories. This means that the minimum supported LSP version is now 0.3.6.

Note: An alternative would have been to mark the usages as `allow(deprecated)` but I think removing it is fine considering that the most recent LSP spec is 3.17 and 3.1 was released in 2017 (I couldn't find the release date for LSP 0.3.6)

---

_Comment by @snowsignal on 2024-04-03 06:24_

@MichaReiser I think eventually the plan will be to build workspace folders solely from the settings passed in via initialization options, which would let us avoid using `workspace_folders` as well.

---
