```yaml
number: 20526
title: "[`playground`]: Fix non‑BMP code point handling in quick‑fixes and markers"
type: pull_request
state: merged
author: danparizher
labels:
  - playground
assignees: []
merged: true
base: main
head: fix-16266
created_at: 2025-09-23T02:33:46Z
updated_at: 2025-09-24T11:02:27Z
url: https://github.com/astral-sh/ruff/pull/20526
synced_at: 2026-01-10T17:40:28Z
```

# [`playground`]: Fix non‑BMP code point handling in quick‑fixes and markers

---

_Pull request opened by @danparizher on 2025-09-23 02:33_

## Summary

Fixes #16266

Updated the playground to convert UTF‑32 (code point) columns from the WASM diagnostics/fixes into Monaco’s UTF‑16 columns when constructing both quick‑fix edit ranges and diagnostic markers in `playground/ruff/src/Editor/SourceEditor.tsx`.

With this change, applying the `C408` quick fix on `dict(𠤪=1)` now correctly yields `{"𠤪": 1}` without leaving a stray `)`.

---

_Label `playground` added by @MichaReiser on 2025-09-23 08:26_

---

_@MichaReiser requested changes on 2025-09-23 08:26_

Thank you. I think I'd prefer to handle this the same way as we do in ty: That is, implement the conversion in `ruff_wasm`. The benefit of this is that downstream `ruff_wasm` users also benefit from this change (I suspect that most downstream users use JS too and would require the exact same conversion).

See https://github.com/astral-sh/ruff/blob/a9b6618f88552caf4d9a9cc4d4920a0ed7400da6/crates/ty_wasm/src/lib.rs#L88-L92

---

_Review requested from @MichaReiser by @danparizher on 2025-09-23 12:58_

---

_Comment by @github-actions[bot] on 2025-09-23 13:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_wasm/src/lib.rs`:234 on 2025-09-23 16:56_

This works but it will break existing clients that assumed UTF8. 

I suggest adding `positionEncoding` a new argument to `new` (similar to ty) and storing it on the `Workspace` and then using it here.

This allows each user to use the encoding that best suits their use case.

---

_@MichaReiser reviewed on 2025-09-23 16:56_

---

_Review requested from @MichaReiser by @danparizher on 2025-09-23 18:04_

---

_@MichaReiser approved on 2025-09-24 08:07_

Thanks. this is great

---

_Merged by @MichaReiser on 2025-09-24 08:08_

---

_Closed by @MichaReiser on 2025-09-24 08:08_

---

_Branch deleted on 2025-09-24 11:02_

---
