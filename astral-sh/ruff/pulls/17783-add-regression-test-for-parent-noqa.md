```yaml
number: 17783
title: "Add regression test for parent `noqa`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/delete-parent-fields
created_at: 2025-05-01T21:36:39Z
updated_at: 2025-05-03T15:38:33Z
url: https://github.com/astral-sh/ruff/pull/17783
synced_at: 2026-01-12T15:56:05Z
```

# Add regression test for parent `noqa`

---

_@ntBre_

Summary
--

Adds a regression test for #2253 after I tried to delete the fix from #2464.


---

_Label `internal` added by @ntBre on 2025-05-01 21:36_

---

_Comment by @github-actions[bot] on 2025-05-01 21:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-05-02 06:08_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:1 on 2025-05-02 06:08_

It doesn't look like this was added in https://github.com/astral-sh/ruff/pull/16837 Can you expand on why you think this is unused

---

_@ntBre reviewed on 2025-05-02 12:43_

---

_Review comment by @ntBre on `crates/ruff_linter/src/noqa.rs`:1 on 2025-05-02 12:43_

Well I thought all of the references to the `parent` field I found were `None` or constructed from a `CacheMessage`, but I see now that there _is_ a reference from `Diagnostic` that can actually be `Some` (in `Message::from_diagnostic`). The fact that I deleted it and nothing failed was my other big data point, but I guess we just don't have a test for it.

I guess I didn't add a useless field, but trying to delete a useful one might be worse, sorry.

---

_@ntBre reviewed on 2025-05-02 22:19_

---

_Review comment by @ntBre on `crates/ruff_linter/src/noqa.rs`:1 on 2025-05-02 22:19_

Okay, I looked at this a bit more closely, and you were definitely right. This case was handled in #2464, but no tests were added to cover it. I think we can repurpose this PR to add the test if that's okay with you, or I can just close it entirely.

---

_Renamed from "Delete `parent` field on `DiagnosticMessage` and `CacheMessage`" to "Add regression test for parent `noqa`" by @ntBre on 2025-05-02 22:47_

---

_@MichaReiser approved on 2025-05-03 13:47_

Nice, thanks for adding a test!

---

_Merged by @ntBre on 2025-05-03 15:38_

---

_Closed by @ntBre on 2025-05-03 15:38_

---

_Branch deleted on 2025-05-03 15:38_

---
