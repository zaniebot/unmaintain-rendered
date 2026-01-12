```yaml
number: 10641
title: Add tests for parenthesized expressions, tuple, generator
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/test-parenthesized
created_at: 2024-03-28T04:14:04Z
updated_at: 2024-03-29T08:42:38Z
url: https://github.com/astral-sh/ruff/pull/10641
synced_at: 2026-01-12T15:55:32Z
```

# Add tests for parenthesized expressions, tuple, generator

---

_@dhruvmanila_

## Summary

This PR adds test cases for parenthesized expressions, tuple expressions and generators.

---

_Label `parser` added by @dhruvmanila on 2024-03-28 04:14_

---

_Label `testing` added by @dhruvmanila on 2024-03-28 04:14_

---

_Comment by @codspeed-hq[bot] on 2024-03-28 04:19_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/test-parenthesized)

### Merging #10641 will **not alter performance**

<sub>Comparing <code>dhruv/test-parenthesized</code> (9ee1142) with <code>dhruv/parser</code> (cf6fb75)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@dhruvmanila reviewed on 2024-03-28 07:01_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1381 on 2024-03-28 07:01_

This isn't being used anywhere.

---

_@dhruvmanila reviewed on 2024-03-28 07:05_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1453 on 2024-03-28 07:05_

This is for `(*expr)` which isn't allowed anywhere. This is the reason we check for whether the starred expression is parenthesized or not in the previous diff chunk where we raise `IterableUnpackingInComprehension`. 

---

_Marked ready for review by @dhruvmanila on 2024-03-28 07:07_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-28 07:07_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/resources/invalid/expressions/parenthesized/missing_closing_paren_1.py`:2 on 2024-03-28 08:17_

Should it say, is considered to be **parenthesized**?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1381 on 2024-03-28 08:20_

Can we remove `ParserCtxFlags::PARENTHESIZED_EXPR` too or is that used somehwere? If so, is it just that this rule doesn't exercise the code path where `PARENTHESIZED_EXPR` is used?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__parenthesized__parenthesized.py.snap`:74 on 2024-03-28 08:25_

You might want to move this into its own file (or add another test in a seperate file to test both the recovery and the unparenthesized named expression case) because it seems that the actual and expected diagnostics don't.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__parenthesized__tuple.py.snap`:286 on 2024-03-28 08:27_

Future: I think this's worth improving by using the tuple parsing if 
* Not at a `)`
* Not at a `,`
* At the start of an expression

---

_@MichaReiser approved on 2024-03-28 08:29_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1381 on 2024-03-28 09:15_

Yes, I'll remove it. I missed that.

---

_@dhruvmanila reviewed on 2024-03-28 09:15_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__parenthesized__parenthesized.py.snap`:74 on 2024-03-28 09:16_

Wait, it should say "Unparenthesized named expression is _not_ allowed"

---

_@dhruvmanila reviewed on 2024-03-28 09:16_

---

_@dhruvmanila reviewed on 2024-03-29 08:28_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__parenthesized__parenthesized.py.snap`:74 on 2024-03-29 08:28_

Actually, this is correct. It's just the way parser is recovering. It's similar to what we talked about yesterday for `yield` expression. Here, the grammar doesn't allow named expression at the statement level which means the first statement is `x` and the parser moves on to the next statement. It's starting with `:=` which cannot start any statement. 

I'll add this to the error recovery tracking issue. It is easy to solve, just that we need to be careful. The solution would to _allow_ named expression and check if it's parenthesized or not.

---

_Merged by @dhruvmanila on 2024-03-29 08:34_

---

_Closed by @dhruvmanila on 2024-03-29 08:34_

---

_Branch deleted on 2024-03-29 08:34_

---

_Comment by @github-actions[bot] on 2024-03-29 08:42_

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
