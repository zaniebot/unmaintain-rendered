```yaml
number: 18074
title: "Inline `DiagnosticKind` into other diagnostic types"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/inline-diagnostic-kind
created_at: 2025-05-13T18:21:07Z
updated_at: 2025-05-15T14:27:23Z
url: https://github.com/astral-sh/ruff/pull/18074
synced_at: 2026-01-10T18:51:01Z
```

# Inline `DiagnosticKind` into other diagnostic types

---

_Pull request opened by @ntBre on 2025-05-13 18:21_

## Summary

This PR deletes the `DiagnosticKind` type by inlining its three fields (`name`, `body`, and `suggestion`) into three other diagnostic types:  `Diagnostic`, `DiagnosticMessage`, and `CacheMessage`.

Instead of deferring to an internal `DiagnosticKind`, both `Diagnostic` and `DiagnosticMessage` now have their own macro-generated `AsRule` implementations.

This should make both https://github.com/astral-sh/ruff/pull/18051 and another follow-up PR changing the type of `name` on `CacheMessage` easier since its type will be able to change separately from `Diagnostic` and `DiagnosticMessage`.

## Test Plan

Existing tests


---

_Label `diagnostics` added by @ntBre on 2025-05-13 18:21_

---

_Comment by @github-actions[bot] on 2025-05-13 18:27_

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

_Marked ready for review by @ntBre on 2025-05-13 21:26_

---

_Review requested from @MichaReiser by @ntBre on 2025-05-13 21:26_

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/map_codes.rs`:476 on 2025-05-14 06:05_

By how much does this increase the code size, considering that this is a pretty large match?

Should we instead implement `TryFrom<&str> for Rule` and the `AsRule` impl for `Diagnostic` and `DiagnosticMessagte` both call `Rule::try_from(&self.name).or_else(|| panic!("invalid rule name: {}", self.name))`

---

_@MichaReiser approved on 2025-05-14 06:06_

This looks good. I only skimmed through and I've one comment related to code size/compile time.

---

_Label `internal` added by @MichaReiser on 2025-05-14 06:19_

---

_@ntBre reviewed on 2025-05-14 13:06_

---

_Review comment by @ntBre on `crates/ruff_macros/src/map_codes.rs`:476 on 2025-05-14 13:06_

Oh good question. There's even a strum [macro](https://docs.rs/strum_macros/latest/strum_macros/derive.EnumString.html) for generating a  `FromStr` (and `TryFrom<&str>`) implementation that I used in one of my drafts, so we can definitely switch to that if you prefer. I guess there's no reason _not_ to do that, unless we're worried about people calling `Rule::from_str` directly.

---

_Merged by @ntBre on 2025-05-15 14:27_

---

_Closed by @ntBre on 2025-05-15 14:27_

---

_Branch deleted on 2025-05-15 14:27_

---
