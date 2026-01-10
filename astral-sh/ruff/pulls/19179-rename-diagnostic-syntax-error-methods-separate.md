```yaml
number: 19179
title: "Rename `Diagnostic::syntax_error` methods, separate `Ord` implementation"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/diagnostic-cleanup
created_at: 2025-07-07T14:24:10Z
updated_at: 2025-07-08T13:54:21Z
url: https://github.com/astral-sh/ruff/pull/19179
synced_at: 2026-01-10T18:33:12Z
```

# Rename `Diagnostic::syntax_error` methods, separate `Ord` implementation

---

_Pull request opened by @ntBre on 2025-07-07 14:24_

## Summary

This PR addresses some additional feedback on #19053:

- Renaming the `syntax_error` methods to `invalid_syntax` to match the lint id
- Moving the standalone `diagnostic_from_violation` function to `Violation::into_diagnostic`
- Removing the `Ord` and `PartialOrd` implementations from `Diagnostic` in favor of `Diagnostic::start_ordering`

## Test Plan

Existing tests

## Additional Follow-ups

Besides these, I also put the following comments on my todo list, but they seemed like they might be big enough to have their own PRs:

  - [Use `LintId::IOError` for IO errors](https://github.com/astral-sh/ruff/pull/19053#discussion_r2189425922)
  - [Move `Fix` and `Edit`](https://github.com/astral-sh/ruff/pull/19053#discussion_r2189448647)
  - [Avoid so many unwraps](https://github.com/astral-sh/ruff/pull/19053#discussion_r2189465980)


---

_Label `internal` added by @ntBre on 2025-07-07 14:24_

---

_@ntBre reviewed on 2025-07-07 14:27_

---

_Review comment by @ntBre on `crates/ruff/src/commands/check_stdin.rs`:57 on 2025-07-07 14:27_

I mentioned this [here](https://github.com/astral-sh/ruff/pull/19053#discussion_r2190110553), but is  it okay to unwrap these, or should we fall back to `Ordering::Equal` just in case? It shouldn't currently be possible for this to fail, but I'm afraid of introducing a panic.

---

_Comment by @github-actions[bot] on 2025-07-07 14:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @ntBre on `crates/ruff_linter/src/violation.rs`:35 on 2025-07-07 14:28_

This is to pass `self` by value down below. I also have a branch converting all of the various `report_diagnostic` methods to take a reference, if we'd prefer that approach.

---

_@ntBre reviewed on 2025-07-07 14:28_

---

_Comment by @github-actions[bot] on 2025-07-07 14:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-07-07 14:36_

---

_Review requested from @carljm by @ntBre on 2025-07-07 14:36_

---

_Review requested from @AlexWaygood by @ntBre on 2025-07-07 14:36_

---

_Review requested from @sharkdp by @ntBre on 2025-07-07 14:36_

---

_Review requested from @dcreager by @ntBre on 2025-07-07 14:36_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-07 14:36_

---

_Review request for @dcreager removed by @ntBre on 2025-07-07 14:36_

---

_Review request for @carljm removed by @ntBre on 2025-07-07 14:36_

---

_Review request for @sharkdp removed by @ntBre on 2025-07-07 14:36_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-07-07 14:36_

---

_@MichaReiser reviewed on 2025-07-07 14:39_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/check_stdin.rs`:57 on 2025-07-07 14:39_

I think it's fine but is probably something we have to look into once we start e.g. using an IO error without a text range.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:994 on 2025-07-07 14:44_

Hmm. I guess this works and it was probably even me who added `PartialOrd` to `ruff_db::files::File`. But I'm not sure if it's a good idea because the ordering can change between runs (and users would probably expect that the ordering is based on the path). 

However, changing this either requires that `start_ordering` requires a `FileResolver` or we make the method specific to ruff (and expect a ruff file). I'd be fine with either.

The alternative is to at least add a note to `UnifiedFile` that the ordering of the `Ty` file's are based on the salsa index and not on the path and that `Ty` files come before `Ruff` Files

---

_@MichaReiser approved on 2025-07-07 14:45_

Thank you

---

_@ntBre reviewed on 2025-07-07 14:58_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:994 on 2025-07-07 14:58_

Ah, I see. Yeah, I was preparing to rename the method to `ruff_start_ordering` when I decided to try this instead. I might just go back to that approach for now. It would be surprising if this got used in ty and the ordering changed between runs.

I'm not sure if you saw my [comment](https://github.com/astral-sh/ruff/pull/19053#discussion_r2190110553) on the other PR, but we could also use `RenderingSortKey` with a dummy `FileResolver`, if we don't mind falling back to the `DiagnosticId` sort for diagnostics on the same line. That only changed 6 snapshots when I tested it.

---

_@MichaReiser reviewed on 2025-07-07 15:36_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:994 on 2025-07-07 15:36_

> if we don't mind falling back to the DiagnosticId sort for diagnostics on the same line. That only changed 6 snapshots when I tested it.

I think that should be fine. I don't consider the sort order as something that's part of our semver versioned CLI interface

---

_@ntBre reviewed on 2025-07-07 15:52_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:994 on 2025-07-07 15:52_

Okay, I'll try that then. That will avoid the unwrap too.

---

_@ntBre reviewed on 2025-07-07 16:18_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:994 on 2025-07-07 16:18_

It ended up being 10 snapshots. I guess I only replaced one of the two uses in my first test.

The one downside is that we might end up with two dummy resolvers in Ruff if we also implement `FileResolver` for `EmitterContext` as in my current draft of #19133. But we can't really reuse `EmitterContext` in this case because it holds a `&HashMap` and can't implement `Default`.

---

_Review comment by @MichaReiser on `crates/ruff/tests/integration_test.rs`:1137 on 2025-07-07 16:31_

Looking at this now. the sort order is sort of confusing

---

_@MichaReiser reviewed on 2025-07-07 16:31_

---

_@MichaReiser reviewed on 2025-07-07 16:33_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:988 on 2025-07-07 16:33_

```suggestion
#[derive(Debug, Clone, PartialEq, Eq, get_size2::GetSize)]
```

Yeah, I think I've changed my opinion after looking through the snapshots. The new sort order is sort of confusing (Unless we make `RenderingSortKey` parametrizable on whether it should use the `secondary_code` first before using the `id`).

---

_@ntBre reviewed on 2025-07-07 16:34_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:988 on 2025-07-07 16:34_

Sounds good, I'll revert to `start_ordering` and make it Ruff-specific. Thanks for taking a look!

---

_@ntBre reviewed on 2025-07-07 18:49_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:988 on 2025-07-07 18:49_

I tried parameterizing `RenderingSortKey` too, but it also caused some snapshot changes. I'll just stick with `start_ordering` for now, if that's okay with you.

I also went ahead and `expect`ed a range and returned an `Ordering` instead of an `Option` since the function could already panic. 

---

_@MichaReiser approved on 2025-07-08 06:27_

---

_Merged by @ntBre on 2025-07-08 13:54_

---

_Closed by @ntBre on 2025-07-08 13:54_

---

_Branch deleted on 2025-07-08 13:54_

---
