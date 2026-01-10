```yaml
number: 88
title: Add test infrastructure for the language server
type: issue
state: closed
author: dhruvmanila
labels:
  - server
  - testing
assignees: []
created_at: 2025-02-28T04:38:18Z
updated_at: 2025-07-23T06:56:59Z
url: https://github.com/astral-sh/ty/issues/88
synced_at: 2026-01-10T02:06:24Z
```

# Add test infrastructure for the language server

---

_Issue opened by @dhruvmanila on 2025-02-28 04:38_

Currently, there are very limited tests for both the language server and the VS Code extension (E2E tests).

We do get some of the testing done via the linter and formatter tests but not much for the server specific behavior.

Following is an enumeration of the existing tests in the language server:
* Settings: [here](https://github.com/astral-sh/ruff/blob/cf83584abb130574059064ad5f0dd806c22e45d5/crates/ruff_server/src/session/settings.rs#L531-L532) and [here](https://github.com/astral-sh/ruff/blob/cf83584abb130574059064ad5f0dd806c22e45d5/crates/ruff_server/src/session/index/ruff_settings.rs#L454-L455)
* Did change behavior: [here](https://github.com/astral-sh/ruff/blob/cf83584abb130574059064ad5f0dd806c22e45d5/crates/ruff_server/src/edit/notebook.rs#L245-L246) and [here](https://github.com/astral-sh/ruff/blob/cf83584abb130574059064ad5f0dd806c22e45d5/crates/ruff_server/src/edit/text_document.rs#L152-L153)
* Text replacement behavior: [here](https://github.com/astral-sh/ruff/blob/cf83584abb130574059064ad5f0dd806c22e45d5/crates/ruff_server/src/edit/replacement.rs#L63-L64)

Most, if not all, of the above tests are in consequence of bug reports.

Following existing servers have a mock server that they use for testing purposes:

- https://github.com/facebook/starlark-rust/blob/main/starlark_lsp/src/test.rs
- https://github.com/rust-lang/rust-analyzer/blob/master/crates/rust-analyzer/tests/slow-tests/support.rs
- https://github.com/supabase-community/postgres_lsp/blob/main/crates/pglt_lsp/tests/server.rs (Uses `tower-lsp`)

A mock server implementation would be immensely helpful but there's one limitation that I see which is that the [settings indexer](https://github.com/astral-sh/ruff/blob/cf83584abb130574059064ad5f0dd806c22e45d5/crates/ruff_server/src/session/index/ruff_settings.rs#L99-L114) walks through the actual file system. It probably won't be an issue in the red-knot server given that we can use the in-memory file system but we would need to have some kind of solution for the Ruff language server.


---

_Label `server` added by @dhruvmanila on 2025-02-28 04:38_

---

_Label `testing` added by @dhruvmanila on 2025-02-28 04:38_

---

_Comment by @dhruvmanila on 2025-03-18 11:09_

Moving this over to astral-sh/ruff#16737 

---

_Closed by @dhruvmanila on 2025-03-18 11:09_

---

_Reopened by @MichaReiser on 2025-03-18 12:45_

---

_Renamed from "Add test infrastructure for the language server" to "[red-knot] add test infrastructure for the language server" by @carljm on 2025-03-27 19:02_

---

_Label `server` added by @MichaReiser on 2025-05-07 15:08_

---

_Label `testing` added by @MichaReiser on 2025-05-07 15:08_

---

_Renamed from "[red-knot] add test infrastructure for the language server" to "add test infrastructure for the language server" by @MichaReiser on 2025-05-07 15:17_

---

_Renamed from "add test infrastructure for the language server" to "Add test infrastructure for the language server" by @dhruvmanila on 2025-06-25 09:16_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-06-25 09:37_

---

_Closed by @dhruvmanila on 2025-07-23 06:56_

---
