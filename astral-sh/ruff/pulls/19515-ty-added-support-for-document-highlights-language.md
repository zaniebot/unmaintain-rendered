```yaml
number: 19515
title: "[ty] Added support for \"document highlights\" language server feature."
type: pull_request
state: merged
author: UnboundVariable
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: doc_highlight
created_at: 2025-07-23T19:45:37Z
updated_at: 2025-07-24T20:06:29Z
url: https://github.com/astral-sh/ruff/pull/19515
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Added support for "document highlights" language server feature.

---

_Pull request opened by @UnboundVariable on 2025-07-23 19:45_

This PR adds support for the "document highlights" language server feature.

This feature allows a client to highlight all instances of a selected name within a document. Without this feature, editors perform highlighting based on a simple text match. This adds semantic knowledge.

The implementation of this feature largely overlaps that of the recently-added "references" feature. This PR refactors the existing "references.rs" module, separating out the functionality and tests that are specific to the other language feature into a "goto_references.rs" module. The "references.rs" module now contains the functionality that is common to "goto references", "document highlights" and "rename" (which is not yet implemented).

As part of this PR, I also created a new `ReferenceTarget` type which is similar to the existing `NavigationTarget` type but better suited for references. This idea was suggested by @MichaReiser in [this code review feedback](https://github.com/astral-sh/ruff/pull/19475#discussion_r2224061006) from a previous PR. Notably, this new type contains a field that specifies the "kind" of the reference (read, write or other). This "kind" is needed for the document highlights feature.

Before: all textual instances of `foo` are highlighted
<img width="156" height="126" alt="Screenshot 2025-07-23 at 12 51 09 PM" src="https://github.com/user-attachments/assets/37ccdb2f-d48a-473d-89d5-8e89cb6c394e" />

After: only semantic matches are highlighted
<img width="164" height="157" alt="Screenshot 2025-07-23 at 12 52 05 PM" src="https://github.com/user-attachments/assets/2efadadd-4691-4815-af04-b031e74c81b7" />

---

_Review requested from @carljm by @UnboundVariable on 2025-07-23 19:45_

---

_Review requested from @MichaReiser by @UnboundVariable on 2025-07-23 19:45_

---

_Review requested from @AlexWaygood by @UnboundVariable on 2025-07-23 19:45_

---

_Review requested from @sharkdp by @UnboundVariable on 2025-07-23 19:45_

---

_Review requested from @dcreager by @UnboundVariable on 2025-07-23 19:45_

---

_Comment by @github-actions[bot] on 2025-07-23 19:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @MichaReiser on 2025-07-24 05:45_

---

_Label `ty` added by @MichaReiser on 2025-07-24 05:45_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/doc_highlights.rs`:134 on 2025-07-24 07:20_

Nit: You could create a single diagnostic instead of multiple once by adding multiple annotations to the same diagnostic. This would help reduce the size of the snapshot (because it skips the header)

You can add multiple annotations by calling `diagnostic.annotate` multiple times

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-24 07:28_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/lib.rs`:135 on 2025-07-24 08:37_

Nit: Could we use `FileRange` here?

---

_@MichaReiser approved on 2025-07-24 08:37_

Nice!

---

_@UnboundVariable reviewed on 2025-07-24 19:34_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/doc_highlights.rs`:134 on 2025-07-24 19:34_

The problem I see with that approach is that it doesn't allow us to include the "kind" ("Read", "Write", "Other") for each reference.

---

_Merged by @UnboundVariable on 2025-07-24 20:06_

---

_Closed by @UnboundVariable on 2025-07-24 20:06_

---

_Branch deleted on 2025-07-24 20:06_

---
