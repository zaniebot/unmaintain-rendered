```yaml
number: 11770
title: Use speculative parsing for with-items
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: dhruv/speculative-with-items-parsing
created_at: 2024-06-06T04:48:18Z
updated_at: 2024-06-06T11:45:35Z
url: https://github.com/astral-sh/ruff/pull/11770
synced_at: 2026-01-12T15:55:38Z
```

# Use speculative parsing for with-items

---

_@dhruvmanila_

## Summary

This PR updates the with-items parsing logic to use speculative parsing instead.

### Existing logic

First, let's understand the previous logic:
1. The parser sees `(`, it doesn't know whether it's part of a parenthesized with items or a parenthesized expression
2. Consider it a parenthesized with items and perform a hand-rolled speculative parsing
3. Then, verify the assumption and if it's incorrect convert the parsed with items into an appropriate expression which becomes part of the first with item

Here, in (3) there are lots of edge cases which we've to deal with:
1. Trailing comma with a single element should be [converted to the expression as is](https://github.com/astral-sh/ruff/blob/9b2cf569b22855439fa916be6fc417b372074f42/crates/ruff_python_parser/src/parser/statement.rs#L2140-L2153)
2. Trailing comma with multiple elements should be [converted to a tuple expression](https://github.com/astral-sh/ruff/blob/9b2cf569b22855439fa916be6fc417b372074f42/crates/ruff_python_parser/src/parser/statement.rs#L2155-L2178)
3. Limit the allowed expression based on whether it's [(1)](https://github.com/astral-sh/ruff/blob/9b2cf569b22855439fa916be6fc417b372074f42/crates/ruff_python_parser/src/parser/statement.rs#L2144-L2152) or [(2)](https://github.com/astral-sh/ruff/blob/9b2cf569b22855439fa916be6fc417b372074f42/crates/ruff_python_parser/src/parser/statement.rs#L2157-L2171)
4. [Consider postfix expressions](https://github.com/astral-sh/ruff/blob/9b2cf569b22855439fa916be6fc417b372074f42/crates/ruff_python_parser/src/parser/statement.rs#L2181-L2200) after (3)
5. [Consider `if` expressions](https://github.com/astral-sh/ruff/blob/9b2cf569b22855439fa916be6fc417b372074f42/crates/ruff_python_parser/src/parser/statement.rs#L2203-L2208) after (3)
6. [Consider binary expressions](https://github.com/astral-sh/ruff/blob/9b2cf569b22855439fa916be6fc417b372074f42/crates/ruff_python_parser/src/parser/statement.rs#L2210-L2228) after (3)

Consider other cases like
* [Single generator expression](https://github.com/astral-sh/ruff/blob/9b2cf569b22855439fa916be6fc417b372074f42/crates/ruff_python_parser/src/parser/statement.rs#L2020-L2035)
* [Expecting a comma](https://github.com/astral-sh/ruff/blob/9b2cf569b22855439fa916be6fc417b372074f42/crates/ruff_python_parser/src/parser/statement.rs#L2122-L2130)

And, this is all possible only if we allow parsing these expressions in the [with item parsing logic](https://github.com/astral-sh/ruff/blob/9b2cf569b22855439fa916be6fc417b372074f42/crates/ruff_python_parser/src/parser/statement.rs#L2287-L2334).

### Speculative parsing

With #11457 merged, we can simplify this logic by changing the step (3) from above to just rewind the parser back to the `(` if our assumption (parenthesized with-items) was incorrect and then continue parsing it considering parenthesized expression.

This also behaves a lot similar to what a PEG parser does which is to consider the first grammar rule and if it fails consider the second grammar rule and so on.

resolves: #11639 

## Test Plan

- [x] Verify the updated snapshots
- [x] Run the fuzzer on around 3000 valid source code (locally)


---

_Label `internal` added by @dhruvmanila on 2024-06-06 04:48_

---

_Label `parser` added by @dhruvmanila on 2024-06-06 04:48_

---

_Comment by @github-actions[bot] on 2024-06-06 05:07_

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

_Marked ready for review by @dhruvmanila on 2024-06-06 06:27_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-06 06:27_

---

_@dhruvmanila reviewed on 2024-06-06 06:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__with__ambiguous_lpar_with_items.py.snap`:1441 on 2024-06-06 06:38_

The problem here is that the speculative parsing failed so we fallback to considering parenthesized expression. The terminator token for with-items parsing with the kind being parenthesized expression is `:`, so it'll expect a `,` after a with item (`item`) but not at the end (`:`).

I think I want to improve the logic of parsing comma-separated list from `(element comma) (element comma) ...` to `(element) (comma element) (comma element) ...`.

---

_@dhruvmanila reviewed on 2024-06-06 06:38_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@with_items_parenthesized_missing_colon.py.snap`:60 on 2024-06-06 06:38_

This is an improvement: https://play.ruff.rs/3c1fd581-e5df-4aa4-8528-7550e6d63e4f

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__with__ambiguous_lpar_with_items.py.snap`:1367 on 2024-06-06 06:41_

I haven't included this logic (https://github.com/astral-sh/ruff/blob/9b2cf569b22855439fa916be6fc417b372074f42/crates/ruff_python_parser/src/parser/statement.rs#L1910-L1940) which took care of raising "expected an expression" error. It's an edge case where we would raise a different error while in the following we'd raise "trailing comma not allowed".

```py
with (item1, item2), item3,: ...
```

---

_@dhruvmanila reviewed on 2024-06-06 06:41_

---

_@dhruvmanila reviewed on 2024-06-06 06:46_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__with__ambiguous_lpar_with_items.py.snap`:1367 on 2024-06-06 06:46_

I couldn't find a simple way to do it and I'm not sure if it's worth it. The "trailing comma" error is raised by list parsing while the "expect an expression" is raised by the with-item parsing logic. Even if I'm able to add the logic, both errors will be displayed because they're raised at separate location but they both contradict each other. So, we should only raise one of them.

---

_@MichaReiser reviewed on 2024-06-06 07:27_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__with__ambiguous_lpar_with_items.py.snap`:1367 on 2024-06-06 07:27_

I think both errors are equally good and ask the same fo the user. Remove the comma or add an expression.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__with__unclosed_ambiguous_lpar.py.snap`:29 on 2024-06-06 07:29_

This seems better :)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__with__unclosed_ambiguous_lpar_eof.py.snap`:18 on 2024-06-06 07:30_

It seems that this PR changed whether the with item includes the range of the parentheses. Is this expected?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@with_items_parenthesized_missing_comma.py.snap`:246 on 2024-06-06 07:31_

The old parse tree here was probably slightly better. 

---

_@MichaReiser approved on 2024-06-06 07:37_

Nice! I find the new code much easier to reason about! 

The only thought I have is if we should restrict error recovery while we're doing speculative parsing to avoid that the error recovery breaks our validation if the with items is what we think it is... I do think it isn't necessary because the error recovery never eats over a character that we use to determine whether our assumption was correct (`)` or `:` both exit the recovery logic). 

Edit: ~~I recommend you to run the fuzzer for a while in the background. It proved useful last time ;)~~ 
I should read the summary more carefully. You already did that.

---

_@dhruvmanila reviewed on 2024-06-06 08:36_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__with__unclosed_ambiguous_lpar_eof.py.snap`:18 on 2024-06-06 08:36_

Yes. This is for the following code:
```py
with (
```

Here, the speculative parsing fails and thus it becomes a parenthesized expression.

---

_@dhruvmanila reviewed on 2024-06-06 08:52_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@with_items_parenthesized_missing_comma.py.snap`:246 on 2024-06-06 08:52_

Yeah, missing closing parenthesis is difficult to recover from because it's occurring in list parsing which skips over the `:` token.

---

_Merged by @dhruvmanila on 2024-06-06 08:59_

---

_Closed by @dhruvmanila on 2024-06-06 08:59_

---

_Branch deleted on 2024-06-06 08:59_

---

_@MichaReiser reviewed on 2024-06-06 09:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@with_items_parenthesized_missing_comma.py.snap`:246 on 2024-06-06 09:08_

Should the recovery skip over the `:`? Shouldn't it stop when seeing a `:` because it is part of the with items recovery set?

---

_@dhruvmanila reviewed on 2024-06-06 09:20_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@with_items_parenthesized_missing_comma.py.snap`:246 on 2024-06-06 09:20_

For the initial assumption of parenthesized with-items, the `:` is not part of the recovery set (maybe it should?) and so we don't stop at `:` which means we can't reliably detect whether our assumption was correct. This is why it falls back to parenthesized expression and parsing it as a tuple expression.

---

_@MichaReiser reviewed on 2024-06-06 10:22_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@with_items_parenthesized_missing_comma.py.snap`:246 on 2024-06-06 10:22_

Yeah, i think it probably should. Or we risk running over the end of the case header

---

_@dhruvmanila reviewed on 2024-06-06 11:45_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@with_items_parenthesized_missing_comma.py.snap`:246 on 2024-06-06 11:45_

Done in https://github.com/astral-sh/ruff/pull/11775

---
