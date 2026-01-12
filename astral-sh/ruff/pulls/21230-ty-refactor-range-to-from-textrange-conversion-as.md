```yaml
number: 21230
title: "[ty] Refactor `Range` to/from `TextRange` conversion as prep for notebook support"
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
head: micha/lsp-range
created_at: 2025-11-03T01:58:45Z
updated_at: 2025-11-05T13:24:06Z
url: https://github.com/astral-sh/ruff/pull/21230
synced_at: 2026-01-12T15:57:19Z
```

# [ty] Refactor `Range` to/from `TextRange` conversion as prep for notebook support

---

_@MichaReiser_

## Summary

This PR changes the signature of the `TextSize` <-> `Position` and `TextRange` <-> `Range` conversion methods in preparation for notebook support.

According to the LSP specification, each Notebook consists of many `TextDocument`s. However, ty internally concatenates all cells to a single text document. That means, offsets coming from ty need to be mapped back to their corresponding cell, and then to the offset within the cell. The same applies for LSP offsets: The cell-relative offset needs to be converted to an absolute offset into the concatenated notebook document.

This PR doesn't implement the logic itself for mapping between the different offsets but it refactors the call-sites and the methods used for those conversions to add the required logic in a follow-up PR.

The most important change is that we now pass `db` and `File` to the conversion methods. This will allow us to query the notebook to resolve the cell metadata (offset per cell, etc).

## Test Plan

Existing tests. I tested different LSP features and they seem to work fine

---

_Label `internal` added by @MichaReiser on 2025-11-03 01:58_

---

_Label `ty` added by @MichaReiser on 2025-11-03 01:58_

---

_Comment by @github-actions[bot] on 2025-11-03 02:00_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-03 02:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser reviewed on 2025-11-03 02:08_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/completion.rs`:54 on 2025-11-03 02:08_

It's a bit unfortunate that we need the `url` here but the file isn't unique enough (the file maps to the entire notebook). We need the `url` to lookup the corresponding cell within the notebook.

---

_@MichaReiser reviewed on 2025-11-03 02:10_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/completion.rs`:77 on 2025-11-03 02:10_

Using `to_local_range` here is technically incorrect. In fact, it's very likely that the importer adds the import to the very first cell within the notebook, in which case the edit is not local to a single cell. 

However, fixing this requires changes to the importer and they're substantial enough to justify a separate PR

---

_@MichaReiser reviewed on 2025-11-03 02:27_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/doc_highlights.rs`:60 on 2025-11-03 02:27_

We'll need to limit the `document_highlights` to the current cell. This seems very unfortunate as the highlights are very likely to be cross cell but the LSP protocol doesn't allow us to specify a URI. Again, I think this is best done in a separate PR

---

_@MichaReiser reviewed on 2025-11-03 02:29_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/document_symbols.rs`:108 on 2025-11-03 02:29_

Same here: We need to limit the document symbol request to the current cell. But I'd prefer doing that in a separate PR

---

_@MichaReiser reviewed on 2025-11-03 02:31_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/semantic_tokens.rs`:27 on 2025-11-03 02:31_

We'll need to limit the semantic tokens to a single range but I'll do this in a separate PR

---

_Marked ready for review by @MichaReiser on 2025-11-03 02:50_

---

_Review requested from @carljm by @MichaReiser on 2025-11-03 02:50_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-03 02:50_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-03 02:50_

---

_Label `server` added by @MichaReiser on 2025-11-03 02:50_

---

_Comment by @sharkdp on 2025-11-03 15:35_

Seems like something that maybe @dhruvmanila would be a good candidate to review?

---

_@dhruvmanila reviewed on 2025-11-04 20:48_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/doc_highlights.rs`:60 on 2025-11-04 20:48_

This isn't something that requires to be investigated right now but I tested Pylance and it seems to be able to highlight a symbol across multiple cells in a notebook.

https://github.com/user-attachments/assets/ef8dd35d-9898-40a5-8c22-edf2374c030b

---

_@dhruvmanila approved on 2025-11-04 20:49_

Looks good!

---

_@MichaReiser reviewed on 2025-11-04 20:58_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/doc_highlights.rs`:60 on 2025-11-04 20:58_

I'm not sure this is something Pylance does (although it could because it isn't an LSP).

But I don't see a highlight request for cells at all and the highlighting is also incorrect, suggesting it's just a simple name matching:




https://github.com/user-attachments/assets/1b323d9f-bfa3-4f4b-bc3d-6e2095d649b8



---

_@MichaReiser reviewed on 2025-11-04 20:59_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/doc_highlights.rs`:60 on 2025-11-04 20:59_

It could mean that we should return `Option` from `to_location` but I've to revisit this when being further along with the notebook integration

---

_@dhruvmanila reviewed on 2025-11-04 21:17_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/doc_highlights.rs`:60 on 2025-11-04 21:17_

That makes sense.

---

_Renamed from "[ty] Refactor Range<->TextRange conversion as prep for notebook support" to "[ty] Refactor `Range` to/from `TextRange` conversion as prep for notebook support" by @MichaReiser on 2025-11-05 13:13_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-05 13:21_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-05 13:21_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-05 13:21_

---

_Merged by @MichaReiser on 2025-11-05 13:24_

---

_Closed by @MichaReiser on 2025-11-05 13:24_

---

_Branch deleted on 2025-11-05 13:24_

---
