```yaml
number: 11497
title: "`ruff server`: Fix multiple issues with Neovim and Helix"
type: pull_request
state: merged
author: snowsignal
labels:
  - bug
  - configuration
  - server
assignees: []
merged: true
base: main
head: jane/server/misc-fixes
created_at: 2024-05-22T20:08:31Z
updated_at: 2024-05-27T11:41:18Z
url: https://github.com/astral-sh/ruff/pull/11497
synced_at: 2026-01-12T15:55:38Z
```

# `ruff server`: Fix multiple issues with Neovim and Helix

---

_@snowsignal_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/11236.

This PR fixes several issues, most of which relate to non-VS Code editors (Helix and Neovim).

1. Global-only initialization options are now correctly deserialized from Neovim and Helix
2. Empty diagnostics are now published correctly for Neovim and Helix.
3. A workspace folder is created at the current working directory if the initialization parameters send an empty list of workspace folders.
4. The server now gracefully handles opening files outside of any known workspace, and will use global fallback settings taken from client editor settings and a user settings TOML, if it exists.

## Test Plan

I've tested to confirm that each issue has been fixed.

* Global-only initialization options are now correctly deserialized from Neovim and Helix + the server gracefully handles opening files outside of any known workspace

https://github.com/astral-sh/ruff/assets/19577865/4f33477f-20c8-4e50-8214-6608b1a1ea6b

* Empty diagnostics are now published correctly for Neovim and Helix

https://github.com/astral-sh/ruff/assets/19577865/c93f56a0-f75d-466f-9f40-d77f99cf0637

* A workspace folder is created at the current working directory if the initialization parameters send an empty list of workspace folders.


https://github.com/astral-sh/ruff/assets/19577865/b4b2e818-4b0d-40ce-961d-5831478cc726



---

_Label `bug` added by @snowsignal on 2024-05-22 20:08_

---

_Label `configuration` added by @snowsignal on 2024-05-22 20:08_

---

_Label `server` added by @snowsignal on 2024-05-22 20:08_

---

_Added to milestone `Ruff Server: Beta` by @snowsignal on 2024-05-22 20:08_

---

_Review requested from @charliermarsh by @snowsignal on 2024-05-22 20:08_

---

_Comment by @github-actions[bot] on 2024-05-22 20:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-05-22 20:41_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/server.rs`:75 on 2024-05-22 20:41_

What's the difference between this and the default?

---

_@charliermarsh reviewed on 2024-05-22 20:41_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/server.rs`:96 on 2024-05-22 20:41_

Should `!folders.is_empty()` be a `.filter`?

---

_@charliermarsh approved on 2024-05-22 20:41_

---

_@snowsignal reviewed on 2024-05-22 20:44_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:75 on 2024-05-22 20:44_

The default `serde_json::Value` is `Null`, which we cannot successfully deserialize `InitializationOptions` from. However, we can successfully deserialize `InitalizationOptions` from an empty object (`{}`), since all client settings fields are optional.

---

_Merged by @snowsignal on 2024-05-22 20:50_

---

_Closed by @snowsignal on 2024-05-22 20:50_

---

_Branch deleted on 2024-05-22 20:50_

---

_@dhruvmanila reviewed on 2024-05-27 11:20_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:134 on 2024-05-27 11:20_

@snowsignal when you're back, can you explain why is this being flattened? This is causing #11507 because now it's not under `settings` but rather in the global table.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:134 on 2024-05-27 11:41_

Ok, I understand why it was flattened - to account for the empty initialization options. I think we need to re-think about how to handle these cases.

---

_@dhruvmanila reviewed on 2024-05-27 11:41_

---
