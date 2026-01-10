```yaml
number: 20309
title: "Refactor diagnostic start|end location helpers"
type: pull_request
state: merged
author: amyreese
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: amy/unwind-expect-range
created_at: 2025-09-08T19:43:33Z
updated_at: 2025-09-09T18:39:32Z
url: https://github.com/astral-sh/ruff/pull/20309
synced_at: 2026-01-10T17:46:22Z
```

# Refactor diagnostic start|end location helpers

---

_Pull request opened by @amyreese on 2025-09-08 19:43_

- Renames functions to drop `expect_` from names.
- Make functions return `Option<LineColumn>` to appropriately signal
  when range is not available.
- Update existing consumers to use `unwrap_or_default()`. Uncertain if
  there are better fallback behaviors for individual consumers.


---

_Review requested from @carljm by @amyreese on 2025-09-08 19:43_

---

_Review requested from @MichaReiser by @amyreese on 2025-09-08 19:43_

---

_Review requested from @sharkdp by @amyreese on 2025-09-08 19:43_

---

_Review requested from @dcreager by @amyreese on 2025-09-08 19:43_

---

_Review requested from @ntBre by @amyreese on 2025-09-08 19:43_

---

_Comment by @github-actions[bot] on 2025-09-08 19:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review request for @dcreager removed by @ntBre on 2025-09-08 19:58_

---

_Review request for @carljm removed by @ntBre on 2025-09-08 19:58_

---

_Review request for @sharkdp removed by @ntBre on 2025-09-08 19:58_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:475 on 2025-09-08 20:06_

If we're already returning an `Option`, could we get rid of some of these other `expect` calls too? You might have to move them out of the closure, or use `and_then` instead of `map`, but ideally we'll actually get to remove _all_ the `expect` methods eventually and that seems like another step in that direction.

With a little more work, we could get a `SourceCode` from a ty file too. Something like this:

https://github.com/astral-sh/ruff/blob/d55edb3d74b8220dad93eb6f187489c246698089/crates/ruff_db/src/diagnostic/render/gitlab.rs#L73-L77

Ruff's `EmitterContext` implements the resolver trait needed by `UnifiedFile::diagnostic_source`. 

Then we could potentially use this helper in the output formats that have already been ported over to `ruff_db` too. I'd be okay deferring this part, though.

---

_@ntBre reviewed on 2025-09-08 20:18_

Thanks! I think we could push the refactor a little bit further here.

We could possibly do something more than `unwrap_or_default` in all of the output formats too, but I think this is a fine intermediate step on that end. I'll update the handling in the GitHub format when porting it to ty anyway, which will only leave `grouped` and `sarif`.

---

_Label `internal` added by @ntBre on 2025-09-08 22:14_

---

_Label `diagnostics` added by @ntBre on 2025-09-08 22:14_

---

_@amyreese reviewed on 2025-09-08 22:17_

---

_Review comment by @amyreese on `crates/ruff_db/src/diagnostic/mod.rs`:475 on 2025-09-08 22:17_

> If we're already returning an `Option`, could we get rid of some of these other `expect` calls too? You might have to move them out of the closure, or use `and_then` instead of `map`, but ideally we'll actually get to remove _all_ the `expect` methods eventually and that seems like another step in that direction.

I was initially trying to do that with a couple of chained `.map()` calls, but ran into some issues with the borrow checker that I don't understand, and resorted to this for now.

---

_@ntBre reviewed on 2025-09-08 22:28_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:475 on 2025-09-08 22:28_

Something like this should work:

```rust
    pub fn ruff_start_location(&self) -> Option<LineColumn> {
        let span = self.primary_span()?;
        let source_file = span.as_ruff_file()?;
        self.range()
            .map(|range| source_file.to_source_code().line_column(range.start()))
    }
```

or 

```rust
    pub fn ruff_start_location(&self) -> Option<LineColumn> {
        self.range().and_then(|range| {
            Some(
                self.primary_span()?
                    .as_ruff_file()?
                    .to_source_code()
                    .line_column(range.start()),
            )
        })
    }
```

or I guess using question marks everywhere might be better at that point:

```rust
    pub fn ruff_start_location(&self) -> Option<LineColumn> {
        Some(
            self.primary_span()?
                .as_ruff_file()?
                .to_source_code()
                .line_column(self.range()?.start()),
        )
    }
```

but yeah, it's easy to run into lifetime issues in this area of the code base, especially with `FileResolver` and `DiagnosticSource`.

---

_@amyreese reviewed on 2025-09-08 23:10_

---

_Review comment by @amyreese on `crates/ruff_db/src/diagnostic/mod.rs`:475 on 2025-09-08 23:10_

Turns out that leaning on `ruff_source_file()` was the correct path as that already uses `primary_span_ref()?.as_ruff_file()` to bypass the lifetime issue. Someday I will understand how Rust works... 😅

---

_Comment by @amyreese on 2025-09-08 23:11_

Not sure why the clippy and wasm jobs are failing — do they run from different base revisions or code checkouts?

---

_Comment by @ntBre on 2025-09-08 23:16_

Ah, there are two very-nearly identical chunks of code in the SARIF format. You've modified the non-WASM code:

https://github.com/astral-sh/ruff/blob/c3f1f0b9f1ed8e522a6d22c7d1670997c19183de/crates/ruff_linter/src/message/sarif.rs#L159-L162

but not the WASM code below it:

https://github.com/astral-sh/ruff/blob/c3f1f0b9f1ed8e522a6d22c7d1670997c19183de/crates/ruff_linter/src/message/sarif.rs#L178-L182

The `#[cfg(...)]` stuff is for conditional compilation, so unless you explicitly pass a WASM flag locally that code won't be compiled. That's probably why they pass locally (assuming they do).

---

_Comment by @amyreese on 2025-09-08 23:28_

> Ah, there are two very-nearly identical chunks of code in the SARIF format. You've modified the non-WASM code:

Weird, I would have expected the <F2> rename refactoring to update those as well, or rust/clippy to complain that the code referenced removed functions, even if the lines weren't enabled in my current build. 🫢

---

_@ntBre approved on 2025-09-09 15:17_

Thanks! And nice catch on the docs and using `ruff_source_file`

---

_Merged by @amyreese on 2025-09-09 18:39_

---

_Closed by @amyreese on 2025-09-09 18:39_

---

_Branch deleted on 2025-09-09 18:39_

---
