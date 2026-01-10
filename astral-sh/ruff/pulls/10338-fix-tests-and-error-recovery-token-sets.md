```yaml
number: 10338
title: Fix tests and error recovery token sets
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/fix-tests
created_at: 2024-03-11T13:23:24Z
updated_at: 2024-03-12T08:11:16Z
url: https://github.com/astral-sh/ruff/pull/10338
synced_at: 2026-01-10T22:47:01Z
```

# Fix tests and error recovery token sets

---

_Pull request opened by @dhruvmanila on 2024-03-11 13:23_

## Summary

This PR fixes the failing CI tests from the original PR. The changes have been highlighted using PR comments to have the proper context.

## Test Plan

`cargo test`


---

_Label `parser` added by @dhruvmanila on 2024-03-11 13:23_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-11 13:23_

---

_Converted to draft by @dhruvmanila on 2024-03-11 13:23_

---

_@dhruvmanila reviewed on 2024-03-11 13:29_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/tests/parser.rs`:502 on 2024-03-11 13:29_

The new parser now raises a syntax error for invalid delete target. This is also a hard syntax error in the CPython parser.

There could be an argument to make this a "soft" syntax error. I think I'd go through all of the hard syntax errors the new parser will raise and decide if it should actually be a "soft" syntax error before merging the main PR.

---

_@dhruvmanila reviewed on 2024-03-11 13:29_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:565 on 2024-03-11 13:29_

Silly mistake, the node should always start _before_ we move the parser any further ahead.

---

_@dhruvmanila reviewed on 2024-03-11 13:30_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:882 on 2024-03-11 13:30_

For slices specifically, an element can start with a `:` e.g., `data[1, :2]`

---

_@dhruvmanila reviewed on 2024-03-11 13:31_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:895 on 2024-03-11 13:31_

Forgot about the positional argument separator e.g., `def foo(a, b, /): ...`

---

_@dhruvmanila reviewed on 2024-03-11 13:32_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:846 on 2024-03-11 13:32_

I think we can just use the `at_sequence_end` even for parenthesized tuple which might help in error recovery. I'll need to test this out further but it shouldn't block this PR.

---

_@dhruvmanila reviewed on 2024-03-12 04:15_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/tests/snapshots/format@empty_multiple_trailing_newlines.py.snap`:15 on 2024-03-12 04:15_

I think I forgot to update some snapshots before merging earlier PRs. Sorry.

---

_@dhruvmanila reviewed on 2024-03-12 04:15_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:94 on 2024-03-12 04:15_

This is just moving the function up top with other `at_*` functions.

---

_@dhruvmanila reviewed on 2024-03-12 04:16_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__function_type_parameters.py.snap`:408 on 2024-03-12 04:16_

This is not an ideal recovery but it's happening because we removed `skip_until`. Here's an explanation of why's that the case (from Discord):

---

Removing `skip_while` has some consequences. I was trying to figure out why the type parameter snapshot is changing for the following code:

```py
def foo[A,,100](): ...
```

In the original PR, the parser would recover correctly and provide a proper AST containing the function node which didn't have 100 as a type parameter but now the `(): ...` is parsed as an annotated assignment statement.

So, our recovery context when the parser is trying to parse a list of type parameters:
- Module statements
  - Type parameters

Now, when the parser sees `100`, which is _not_ the start of a type parameter (not in FIRST set), it checks if it can be recovered via an outer context. The outer context being module statement, it can because `100` is a start of a statement, specifically a simple statement. This means that the parser is at `100` in module statement context but as there's no semicolon, it'll create an error (as expected).

The problem comes when we're trying to parse the function parameters. Earlier there was a use of `skip_while` if a token was found which is not a parameter then we'd skip until the end of parameter list. But now as we don't do it, we move ahead being in the context of module statement and parse `]` as error because we expect a statement and then `(): ...` as annotated assign statement.

This is correct but I might look into statement parsing to see if it can be improved.

---

_@dhruvmanila reviewed on 2024-03-12 04:17_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__named_expression_statement.py.snap`:48 on 2024-03-12 04:17_

Sorry, forgot to update the snapshot from my earlier PR.

---

_Renamed from "Fix error recovery token sets" to "Fix tests and error recovery token sets" by @dhruvmanila on 2024-03-12 04:17_

---

_Marked ready for review by @dhruvmanila on 2024-03-12 04:17_

---

_Merged by @dhruvmanila on 2024-03-12 08:11_

---

_Closed by @dhruvmanila on 2024-03-12 08:11_

---

_Branch deleted on 2024-03-12 08:11_

---
