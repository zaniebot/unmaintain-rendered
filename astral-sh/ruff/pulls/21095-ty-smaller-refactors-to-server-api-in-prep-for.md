```yaml
number: 21095
title: "[ty] Smaller refactors to server API in prep for notebook support"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/lsp-notebooks-phase1
created_at: 2025-10-27T12:28:08Z
updated_at: 2025-10-31T20:38:46Z
url: https://github.com/astral-sh/ruff/pull/21095
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Smaller refactors to server API in prep for notebook support

---

_Pull request opened by @MichaReiser on 2025-10-27 12:28_

## Summary

While prototyping notebook support in ty's LSP, I realized that I'm having a hard time understanding `DocumentKey`, `DocumentQuery`, `Document` (which is referred to as controller?). I then decided to take a closer look at those types and ended with a few refactors that I think are worthwhile, getting in independently of notebook support.


* Make `AnySystemPath::key_from_url` non-failable (and in turn, `key_from_url`) by using a virtual path for file URLs that don't represent a valid path.
* Make `snapshot_document` return a `Result` that `Err`s when the document wasn't found (instead of storing the document as `Result`). This was introduced in https://github.com/astral-sh/ruff/pull/19277 but I don't see how it is necessary to ensure we always send a response. The fix added in https://github.com/astral-sh/ruff/pull/19277, which sends a response if the URL isn't valid, can also be applied when the document wasn't found.
* Delete `DocumentQuery`: ty uses salsa to access the file content (e.g. `source_text`) which then reads the latest document version using the `LSPSystem`. That eliminates the need to snapshot most document fields. The only exception to this is the notebook metadata which we now expose directly in `DocumentSnapshot`. I added new `document` methods that return a `&Document` for the `LSPSystem`. 
* I reduced `DocumentKey` to two variants roughly resembling `AnySystemPath`. Its purpose is to uniquely identify a document within `Index`. We could probably use `AnySystemPath` for that for now, but I wanted a distinct type to make it clear that a cell doesn't map to a ty-path (there's no corresponding file). Instead, cells should use the notebook file. 
* I introduced a new `DocumentHandle` type, which is a read-only handle to a document in `Index`. It takes `DocumentKey`'s place in notification handlers because it now exposes the `to_file_path` and `url` methods that were previously on `DocumentKey`. The motivation for the new type were:
  * I wanted `DocumentKey` to only be used to identify a document within `Index`, nothing more 
  * I wanted a more object-oriented API for document, so that we can add `document.close` and `document.update_text_document` to it (rather than having to deal with keys everywhere).
* Most `Session` operations now take `&Url` as an argument, hiding `DocumentKey`.

My plan for notebooks is to store the cells as regular text documents. This is different from Ruff where the notebook cells are stored as part of the `Notebook`. We'll see how well this goes but my thinking is that a model closer to how notebooks are handled within VS Code/the LSP specification, the less likely it is that we run into weird edge cases (e.g. where the notebook gets removed before the cells and we then can't find the cell anymore).


## Test plan

Tried different LSP operations

---

_Label `server` added by @MichaReiser on 2025-10-27 12:28_

---

_Comment by @github-actions[bot] on 2025-10-27 12:30_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@MichaReiser reviewed on 2025-10-27 12:30_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/path.rs`:860 on 2025-10-27 12:30_

This is so that we can use `SystemVirtualPath` as key when calling `dict.get`

---

_Comment by @github-actions[bot] on 2025-10-27 12:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-27 12:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @MichaReiser on 2025-10-27 12:57_

---

_Review requested from @carljm by @MichaReiser on 2025-10-27 12:57_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-27 12:57_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-27 12:57_

---

_Renamed from "[ty] Smaller refactors to server in prep for notebook support" to "[ty] Smaller refactors to server API in prep for notebook support" by @MichaReiser on 2025-10-27 13:00_

---

_Label `internal` added by @MichaReiser on 2025-10-27 13:00_

---

_Label `ty` added by @AlexWaygood on 2025-10-27 14:55_

---

_Review comment by @dcreager on `crates/ty_server/src/document/notebook.rs`:1 on 2025-10-29 19:06_

super nit: I don't know how strict we are with our `use` sections, but I don't think we want `crate` imports in the same section as external-but-not-stdlib imports. (If anything, the `crate` and `super` sections below should be merged)

---

_Review comment by @dcreager on `crates/ty_server/src/server/api/notifications/did_open_notebook.rs`:36 on 2025-10-29 19:11_

nit: Can you pull out `uri` above too to be consistent with the other fields? Or is there an ownership issue?

---

_@dcreager approved on 2025-10-29 19:18_

This looks good to me, and lets me learn more about the LSP internals. Though it probably deserves at least a cursory look from someone who's already more familiar with the LSP!

---

_Merged by @MichaReiser on 2025-10-31 20:00_

---

_Closed by @MichaReiser on 2025-10-31 20:00_

---

_Branch deleted on 2025-10-31 20:00_

---

_Comment by @dhruvmanila on 2025-10-31 20:38_

> My plan for notebooks is to store the cells as regular text documents. This is different from Ruff where the notebook cells are stored as part of the `Notebook`. We'll see how well this goes but my thinking is that a model closer to how notebooks are handled within VS Code/the LSP specification, the less likely it is that we run into weird edge cases (e.g. where the notebook gets removed before the cells and we then can't find the cell anymore).

I agree that this is great!

---
