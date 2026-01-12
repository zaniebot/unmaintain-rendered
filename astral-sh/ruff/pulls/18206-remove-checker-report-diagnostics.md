```yaml
number: 18206
title: "Remove `Checker::report_diagnostics`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/remove-report-diagnostics
created_at: 2025-05-19T20:34:08Z
updated_at: 2025-05-20T14:00:08Z
url: https://github.com/astral-sh/ruff/pull/18206
synced_at: 2026-01-12T15:56:14Z
```

# Remove `Checker::report_diagnostics`

---

_@ntBre_

Summary
--

I thought that emitting multiple diagnostics at once would be difficult to port to a diagnostic construction model closer to ty's `InferContext::report_lint`, so as a first step toward that, this PR removes `Checker::report_diagnostics`.

In many cases I was able to do some related refactoring to avoid allocating a `Vec<Diagnostic>` at all, often by adding a `Checker` field to a `Visitor` or by passing a `Checker` instead of a `&mut Vec<Diagnostic>`.

In other cases, I had to fall back on something like

```rust
for diagnostic in diagnostics {
    checker.report_diagnostic(diagnostic);
}
```

which I guess is a bit worse than the `extend` call in `report_diagnostics`, but hopefully it won't make too much of a difference.

I'm still not quite sure what to do with the remaining loop cases. The two main use cases for collecting a sequence of diagnostics before emitting any of them are:

1. Applying a single `Fix` to a group of diagnostics
2. Avoiding an earlier diagnostic if something goes wrong later

I was hoping we could get away with just a `DiagnosticGuard` that reported a `Diagnostic` on drop, but I guess we will still need a `DiagnosticGuardBuilder` that can be collected in these cases and produce a `DiagnosticGuard` once we know we actually want the diagnostics.

Test Plan
--

Existing tests


---

_Comment by @github-actions[bot] on 2025-05-19 20:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` added by @ntBre on 2025-05-19 21:33_

---

_Label `diagnostics` added by @ntBre on 2025-05-19 21:33_

---

_Marked ready for review by @ntBre on 2025-05-19 21:33_

---

_Review requested from @AlexWaygood by @ntBre on 2025-05-19 21:33_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-05-19 21:37_

---

_Review requested from @MichaReiser by @ntBre on 2025-05-19 21:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs`:917 on 2025-05-20 05:42_

This will be interesting to replace but I suspect will have to make this entire check earlier (or use an internal intermediate representation for the diagnostics, or have a separate `report_diagnostic` API for cases where diagnostics are created first and then maybe dropped later, sort of a defuse method)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_password_func_arg.rs`:55 on 2025-05-20 05:43_

Here, and some other faces where we use `filter_map`: I think it's easier by calling `report_diagnostic` instead of returning `Some` and iterating over the filtered diagnostics

```rust
for keyword in keywords {
	...
	checker.report_diagnostic(Diagnostic::new(...))
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_password_string.rs`:79 on 2025-05-20 05:44_

```rust
for `comp` in comperators {
	...
	checker.report_diagnostic(Diagnostic::new(...))
}
```
	

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/assertion.rs`:598 on 2025-05-20 05:45_

```suggestion
    for handler in handlers {
    	match handler {
```

---

_@MichaReiser approved on 2025-05-20 05:45_

---

_@ntBre reviewed on 2025-05-20 12:46_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_password_func_arg.rs`:55 on 2025-05-20 12:46_

I left some of these because they used `?` to return early, but I can do another pass. `let`-`else`-`continue` isn't too bad either.

---

_Merged by @ntBre on 2025-05-20 14:00_

---

_Closed by @ntBre on 2025-05-20 14:00_

---

_Branch deleted on 2025-05-20 14:00_

---
