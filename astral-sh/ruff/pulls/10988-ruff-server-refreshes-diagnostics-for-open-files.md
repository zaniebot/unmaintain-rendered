```yaml
number: 10988
title: "`ruff server` refreshes diagnostics for open files when file configuration is changed"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/diagnostics/refresh
created_at: 2024-04-17T01:15:04Z
updated_at: 2024-04-18T02:54:46Z
url: https://github.com/astral-sh/ruff/pull/10988
synced_at: 2026-01-10T22:37:01Z
```

# `ruff server` refreshes diagnostics for open files when file configuration is changed

---

_Pull request opened by @snowsignal on 2024-04-17 01:15_

## Summary

The server now requests a [workspace diagnostic refresh](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#diagnostic_refresh) when a configuration file gets changed. This means that diagnostics for all open files will be automatically re-requested by the client on a config change.

## Test Plan

You can test this by opening several files in VS Code, setting `select` in your file configuration to `[]`, and observing that the diagnostics go away once the file is saved (besides any `Pylance` diagnostics). Restore it to what it was before, and you should see the diagnostics automatically return once a save happens.

---

_Label `server` added by @snowsignal on 2024-04-17 01:15_

---

_Added to milestone `Ruff Server: Beta` by @snowsignal on 2024-04-17 01:15_

---

_Comment by @github-actions[bot] on 2024-04-17 01:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/notifications/did_change_watched_files.rs`:32 on 2024-04-17 07:06_

I think we should show a warning to users if the `workspace_refresh` capapbiltiy is not supported, telling them that they might see stale diagnostics and should restart the server. Or is there another way for us to push diagnostics (or clear them)?

Related to my question on the show error message PR. Why don't we show an error message here if senting the refresh request failed? 

---

_@MichaReiser approved on 2024-04-17 07:06_

---

_@dhruvmanila reviewed on 2024-04-17 12:41_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/notifications/did_change_watched_files.rs`:32 on 2024-04-17 12:41_

Neovim doesn't support this capability and I don't know of any server which would send a message to the client so I'm not sure if it's ok to do so. Rust analyzer does refresh the workspace when any of the `Cargo.toml` file changes but that's probably something else.

---

_Comment by @MichaReiser on 2024-04-17 13:08_

It would be nice to have integration tests for this :P

---

_@MichaReiser reviewed on 2024-04-17 13:31_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/notifications/did_change_watched_files.rs`:32 on 2024-04-17 13:31_

What do you mean by refreshing the workspace? Is this another LSP command that we could use as fallback?

My main concern is that we'll get user reports that diagnostics are stale if we don't indicate in anyway that we weren't able to nuke them.

---

_@snowsignal reviewed on 2024-04-17 16:13_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/notifications/did_change_watched_files.rs`:32 on 2024-04-17 16:13_

@MichaReiser At the moment we can't even check whether or not the capability is supported due to a [bug in `lsp-types`](https://github.com/gluon-lang/lsp-types/pull/281). I recommend we do this in a follow-up issue.

> Why don't we show an error message here if senting the refresh request failed?

The callback only runs if the request succeeds. Any erroneous response is [handled internally](https://github.com/astral-sh/ruff/blob/518b29a9efaa7c7a269c91d484806dff97f8a041/crates/ruff_server/src/server/client.rs#L165-L168).

---

_Merged by @snowsignal on 2024-04-17 16:14_

---

_Closed by @snowsignal on 2024-04-17 16:14_

---

_Branch deleted on 2024-04-17 16:14_

---

_@dhruvmanila reviewed on 2024-04-18 02:54_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/notifications/did_change_watched_files.rs`:32 on 2024-04-18 02:54_

> What do you mean by refreshing the workspace? Is this another LSP command that we could use as fallback?

I'd need to look into it but whenever I change any `Cargo.toml` files, the server starts indexing the project again. Maybe the server is watching over those files and starting to refresh all the relevant things like semantic tokens, inlay hints and possibly diagnostics.

---
