```yaml
number: 15014
title: Check diagnostic refresh support from client capability
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: dhruv/diagnostic-refresh
created_at: 2024-12-16T09:02:16Z
updated_at: 2024-12-16T10:56:42Z
url: https://github.com/astral-sh/ruff/pull/15014
synced_at: 2026-01-12T15:55:49Z
```

# Check diagnostic refresh support from client capability

---

_@dhruvmanila_

## Summary

Per the LSP spec, the property name is `workspace.diagnostics` with an `s` at the end but the `lsp-types` dependency uses `workspace.diagnostic` (without an `s`). Our fork contains this fix (https://github.com/astral-sh/lsp-types/commit/0f58d628799182647f688908ba752a33f854fb3a) so we should avoid the hardcoded value.

The implication of this is that the client which doesn't support workspace refresh capability didn't support the [dynamic configuration](https://docs.astral.sh/ruff/editors/features/#dynamic-configuration) feature because the server would _always_ send the workspace refresh request but the client would ignore it. We have a fallback logic to publish the diagnostics instead:

https://github.com/astral-sh/ruff/blob/5f6fc3988b5ce19790746921bd80cde9b646d690/crates/ruff_server/src/server/api/notifications/did_change_watched_files.rs#L28-L40

fixes: #15013 

## Test Plan

### VS Code

https://github.com/user-attachments/assets/61ac8e6f-aa20-41cc-b398-998e1866b5bc

### Neovim


https://github.com/user-attachments/assets/4131e13c-3fba-411c-9bb7-478d26eb8d56



---

_Label `bug` added by @dhruvmanila on 2024-12-16 09:02_

---

_Label `server` added by @dhruvmanila on 2024-12-16 09:02_

---

_Comment by @github-actions[bot] on 2024-12-16 09:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dhruvmanila on 2024-12-16 09:13_

There's something weird going on in Neovim as I'm seeing duplicate diagnostics when changing the configuration file. I wonder if it's related to the fact that we're publishing the diagnostics when Neovim might pull diagnostics.

---

_Marked ready for review by @dhruvmanila on 2024-12-16 09:35_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-12-16 09:35_

---

_@MichaReiser approved on 2024-12-16 10:09_

---

_Merged by @dhruvmanila on 2024-12-16 10:56_

---

_Closed by @dhruvmanila on 2024-12-16 10:56_

---

_Branch deleted on 2024-12-16 10:56_

---
