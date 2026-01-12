```yaml
number: 21519
title: Only render hyperlinks for terminals known to support them
type: pull_request
state: merged
author: MichaReiser
labels:
  - cli
assignees: []
merged: true
base: main
head: micha/supports-hyperlinks
created_at: 2025-11-19T08:17:17Z
updated_at: 2025-11-19T13:15:59Z
url: https://github.com/astral-sh/ruff/pull/21519
synced_at: 2026-01-12T15:57:26Z
```

# Only render hyperlinks for terminals known to support them

---

_@MichaReiser_

## Summary

GitHub action's renderer doesn't support hyperlink rendering and, unlike the terminals I tested,
it also isn't just ignoring the hyperlink escape sequence, instead it removes/hides the entire content between the Hyperlink escape sequence. 

This PR uses [`supports-hyperlinks`](https://docs.rs/supports-hyperlinks/3.1.0/supports_hyperlinks/) to only render hyperlinks 
on terminals that are known to support the escape sequence. 

The crate is used by cargo and miette. 


## Test Plan

Is mypy primer timing out proof enough that ty no longer renders the hyperlink ANSI escapes in CI 😅?

I tested that the hyperlinks are clickable in Ghostty


---

_Label `cli` added by @MichaReiser on 2025-11-19 08:17_

---

_@MichaReiser reviewed on 2025-11-19 08:17_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/full.rs`:733 on 2025-11-19 08:17_

ahh, I hate that the trailing whitespace is needed here

---

_Comment by @astral-sh-bot[bot] on 2025-11-19 08:19_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@MichaReiser reviewed on 2025-11-19 08:23_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/full.rs`:707 on 2025-11-19 08:23_

I made those tests "normal" snapshot tests because some snapshot lines contain trailing whitespace that Zed strips (might be a bug in Zed as I don't think an editor should strip trailing whitespace in a string!). 

I spent some time yesterday trying to fix the trailing whitespace but without success.

---

_Renamed from "Only reneder hyperlinks for terminals known to support them" to "Only render hyperlinks for terminals known to support them" by @AlexWaygood on 2025-11-19 08:23_

---

_Marked ready for review by @MichaReiser on 2025-11-19 08:33_

---

_Review requested from @carljm by @MichaReiser on 2025-11-19 08:33_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-19 08:33_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-19 08:33_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-11-19 08:33_

---

_Comment by @astral-sh-bot[bot] on 2025-11-19 08:33_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_@sharkdp approved on 2025-11-19 08:51_

That library looks reasonable. And I don't mind if this buys us a few "false negatives".

---

_Merged by @MichaReiser on 2025-11-19 09:02_

---

_Closed by @MichaReiser on 2025-11-19 09:02_

---

_Branch deleted on 2025-11-19 09:02_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render/full.rs`:707 on 2025-11-19 13:15_

Yeah I support this. My editor (nvim) is configured to do the same.

---

_@BurntSushi reviewed on 2025-11-19 13:15_

LGTM

---
