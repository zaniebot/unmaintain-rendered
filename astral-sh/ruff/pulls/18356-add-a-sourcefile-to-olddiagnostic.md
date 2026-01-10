```yaml
number: 18356
title: "Add a `SourceFile` to `OldDiagnostic`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/diagnostic-source-file
created_at: 2025-05-28T18:38:48Z
updated_at: 2025-05-30T13:38:33Z
url: https://github.com/astral-sh/ruff/pull/18356
synced_at: 2026-01-10T18:45:04Z
```

# Add a `SourceFile` to `OldDiagnostic`

---

_Pull request opened by @ntBre on 2025-05-28 18:38_

Summary
--

This is the last main difference between the `OldDiagnostic` and `Message`
types, so attaching a `SourceFile` to `OldDiagnostic` should make combining the
two types almost trivial.

Initially I updated the remaining rules without access to a `Checker`  to take a
`&SourceFile` directly, but after Micha's suggestion in https://github.com/astral-sh/ruff/pull/18356#discussion_r2113281552, I updated all of these calls to take a
`LintContext` instead. This new type is a thin wrapper around a `RefCell<Vec<OldDiagnostic>>`
and a `SourceFile` and now has the `report_diagnostic` method returning a `DiagnosticGuard` instead of `Checker`.
This allows the same `Drop`-based implementation to be used in cases without a `Checker` and also avoids a lot of intermediate allocations of `Vec<OldDiagnostic>`s.

`Checker` now also contains a `LintContext`, which it defers to for its `report_diagnostic` methods, which I preserved for convenience.

Test Plan
--

Existing tests

---

_Label `internal` added by @ntBre on 2025-05-28 18:38_

---

_Label `diagnostics` added by @ntBre on 2025-05-28 18:38_

---

_Comment by @github-actions[bot] on 2025-05-28 18:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-05-28 19:08_

---

_Review requested from @AlexWaygood by @ntBre on 2025-05-28 19:08_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-05-28 19:09_

---

_Review requested from @MichaReiser by @ntBre on 2025-05-28 19:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:122 on 2025-05-29 06:03_

This is a bit unfortunate, because `SourceFile`'s index is lazy too. But I also don't see an easy way to share the line index. So I think this is fine (It's also very rare now that we access the line index during linting. We needed it before we changed from `row`/`column` positions to `TextRane`.)


It could make sense (can be a follow up PR/issue to do it later) to investigate if we can remove `locator.line_index` and instead use `source_file` everywhere. We could even consider implementing `Located` on source file, which should allow us to replace most `Locator` usages.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:115 on 2025-05-29 06:04_

I think the `LazyCell` here is mostly useless because you always dereference it when calling `check_tokens`, `check_file_path`. We should probably just build it eagerly

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/whitespace_before_parameters.rs`:80 on 2025-05-29 06:11_

Nit: We could consider to have a `context.report_diagnostic` method that returns the same guard as `checker.report_diagnostic` but I think this is fine too

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/type_comment_in_stub.rs`:45 on 2025-05-29 06:12_

Nit: We could consider introducing a `DiagnosticsCollector` or similar which is a thin wrapper around a `Vec<OldDIagnostic>` and ` SourceFile` with a `report_diagnostic` method. This would avoid introducing more arguments and would allow us to use the same pattern in more places.

---

_@MichaReiser approved on 2025-05-29 06:17_

This looks good. The only annoyance is that this introduces a new argument to so many functions. It may be worth checking if we can replace some `Locator` usages by using `SourceFile` instead (`str` implements `Located` which gives access to most methods on `Locator`))

---

_@ntBre reviewed on 2025-05-29 12:42_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/whitespace_before_parameters.rs`:80 on 2025-05-29 12:42_

I did consider that briefly, but I thought it might be annoying to make the guard generic over the context type. I think your `DiagnosticsCollector` idea below could help with that too, though! I'll give it a try.

---

_@ntBre reviewed on 2025-05-29 12:46_

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:122 on 2025-05-29 12:46_

Ahh okay, that makes sense. I did notice that `Locator` was passed pretty much everywhere `SourceFile` was. I thought a little bit about storing the `SourceFile` _in_ `Locator` but not about replacing `Locator`. This sounds interesting to try too.

---

_@MichaReiser reviewed on 2025-05-29 12:53_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/whitespace_before_parameters.rs`:80 on 2025-05-29 12:53_

Oh, I haven't thoubht about the context. Yeah, that could be annoying

---

_Comment by @ntBre on 2025-05-29 18:32_

I ran with your suggestion about the `DiagnosticsCollector` and replaced pretty much all of the places I had previously passed a `&SourceFile`. This seems quite a bit nicer, and we get to use the new guard API too. I also opened #18376 to track the `Locator` idea. The new commits are PR-sized on their own, so this could probably use another review if you want.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:240 on 2025-05-29 21:00_

Nit. Maybe rename to diagnostics. It's the more important part of the name 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:2964 on 2025-05-29 21:01_

Same here and in other places. Rename to diagnostics

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/noqa.rs`:50 on 2025-05-29 21:08_

I think this api opens the door for multiple mutable borrows. First when calling with_diagnostics and then when calling report_diagnostics on the collector. I think I would instead add an as_mut_slice method that takes a mut self. The method here should then also require no changes 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:3103 on 2025-05-29 21:10_

I think this method should take a mut self to avoid mutable borrows (eg by calling collectir.report from inside the callback)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:3110 on 2025-05-29 21:11_

Same, ideally this would take a mut self

---

_@MichaReiser reviewed on 2025-05-29 21:11_

I haven't done a full review but here some thoughts 

---

_@ntBre reviewed on 2025-05-29 21:35_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:240 on 2025-05-29 21:35_

Ah okay, that makes sense. I replaced all of them!

Do you think `diagnostics.report_diagnostic` is too repetitive? And `diagnostics.into_diagnostics`, though that one is much less common.

---

_@ntBre reviewed on 2025-05-29 21:38_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/noqa.rs`:50 on 2025-05-29 21:38_

Thanks, these all make sense. I saw `RefCell` and just made everything `&self` without thinking about this.

---

_@ntBre reviewed on 2025-05-29 21:48_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/noqa.rs`:50 on 2025-05-29 21:48_

Actually, I may be slightly misunderstanding in this case. What do you mean by the `as_mut_slice` method? I tried writing

```rust
pub(crate) fn as_mut_slice(&mut self) -> &mut [OldDiagnostic] {
    self.diagnostics.borrow_mut().as_mut_slice()
}
```

but the temporary value from `borrow_mut` is dropped in the function and can't be returned. This is actually the original problem I ran into. All I really wanted was to call `self.diagnostics.borrow().iter()`, but the borrow gets dropped. That's what led me to add `with_diagnostics`.

For now I've just updated `with_diagnostics` to take a `&mut self` like `retain` and `swap_remove`.

---

_@MichaReiser reviewed on 2025-05-30 05:45_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/noqa.rs`:50 on 2025-05-30 05:45_

You can use `self.diagnostics.get_mut` when you have a `&self mut` because the borrow checker than guarantees that no one else can be mutating `diagnostic`.

---

_@MichaReiser reviewed on 2025-05-30 05:47_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:240 on 2025-05-30 05:47_

It's not ideal for sure :) I also thought about `LintContext` That would allow us to put more stuff on it that's needed by all lint rules and isn't directly tied to `Checker`. 

---

_@ntBre reviewed on 2025-05-30 11:36_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/noqa.rs`:50 on 2025-05-30 11:36_

Oh :facepalm: Thank you!

I think I'll just replace all of these helper methods with an `as_mut` method. Some of them actually need the `Vec`, so I'm thinking either `as_mut_vec` or maybe `as_diagnostics` for the name.

I also added an `iter` method, also using `get_mut`, just to avoid having to call `as_mut_vec().iter()`.

---

_@ntBre reviewed on 2025-05-30 11:37_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:240 on 2025-05-30 11:37_

I like that. I can do one more rename to `LintContext` and update the variable names to `context`.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:120 on 2025-05-30 13:25_

We can probably just remove this. At this point, it's almost guaranteed that the line index is `none` (unless I"m mistaken)

---

_@MichaReiser approved on 2025-05-30 13:25_

---

_Merged by @ntBre on 2025-05-30 13:34_

---

_Closed by @ntBre on 2025-05-30 13:34_

---

_Branch deleted on 2025-05-30 13:34_

---
