```yaml
number: 10554
title: Add tests for list expression parsing
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/test-list-parsing
created_at: 2024-03-25T03:16:23Z
updated_at: 2024-03-29T08:19:01Z
url: https://github.com/astral-sh/ruff/pull/10554
synced_at: 2026-01-10T22:47:02Z
```

# Add tests for list expression parsing

---

_Pull request opened by @dhruvmanila on 2024-03-25 03:16_

## Summary

This PR updates the existing valid test case for list expression and adds some invalid ones to check how the parser is recovering. It also updates the parsing function used to match the grammar.

The goal is mainly to check if the list parsing is correct or not as per the grammar.

There are test cases for unclosed bracket where the parser sometimes doesn't recover well or doesn't provide useful error messages. I think these will be improved by feeding the information back to the lexer. The lexer could then emit `Newline` where appropriate instead of `NonLogicalNewline` and the recovery set could include the `Newline` as the list terminator to recover from it.

---

_Label `parser` added by @dhruvmanila on 2024-03-25 03:16_

---

_Label `testing` added by @dhruvmanila on 2024-03-25 03:16_

---

_Comment by @dhruvmanila on 2024-03-25 12:32_

I need to do starred expression first before I can add related invalid test cases here as the parser is allowing syntax like `[1, * *name, 2]` and parsing it as nested starred expression.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1264 on 2024-03-27 09:09_

What we will be doing is to use a more general rule for the first element as we don't yet know whether it's a comprehension or a normal list. For comprehension, we'll report an error later if a starred expression is being used.

---

_@dhruvmanila reviewed on 2024-03-27 09:09_

---

_Marked ready for review by @dhruvmanila on 2024-03-27 09:10_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-27 09:10_

---

_Comment by @github-actions[bot] on 2024-03-27 09:39_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__list__missing_closing_bracket_0.py.snap`:26 on 2024-03-27 09:42_

Does the parser not generate an error message when parsing out an invalid name or is it because it de-duplicates the error messages? 

Nit: I know this PR doesn't focus on recovery but you already touched that code, so I'll bring it up anyway. I think here it would be better to **not** create an empty name because
the parser now hallucinates elements that don't exist in the source. Instead, we should check if the parser is at an expression and only then parse it out. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1453 on 2024-03-27 09:46_

Nit: We could consider keeping `parse_named_expression_or_higher` where it simply calls `self.parse_star_expression_or_higher(AllowNamedExpression::Yes)

---

_@MichaReiser approved on 2024-03-27 09:47_

---

_@MichaReiser reviewed on 2024-03-27 09:47_

Nice. I like the extensive tests. I think there's one invalid case that we could improve but we don't have to do this today. I recommend adding a TODO or creating an issue (maybe a tracking issue with all error recovery that needs improving) if you decide to improve it later

---

_@MichaReiser reviewed on 2024-03-27 09:53_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1264 on 2024-03-27 09:53_

You say that we report an error later. I wasn't able to spot the error reporting right away. Is this something you plan to add later (including tests)?

---

_@dhruvmanila reviewed on 2024-03-28 09:46_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1264 on 2024-03-28 09:46_

Yes, it was added in the list comprehension PR. Sorry, I should've made that clear.

---

_@dhruvmanila reviewed on 2024-03-28 09:48_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1453 on 2024-03-28 09:48_

I don't understand this. Both methods correspond to different grammar rule. `parse_named_expression_or_higher` is for `named_expression` which does not limit the precedence of a starred expression while `parse_star_expression_or_higher` is a either  `star_expression` or `star_named_expression` where the starred expression should have a minimum precedence of a bitwise or (`|`) operator.

---

_@dhruvmanila reviewed on 2024-03-28 09:49_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@expressions__list__missing_closing_bracket_0.py.snap`:26 on 2024-03-28 09:49_

I agree. I'll look into this for all container types (list, set, tuples, dict)

---

_@MichaReiser reviewed on 2024-03-28 10:04_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1453 on 2024-03-28 10:04_

Ah okay, that was not clear to me from reviewing the PR. I assumed it was simply a rename (because you changed all call sites but what i understand from your comment is that this isn't the case and this PR instead changes the parse rule)

---

_@dhruvmanila reviewed on 2024-03-28 10:06_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1453 on 2024-03-28 10:06_

Yes, it changes to use the correct grammar rule for list expressions.

---

_Merged by @dhruvmanila on 2024-03-29 08:03_

---

_Closed by @dhruvmanila on 2024-03-29 08:03_

---

_Branch deleted on 2024-03-29 08:03_

---

_@dhruvmanila reviewed on 2024-03-29 08:06_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1264 on 2024-03-29 08:06_

Linking for posterity: https://github.com/astral-sh/ruff/pull/10634/files#diff-b0faf8ec2615ebd31aff987f04757ead6fcfef23e995cd370e5da1700e9ae684

---
