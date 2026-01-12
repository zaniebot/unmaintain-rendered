```yaml
number: 173
title: Notebook support
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-03-14T10:10:58Z
updated_at: 2025-11-13T12:46:37Z
url: https://github.com/astral-sh/ty/issues/173
synced_at: 2026-01-12T15:54:22Z
```

# Notebook support

---

_@MichaReiser_

Support for jupyter notebooks

The infrastructure already supports notebooks but we should test if it's necessary to have any notebook-specific behavior and if the infrastructure works as expected (LSP, diagnostic rendering etc)

## Work items

- [x] Handle open (`notebookDocument/didOpen`) and close (`notebookDocument/didClose`) notifications
- [x] Handle change notifications (`notebookDocument/didChange`, `workspace/didChangeWatchedFiles`)
- [x] Consider notebooks for document diagnostic and ~~workspace diagnostics~~ (not supported)

---

_Renamed from "[red-knot] Notebook support" to "Notebook support" by @MichaReiser on 2025-05-07 15:26_

---

_Label `feature` added by @MichaReiser on 2025-05-28 11:28_

---

_Label `feature` removed by @MichaReiser on 2025-05-28 11:28_

---

_Added to milestone `GA` by @MichaReiser on 2025-05-28 11:28_

---

_Comment by @manzt on 2025-07-04 16:18_

Would be a nice feature. One wishlist notebook-specific behavior (if possible) would be aware of cell [`executionOrder`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#notebookDocument_synchronization).

Pyright ignores it, so you lose a lot of static analysis when you, say, create a cell above another that defined some variables. This is generally not a problem for traditional notebooks, where out of order execution is a problem for reproducibility. But in a reactive notebook, like marimo, cell order doesn’t matter. Looking at the spec, it seems like `executionOrder` could offer an opportunity to compose with other static analysis tools.

---

_Comment by @dhruvmanila on 2025-07-07 04:23_

> Would be a nice feature. One wishlist notebook-specific behavior (if possible) would be aware of cell [`executionOrder`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#notebookDocument_synchronization).

Thanks! Yeah, I agree that this seems like a useful information to consider. I don't think Ruff stores or uses this information but we'll keep this in mind when implementing notebook support in ty.

---

_Removed from milestone `GA` by @MichaReiser on 2025-08-15 12:19_

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 12:19_

---

_Label `server` added by @MichaReiser on 2025-08-15 12:19_

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 15:33_

---

_Added to milestone `GA` by @carljm on 2025-08-15 15:33_

---

_Closed by @MichaReiser on 2025-11-13 12:23_

---
