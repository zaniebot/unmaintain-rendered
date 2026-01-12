```yaml
number: 11092
title: "`ruff server`: Support publish diagnostics as a fallback when pull diagnostics aren't supported"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/diagnostics/publish
created_at: 2024-04-22T20:17:15Z
updated_at: 2024-04-23T19:43:31Z
url: https://github.com/astral-sh/ruff/pull/11092
synced_at: 2026-01-12T15:55:34Z
```

# `ruff server`: Support publish diagnostics as a fallback when pull diagnostics aren't supported

---

_@snowsignal_

## Summary

Fixes #11059 

Several major editors don't support [pull diagnostics](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_pullDiagnostics), a method of sending diagnostics to the client that was introduced in version `0.3.17` of the specification. Until now, `ruff server` has only used pull diagnostics, which resulted in diagnostics not being available on Neovim and Helix, which don't support pull diagnostics yet (though Neovim `10.0` will have support for this).

`ruff server` will now utilize the older method of sending diagnostics, known as 'publish diagnostics', when pull diagnostics aren't supported by the client. This involves re-linting a document every time it is opened or modified, and then sending the diagnostics generated from that lint to the client via the `textDocument/publishDiagnostics` notification.

## Test Plan

The easiest way to test that this PR works is to check if diagnostics show up on Neovim `<=0.9`.


---

_Label `server` added by @snowsignal on 2024-04-22 20:17_

---

_Added to milestone `Ruff Server: Beta` by @snowsignal on 2024-04-22 20:17_

---

_@snowsignal reviewed on 2024-04-22 20:21_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/notifications/did_change.rs`:46 on 2024-04-22 20:21_

We can `.unwrap()` here instead of throwing an error because the document is guaranteed to exist (we've already modified it) which means that the snapshot should also exist.

---

_Comment by @github-actions[bot] on 2024-04-22 20:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-04-23 01:34_

---

_Merged by @snowsignal on 2024-04-23 04:06_

---

_Closed by @snowsignal on 2024-04-23 04:06_

---

_Branch deleted on 2024-04-23 04:06_

---

_@dhruvmanila reviewed on 2024-04-23 04:40_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/notifications/did_change.rs`:46 on 2024-04-23 04:40_

I'd add it as a `SAFETY:` comment for posterity.

---

_@dhruvmanila reviewed on 2024-04-23 04:49_

Thank you for implementing it so quickly!

Unless it's already done, we'd need to clear the diagnostics for the document for `on_close` event otherwise the diagnostics will remain in the "Problems" tab even after closing the file. Refer https://github.com/astral-sh/ruff-lsp/blob/187d7790be0783b9ac41ce025a724cf389bf575c/ruff_lsp/server.py#L459-L464

---

_Comment by @MichaReiser on 2024-04-23 07:29_

I think our old extension also had an option to pull diagnostics only on save. But I don't mind pushing this out until we're asked to build this (especially because we can't really support it when using pull diagnostics)

---

_Comment by @dhruvmanila on 2024-04-23 08:46_

@MichaReiser Are you referring to `onType` / `onSave` option value? I was about to comment on that.

---

_Comment by @snowsignal on 2024-04-23 19:41_

@dhruvmanila I've opened an issue (https://github.com/astral-sh/ruff/issues/11114) to solve the problem you mentioned earlier.

---

_Comment by @snowsignal on 2024-04-23 19:43_

As for the `onSave`/`onType` setting - I was going to hold off on implementing that unless there seems to be a need for it.

---
