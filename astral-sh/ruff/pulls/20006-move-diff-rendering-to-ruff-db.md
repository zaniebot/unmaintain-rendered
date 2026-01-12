```yaml
number: 20006
title: "Move diff rendering to `ruff_db`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/ruff-db-diff
created_at: 2025-08-20T16:33:42Z
updated_at: 2025-08-21T13:47:02Z
url: https://github.com/astral-sh/ruff/pull/20006
synced_at: 2026-01-12T15:56:52Z
```

# Move diff rendering to `ruff_db`

---

_@ntBre_

Summary
--

This is a preparatory PR in support of #19919. It moves our `Diff` rendering code from `ruff_linter` to `ruff_db`, where we have direct access to the `DiagnosticStylesheet` used by our other diagnostic rendering code. As shown by the tests, this shouldn't cause any visible changes. The colors aren't exactly the same, as I note in a TODO comment, but I don't think there's any existing way to see those, even in tests.

The `Diff` implementation is mostly unchanged. I just switched from a Ruff-specific `SourceFile` to a `DiagnosticSource` (removing an `expect_ruff_source_file` call) and updated the `LineStyle` struct and other styling calls to use `fmt_styled` and our existing stylesheet.

In support of these changes, I added three styles to our stylesheet: `insertion` and `deletion` for the corresponding diff operations, and `underline`, which apparently we _can_ use, as I hoped on Discord. This isn't supported in all terminals, though. It worked in ghostty but not in st for me.

I moved the `calculate_print_width` function from the now-deleted `diff.rs` to a method on `OneIndexed`, where it was available everywhere we needed it. I'm not sure if that's desirable, or if my other changes to the function are either (using `ilog10` instead of a loop). This does make it `const` and slightly simplifies things in my opinion, but I'm happy to revert it if preferred.

I also inlined a version of `show_nonprinting` from the `ShowNonprinting` trait in `ruff_linter`:

https://github.com/astral-sh/ruff/blob/f4be05a83be770c8c9781088508275170396b411/crates/ruff_linter/src/text_helpers.rs#L3-L5

This trait is now only used in `source_kind.rs`, so I'm not sure it's worth having the trait or the macro-generated implementation (which is only called once). This is obviously closely related to our unprintable character handling in diagnostic rendering, but the usage seems different enough not to try to combine them.

https://github.com/astral-sh/ruff/blob/f4be05a83be770c8c9781088508275170396b411/crates/ruff_db/src/diagnostic/render.rs#L990-L998

We could also move the trait to another crate where we can use it in `ruff_db` instead of inlining here, of course.

Finally, this PR makes `TextEmitter` a very thin wrapper around a `DisplayDiagnosticsConfig`. It's still used in a few places, though, unlike the other emitters we've replaced, so I figured it was worth keeping around. It's a pretty nice API for setting all of the options on the config and then passing that along to a `DisplayDiagnostics`.

Test Plan
--

Existing snapshot tests with diffs


---

_Comment by @github-actions[bot] on 2025-08-20 16:35_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-20 16:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `internal` added by @ntBre on 2025-08-20 16:45_

---

_Marked ready for review by @ntBre on 2025-08-20 16:51_

---

_Review requested from @carljm by @ntBre on 2025-08-20 16:51_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-20 16:51_

---

_Review requested from @sharkdp by @ntBre on 2025-08-20 16:51_

---

_Review requested from @dcreager by @ntBre on 2025-08-20 16:51_

---

_Review request for @dcreager removed by @ntBre on 2025-08-20 16:51_

---

_Review request for @carljm removed by @ntBre on 2025-08-20 16:51_

---

_Review request for @sharkdp removed by @ntBre on 2025-08-20 16:51_

---

_Comment by @github-actions[bot] on 2025-08-20 16:54_

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

_Review comment by @MichaReiser on `crates/ruff_source_file/src/line_index.rs`:626 on 2025-08-20 17:13_

I don't think I understand what this method does :sweat. I'd find it helpful if it had a few doctests to demonstrate what it does.

The name `width` is also somewhat misleading because it suggests to me that it computes the unicode width, which it doesn't (but, as I said, I don't understand what it does).

If I understand this correctly, this computes the number of digits? Maybe `digits` or `digits_len` would be  better name. 

---

_@MichaReiser approved on 2025-08-20 17:13_

This is a bit tough to review because the moves the code and changes the code are in the same commit but I don't think it's worth the work to split the commits now.

---

_@ntBre reviewed on 2025-08-20 17:44_

---

_Review comment by @ntBre on `crates/ruff_source_file/src/line_index.rs`:626 on 2025-08-20 17:44_

This sounds like a good reason to put it back to the old version:

https://github.com/astral-sh/ruff/blob/1a38831d53c13c3a957ff2c62d9faa171e9533ce/crates/ruff_linter/src/message/diff.rs#L190-L202

But yes, it just computes the number of digits. I can add doctests and rename it either way.

Sorry about not splitting the commits!

---

_Review comment by @MichaReiser on `crates/ruff_source_file/src/line_index.rs`:626 on 2025-08-20 17:48_

No worries. I think it's fine on `OneIndexed`. It's just the name that I found confusing. I think `digits` would be easier to understand.

Could we use this also in https://github.com/astral-sh/ruff/blob/7dfde3b929c70b5f5fb9933ef09b8005717a8d85/crates/ty_server/src/server/api/requests/completion.rs#L60

---

_@MichaReiser reviewed on 2025-08-20 17:48_

---

_@ntBre reviewed on 2025-08-20 18:09_

---

_Review comment by @ntBre on `crates/ruff_source_file/src/line_index.rs`:626 on 2025-08-20 18:09_

I meant the old implementation in the new method, but either works for me. I renamed and expanded the docs.

It looks like we can use it there in ty too! Is it okay to unwrap since we know completions is non-empty?

```rust
        let max_index_len = OneIndexed::new(completions.len()).unwrap().digits().get();
```

Otherwise we can use `OneIndexed::from_zero_indexed`, but that adds 1 after `saturating_sub` subtracted one, so it seems a little silly. We can also `unwrap_or_default`.

---

_Merged by @ntBre on 2025-08-21 13:47_

---

_Closed by @ntBre on 2025-08-21 13:47_

---

_Branch deleted on 2025-08-21 13:47_

---
