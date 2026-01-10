```yaml
number: 20322
title: "Remove Diagnostic::expect_range and all consumers"
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
created_at: 2025-09-09T22:51:27Z
updated_at: 2025-09-11T00:19:22Z
url: https://github.com/astral-sh/ruff/pull/20322
synced_at: 2026-01-10T17:40:28Z
```

# Remove Diagnostic::expect_range and all consumers

---

_Pull request opened by @amyreese on 2025-09-09 22:51_

Replace usage with `range().unwrap_or_default()` or more appropriate
alternatives based on context.


---

_Review requested from @carljm by @amyreese on 2025-09-09 22:51_

---

_Review requested from @MichaReiser by @amyreese on 2025-09-09 22:51_

---

_Review requested from @sharkdp by @amyreese on 2025-09-09 22:51_

---

_Review requested from @dcreager by @amyreese on 2025-09-09 22:51_

---

_Comment by @github-actions[bot] on 2025-09-09 23:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @amyreese on 2025-09-09 23:33_

---

_Review request for @carljm removed by @carljm on 2025-09-10 00:20_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:370 on 2025-09-10 12:46_

I think in these rule cases we should try to use the range passed to `report_diagnostic` or `report_diagnostic_if_enabled`. We can just use `prev.range()` again or factor out a shared `range` variable in this instance.

It works either way since the range will always be `Some`, but it seems more clear to me.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/isort/mod.rs`:660 on 2025-09-10 12:53_

Just for fun, would you try deleting one of these sort calls entirely? I'm pretty sure the diagnostics from `test_path` should already be sorted, so we may be able to drop all of these `sort_by_key` and `unwrap_or_default` calls.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:175 on 2025-09-10 13:38_

Same as above, we can just use the `range` passed to `report_diagnostic_if_enabled`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyflakes/mod.rs`:791 on 2025-09-10 13:39_

Unlike isort, I think we actually do need this one since pyflakes uses a slightly different test runner. It might make sense to use `Diagnostic::ruff_sort_ordering` again if it doesn't change any snapshots, though. Hopefully we'll unify these with the other tests soon anyway (https://github.com/astral-sh/ruff/pull/19977).

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:518 on 2025-09-10 13:42_

I think I'd probably immediately if-let on the range itself, but what you have is fine too:


```suggestion
            if let Some(range) = diagnostic.range() {
                let noqa_offset = directives.noqa_line_for.resolve(range.start());
                diagnostic.set_noqa_offset(noqa_offset);
            }
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:988 on 2025-09-10 13:47_

Maybe another good use for `Diagnostic::ruff_sort_ordering`

---

_Review comment by @ntBre on `crates/ruff_linter/src/noqa.rs`:892 on 2025-09-10 13:50_

I'm actually not sure, but I think we don't want to unwrap this one. That would put a noqa offset at the start of a file for any diagnostic without a range, if I understand correctly. I think we should just put the references to `noqa_offset` in an if-let here.

---

_Review comment by @ntBre on `crates/ruff_server/src/lint.rs`:237 on 2025-09-10 13:57_

I double checked what ty does for this, and it looks like ranges are required for `lsp_types::Diagnostic`. We fall back to the default range in ty too:

https://github.com/astral-sh/ruff/blob/7a75702237e6f0bc605d3d8e9470c4ca9f37c69e/crates/ty_server/src/server/api/diagnostics.rs#L300-L302

so this looks good!

---

_Review comment by @ntBre on `crates/ruff_wasm/src/lib.rs`:239 on 2025-09-10 14:07_

This also seems right to me. In ty, it looks like we preserve the optionality on the Rust side but unwrap to a default value in TypeScript:

https://github.com/astral-sh/ruff/blob/7a75702237e6f0bc605d3d8e9470c4ca9f37c69e/playground/ty/src/Editor/Diagnostics.tsx#L74-L75

---

_@ntBre reviewed on 2025-09-10 14:10_

Thanks! Just a few minor suggestions, but this looks good overall

---

_@amyreese reviewed on 2025-09-10 17:10_

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/isort/mod.rs`:660 on 2025-09-10 17:10_

Yep, removing these did not break tests.

---

_Review comment by @amyreese on `crates/ruff_linter/src/noqa.rs`:892 on 2025-09-10 18:03_

This one was tricky to try and unravel what's going on, and I *think* this is actually correct as-is:

It's using the range to look for an associated noqa directive (`find_lines_with_directive` takes the range and does a binary search against noqa directives that it knows about). With a default range, IIUC it should never find a directive, and therefore fall through to the following code at line 915, which reports a `None` directive using the line from the default range.

With an if-let defining `noqa_offset` guarding the use of `find_line_with_directive`, then `noqa_offset` is no longer defined and usable by the block at line 915.

---

_@amyreese reviewed on 2025-09-10 18:03_

---

_@ntBre reviewed on 2025-09-10 19:12_

---

_Review comment by @ntBre on `crates/ruff_linter/src/noqa.rs`:892 on 2025-09-10 19:12_

Hmm, maybe you're right. I'm honestly not that familiar with this part of the code. I was just afraid that you'd be able to put a noqa comment at the start of a file for a diagnostic without a range. But maybe that's even correct?

Anyway, this is actually unreachable until we add any diagnostics without ranges, so this seems fine. Even the panics shouldn't hit this path because they don't have rule/noqa codes anyway.

Thanks for looking into it!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:175 on 2025-09-10 19:16_

Sorry I stopped marking these, but I was hoping we could do a fix similar to `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs` for each of the `diagnostic.range()` calls immediately after constructing a diagnostic. In this case it would look something like:


```suggestion
                        let range = TextRange::at(token.end(), trailing_len);
                        if let Some(mut diagnostic) = ... {
                            diagnostic.set_fix(Fix::safe_edit(Edit::range_deletion(range)));
                        }
```

(with some of the context from above added to the suggestion since github won't let me select it all)

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:194 on 2025-09-10 19:17_

Same as above:

```rust
let range = TextRange::at(token.start() - offset, offset);
...
diagnostic.set_fix(Fix::safe_edit(Edit::range_deletion(range)));
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:221 on 2025-09-10 19:17_

Same as above

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:240 on 2025-09-10 19:17_

Same as above

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:269 on 2025-09-10 19:17_

Same as above

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:293 on 2025-09-10 19:18_

Same as above

---

_Review comment by @ntBre on `crates/ruff_linter/src/test.rs`:386 on 2025-09-10 19:31_

This one could probably be an if-let too. I _think_ we could hit this one with the panics, depending on how we end up testing them (or if a rule panics in a future test, but you'd probably only hit that while trying to fix a bug).

---

_@ntBre reviewed on 2025-09-10 19:32_

---

_Review comment by @amyreese on `crates/ruff_linter/src/test.rs`:386 on 2025-09-10 21:26_

I also added if lets guarding the other use of `range.unwrap()` here, since those are also likely to cause issues in the same way.

---

_@amyreese reviewed on 2025-09-10 21:26_

---

_@ntBre reviewed on 2025-09-10 23:03_

---

_Review comment by @ntBre on `crates/ruff_linter/src/test.rs`:386 on 2025-09-10 23:03_

Good call!

---

_@ntBre approved on 2025-09-10 23:03_

---

_Label `internal` added by @ntBre on 2025-09-10 23:03_

---

_Label `diagnostics` added by @ntBre on 2025-09-10 23:03_

---

_Merged by @amyreese on 2025-09-11 00:19_

---

_Closed by @amyreese on 2025-09-11 00:19_

---

_Branch deleted on 2025-09-11 00:19_

---
