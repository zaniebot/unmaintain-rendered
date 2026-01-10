```yaml
number: 9993
title: "Move `RUF001`, `RUF002` to AST checker"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/move-rules-to-ast-checker
created_at: 2024-02-15T07:13:25Z
updated_at: 2024-02-17T17:08:02Z
url: https://github.com/astral-sh/ruff/pull/9993
synced_at: 2026-01-10T22:57:09Z
```

# Move `RUF001`, `RUF002` to AST checker

---

_Pull request opened by @dhruvmanila on 2024-02-15 07:13_

## Summary

Part of #7595 

This PR moves the `RUF001` and `RUF002` rules to the AST checker. This removes the use of docstring detection from these rules.

## Test Plan

As this is just a refactor, make sure existing test cases pass.


---

_Label `internal` added by @dhruvmanila on 2024-02-15 07:13_

---

_Comment by @codspeed-hq[bot] on 2024-02-15 07:20_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/move-rules-to-ast-checker)

### Merging #9993 will **improve performances by 4.32%**

<sub>Comparing <code>dhruv/move-rules-to-ast-checker</code> (932f802) with <code>main</code> (d46c5d8)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/move-rules-to-ast-checker` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 10.2 ms | 9.8 ms | +4.32% |


---

_Comment by @github-actions[bot] on 2024-02-15 07:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/63dc0f76faa208450b8aaa57246029fcf94d015b/pandas/io/stata.py#L1616'>pandas/io/stata.py:1616:9:</a> PLR0917 Too many positional arguments (8/5)
- <a href='https://github.com/pandas-dev/pandas/blob/63dc0f76faa208450b8aaa57246029fcf94d015b/pandas/io/stata.py#L1616'>pandas/io/stata.py:1616:9:</a> PLR0917 Too many positional arguments (8/5)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:199 on 2024-02-15 08:15_

I think we could reduce the arguments necessary to pass to the function when the function returns the Vec and we move the generation of the diagnostics into this function 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/tokens.rs`:70 on 2024-02-15 08:16_

Could we use comment ranges here to avoid iterating over all tokens only to find the comment tokens?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:196 on 2024-02-15 08:18_

Nit: I think you can simplify this by early returning if it is a byte literal and you can then operate directly on the string like (no need for the branching)

---

_@MichaReiser approved on 2024-02-15 08:18_

---

_@dhruvmanila reviewed on 2024-02-15 11:32_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:199 on 2024-02-15 11:32_

Do you mean that we allocate a vector of diagnostics inside the function and then pass it outside to be extended onto the global diagnostics vector?

---

_@dhruvmanila reviewed on 2024-02-15 11:37_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:196 on 2024-02-15 11:37_

Hmm, yeah. I could probably use `chain` and combine it into a single call (probably not what you meant).

---

_@dhruvmanila reviewed on 2024-02-15 11:40_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:196 on 2024-02-15 11:40_

Or do you mean to take the range of the entire `string_like`? But that would include implicitly concatenated strings.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:196 on 2024-02-15 11:50_

Argh, is it the whole string literal confusion all over again. What confuses me is that `FStringLiteral` has no implicit concatenation but `StringLiteral` has... It feels like having both `StringLiteral` and `BytesLiteral` at the same level as `FStringLiteral` might have been a mistake because it gruops elements that are at different levels. 

But yes, in this case this won't work except if `StringLike` would have a method to iterate over all values.

---

_@MichaReiser reviewed on 2024-02-15 11:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:230 on 2024-02-15 11:54_

Unrelated to your changes but I think the `word_candidates.clear` call at the end is unnecessary because we'll drop the vec anyway

---

_@MichaReiser reviewed on 2024-02-15 11:54_

---

_@dhruvmanila reviewed on 2024-02-15 11:55_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:196 on 2024-02-15 11:55_

> I don't understand the remark on how it would include implicitly concatenated strings since we're operating on string literal's.

Because 😅:

```rust
pub enum StringLike<'a> {
    StringLiteral(&'a ast::ExprStringLiteral),
    BytesLiteral(&'a ast::ExprBytesLiteral),
    FStringLiteral(&'a ast::FStringLiteralElement),
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:199 on 2024-02-15 11:56_

The idea was to return the `word_candidates` vec if `word_flags.is_candidate_word()` and otherwise return an empty `Vec`. You can then generate the `Diagnostics` in the call site. The downside is that it will result in some duplication. I wasn't aware that we call this methods for comments too.

---

_@MichaReiser reviewed on 2024-02-15 11:56_

---

_@dhruvmanila reviewed on 2024-02-15 11:58_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:196 on 2024-02-15 11:58_

> Argh, is it the whole string literal confusion all over again.

Yeah, sorry 😓. I'll try to sort out at least the name confusion over the weekend.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:230 on 2024-02-17 13:06_

I think I just included it for simplicity in case anything got copy-pasted or re-ordered.

---

_@charliermarsh reviewed on 2024-02-17 13:06_

---

_@dhruvmanila reviewed on 2024-02-17 16:54_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/ambiguous_unicode_character.rs`:199 on 2024-02-17 16:54_

I see. I'll continue as is for now because of how `StringLike` enum is. We can revisit later.

---

_Merged by @dhruvmanila on 2024-02-17 17:01_

---

_Closed by @dhruvmanila on 2024-02-17 17:01_

---

_Branch deleted on 2024-02-17 17:01_

---
