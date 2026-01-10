```yaml
number: 18769
title: "Use `LintContext::report_diagnostic_if_enabled` in `check_tokens`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/check-check-tokens
created_at: 2025-06-18T20:42:11Z
updated_at: 2025-06-18T21:05:38Z
url: https://github.com/astral-sh/ruff/pull/18769
synced_at: 2026-01-10T18:39:08Z
```

# Use `LintContext::report_diagnostic_if_enabled` in `check_tokens`

---

_Pull request opened by @ntBre on 2025-06-18 20:42_

## Summary

This PR avoids the `Vec::retain` call in `check_tokens` by checking if rules are enabled as their diagnostics are constructed.

https://github.com/astral-sh/ruff/blob/2a425e43fd2043b9f71c2cc4a907169cb07fb128/crates/ruff_linter/src/checkers/tokens.rs#L174-L176

Since `LintContext::report_diagnostic_if_enabled` required a `LinterSettings`, I added a `settings` field to the context itself instead of trying to pass it everywhere. This also turned `LogicalLinesContext` into a trivial wrapper around `LintContext`, so I just removed it in favor of using `LintContext` directly too.

The diff is a bit smaller with whitespace hidden since many blocks got moved into something like this:

```rust
if let Some(mut diagnostic) = context.report_diagnostic.enabled(...) {
    // old code
}
```

## Test Plan

Existing tests


---

_Label `internal` added by @ntBre on 2025-06-18 20:42_

---

_Label `diagnostics` added by @ntBre on 2025-06-18 20:42_

---

_Review requested from @MichaReiser by @ntBre on 2025-06-18 20:42_

---

_Review requested from @AlexWaygood by @ntBre on 2025-06-18 20:42_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-06-18 20:42_

---

_@MichaReiser approved on 2025-06-18 20:46_

Nice

---

_Marked ready for review by @ntBre on 2025-06-18 20:46_

---

_Review requested from @AlexWaygood by @ntBre on 2025-06-18 20:46_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-06-18 20:46_

---

_Comment by @github-actions[bot] on 2025-06-18 20:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @ntBre on 2025-06-18 21:05_

---

_Closed by @ntBre on 2025-06-18 21:05_

---

_Branch deleted on 2025-06-18 21:05_

---
