```yaml
number: 21836
title: "Remove `BackwardsTokenizer` based `parenthesized_range` references in `ruff_linter`"
type: pull_request
state: merged
author: denyszhak
labels:
  - internal
assignees: []
merged: true
base: main
head: feat/ruff-linter-to-use-optimized-parenthesized-range
created_at: 2025-12-08T00:57:11Z
updated_at: 2025-12-11T12:04:58Z
url: https://github.com/astral-sh/ruff/pull/21836
synced_at: 2026-01-12T15:57:35Z
```

# Remove `BackwardsTokenizer` based `parenthesized_range` references in `ruff_linter`

---

_@denyszhak_

## Summary

ruff_linter now uses optimized version of parenthesized_range on already precomputed tokens instead of generating those over again

Closes #21835 

## Test Plan

Ensured old tests are still successful. If any additional check required please hint the way and I would love to test.


---

_Review requested from @AlexWaygood by @denyszhak on 2025-12-08 00:57_

---

_@denyszhak reviewed on 2025-12-08 00:57_

---

_Review comment by @denyszhak on `crates/ruff_linter/src/fix/edits.rs`:243 on 2025-12-08 00:57_

I plan to double check on this one 

---

_@denyszhak reviewed on 2025-12-08 01:00_

---

_Review comment by @denyszhak on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quoted-type-alias_TC008.py.snap`:473 on 2025-12-08 01:00_

Regression, will take a look at this soon

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 01:08_


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

_Label `performance` added by @AlexWaygood on 2025-12-08 07:37_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-08 07:38_

---

_@MichaReiser reviewed on 2025-12-08 09:44_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/edits.rs`:243 on 2025-12-08 09:44_

Let's keep using `SimpleTokenizer` here and only change the usages of `parenthesized_ranges`. We can look into replacing more usages of `SimpleTokenizer` in separate PRs.

It's not always trivial to replace `SimpleTokenizer` because the tokens have slightly different semantics.

`SimpleTokenizer` can also be faster in many cases because it doesn't require a binary search to find the token. Instead, it can examine the first few bytes of the string directly.



---

_Label `internal` added by @MichaReiser on 2025-12-08 09:46_

---

_Label `performance` removed by @MichaReiser on 2025-12-08 09:46_

---

_Renamed from "fix: ruff_linter uses optimized paranthesized_range" to "Remove `BackwardsTokenizer` based `paranthesized_range` implementation" by @MichaReiser on 2025-12-08 09:46_

---

_@MichaReiser reviewed on 2025-12-08 09:47_

Does this allow us to remove the old `parenthesized_ranges` implementation or are there usages outside `ruff_linter`?

---

_Renamed from "Remove `BackwardsTokenizer` based `paranthesized_range` implementation" to "Remove `BackwardsTokenizer` based `parenthesized_range` implementation" by @AlexWaygood on 2025-12-08 14:35_

---

_Comment by @denyszhak on 2025-12-09 03:09_

> Does this allow us to remove the old `parenthesized_ranges` implementation or are there usages outside `ruff_linter`?

Only 1 place left: `needs_line_break` under `ruff_python_formatter` still uses the old implementation. It looks like it's possible to migrate this one as well. Would you like me to do that and remove old implementation or focus purely on ruff_linter?

---

_Comment by @MichaReiser on 2025-12-09 12:06_

> Only 1 place left: needs_line_break under ruff_python_formatter still uses the old implementation. It looks like it's possible to migrate this one as well. Would you like me to do that and remove old implementation or focus purely on ruff_linter?

Let's focus on the linter for now. 

---

_Renamed from "Remove `BackwardsTokenizer` based `parenthesized_range` implementation" to "Remove `BackwardsTokenizer` based `parenthesized_range` references in `ruff_linter`" by @MichaReiser on 2025-12-09 12:06_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/edits.rs`:303 on 2025-12-09 12:08_

Do the call sites become much more awkward if we revert this change too? 

---

_@MichaReiser reviewed on 2025-12-09 12:10_

This looks good. We mainly need to look into the regression

---

_@denyszhak reviewed on 2025-12-09 23:55_

---

_Review comment by @denyszhak on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quoted-type-alias_TC008.py.snap`:473 on 2025-12-09 23:55_

@MichaReiser 
In `quoted_type_alias`, the outer-parentheses check needs the original file tokens, but `tokens()` may point at the parsed-annotation token stream in this context. Is it acceptable to expose `source_tokens()` directly for this internal use, or is there a preferred way to access the original token stream without widening the public API?
Ultimately, all we need in this context is source token for outer check if we want to use new version, other approaches seem to be less clean. 

wdyt?

---

_@denyszhak reviewed on 2025-12-10 19:53_

---

_Review comment by @denyszhak on `crates/ruff_linter/src/rules/flake8_type_checking/snapshots/ruff_linter__rules__flake8_type_checking__tests__quoted-type-alias_TC008.py.snap`:473 on 2025-12-10 19:53_

I pushed the change for this one

---

_Review requested from @MichaReiser by @denyszhak on 2025-12-10 23:55_

---

_@MichaReiser reviewed on 2025-12-11 09:00_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs`:304 on 2025-12-11 09:00_

Oh nice find. I think exposing `source_tokens` here makes sense`

---

_@MichaReiser reviewed on 2025-12-11 09:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:440 on 2025-12-11 09:01_

```suggestion
    /// Returns the [`Tokens`] for the parsed source file.
    /// 
    /// 
    /// Unlike [`Self::tokens`], this method always returns
    /// the tokens for the current file, even when within a parsed type annotation. 
```

---

_@MichaReiser approved on 2025-12-11 09:04_

This is great, thank you.

I'll wait for https://github.com/astral-sh/ruff/pull/21895 to land first as this can otherwise lead to panics

---

_Merged by @MichaReiser on 2025-12-11 12:04_

---

_Closed by @MichaReiser on 2025-12-11 12:04_

---
