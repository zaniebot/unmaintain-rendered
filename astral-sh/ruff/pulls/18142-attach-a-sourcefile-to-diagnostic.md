```yaml
number: 18142
title: "Attach a `SourceFile` to `Diagnostic`"
type: pull_request
state: closed
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
base: main
head: brent/diagnostic-source-file
created_at: 2025-05-16T20:00:00Z
updated_at: 2025-05-19T17:40:47Z
url: https://github.com/astral-sh/ruff/pull/18142
synced_at: 2026-01-10T18:51:01Z
```

# Attach a `SourceFile` to `Diagnostic`

---

_Pull request opened by @ntBre on 2025-05-16 20:00_

## Summary

As the title says, this PR adds a `SourceFile` field to `ruff_diagnostics::Diagnostic`. After https://github.com/astral-sh/ruff/pull/18051, this will be the main piece of information present in `Message` and not in `ruff_diagnostics::Diagnostic`, so this PR will facilitate replacing `ruff_diagnostics::Diagnostic` with `Message`.

The essential code changes are very small:
- Adding a `source_file: SourceFile` field to `ruff_diagnostics::Diagnostic`
- Adding a `source_file: SourceFile` field to `Checker`

Other than that, the changes are just plumbing `SourceFile` arguments through functions to get them to `ruff_diagnostics::Diagnostic::new` calls.

## Test Plan

Existing tests


---

_Label `internal` added by @ntBre on 2025-05-16 20:00_

---

_Label `diagnostics` added by @ntBre on 2025-05-16 20:00_

---

_Comment by @github-actions[bot] on 2025-05-16 20:09_

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

_Marked ready for review by @ntBre on 2025-05-16 22:12_

---

_Review requested from @AlexWaygood by @ntBre on 2025-05-16 22:12_

---

_Review requested from @MichaReiser by @ntBre on 2025-05-16 22:35_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-05-16 22:35_

---

_Comment by @MichaReiser on 2025-05-17 15:04_

The PR summary is a bit confusing because it isn't clear to me when/if you're speaking about the ruff or linter `Diagnostic` type. Maybe rewrite it to `DiagnosticOld` and clarify at the beginning what `Diagnostic` points to.

(It may be too late for this but I liked what @BurntSushi did early on in his refactor: He renamed the old `Diagnostic` type to `OldDiagnostic`. This helped clarify a lot in his PRs)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:157 on 2025-05-17 15:11_

Can't we pass the file by reference instead of cloning everywhere?

---

_@MichaReiser reviewed on 2025-05-17 15:11_

---

_Comment by @ntBre on 2025-05-17 15:16_

Oh sorry, this PR is all about the old `ruff_diagnostics::Diagnostic`. I guess I only typed that out once and then dropped the prefix. The new `ruff_db::Diagnostic`s also have a file attached, so the old `ruff_diagnostics::Diagnostic` type is the only one of the three (including `Message`) without an associated file.

I think you might be right about it being a bit late. My plan after this and #18051 is to delete `ruff_diagnostics::Diagnostic` (`OldDiagnostic`) entirely. I had a branch stacked on #18051 doing that when I realized I needed the file, which seemed good to split off.

---

_@MichaReiser reviewed on 2025-05-17 15:16_

I feel bad saying this because this must have taken you a lot of time but I feel a bit concerned about introducing a new required argument for every lint rule. 

Could we take inspiration from `InferContext::report_lint` and e.g. introduce a `checker.report_violation(violation, range)` that returns a `DiagnosticGuard` that automatically injects the file and pushes the diagnostic on drop? 

I didn't review all call sites to get a good sense if this works in all cases but it would avoid passing file everywhere 



---

_Comment by @ntBre on 2025-05-17 15:28_

> I feel bad saying this because this must have taken you a lot of time but I feel a bit concerned about introducing a new required argument for every lint rule.
> 
> Could we take inspiration from `InferContext::report_lint` and e.g. introduce a `checker.report_violation(violation, range)` that returns a `DiagnosticGuard` that automatically injects the file and pushes the diagnostic on drop?
> 
> I didn't review all call sites to get a good sense if this works in all cases but it would avoid passing file everywhere

No worries, I was concerned about that too. I'll take a look at `InferContext::report_lint`!

It definitely won't work everywhere, at least as a method on `Checker`, because we don't have a `Checker` available in a surprising number of cases. However, we still _can_ pass a `Checker` to some subset of those. That's actually what I did first before switching to pass only the `SourceFile`. So I think it will still be useful in the vast majority of cases.

---

_Comment by @MichaReiser on 2025-05-17 15:30_

> It definitely won't work everywhere, at least as a method on Checker, because we don't have a Checker available in a surprising number of cases. However, we still can pass a Checker to some subset of those. That's actually what I did first before switching to pass only the SourceFile. So I think it will still be useful in the vast majority of cases.

I'm fine passing `SourceFile` in places where we don't have a `Checker`. I think some of those have a `context` parameter on which we can store it instead.

---

_Comment by @ntBre on 2025-05-19 17:38_

I'm going to go ahead and close this one instead of trying to rebase onto #18051. I'll put up a second attempt with a new `Checker` method for constructing diagnostics after I study `InferContext` a bit.

---

_Closed by @ntBre on 2025-05-19 17:38_

---

_Branch deleted on 2025-05-19 17:40_

---
