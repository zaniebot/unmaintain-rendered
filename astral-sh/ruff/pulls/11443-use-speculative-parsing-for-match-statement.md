```yaml
number: 11443
title: "Use speculative parsing for `match` statement"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/soft-keyword-match
created_at: 2024-05-16T09:34:05Z
updated_at: 2024-05-21T04:46:42Z
url: https://github.com/astral-sh/ruff/pull/11443
synced_at: 2026-01-12T15:55:38Z
```

# Use speculative parsing for `match` statement

---

_@dhruvmanila_

## Summary

This PR adds support for parsing `match` statement based on whether `match` is used as a keyword or an identifier.

The logic is as follows:
1. Use two token lookahead to classify the kind of `match` token based on the table down below
2. If we can't determine whether it's a keyword or an identifier, we'll use speculative parsing
3. Assume that `match` is a keyword and parse the subject expression
4. Then, based on certain heuristic, determine if our assumption is correct or not

For (4), the heuristics are:
1. If the current token is `:`, then it's a keyword
2. If the current token is a newline, then
	1. If the following two tokens are `Indent` and `Case`, it's a keyword
	2. Otherwise, it's an identifier
3. Otherwise, it's an identifier

In the above heuristic, the (2) condition is so that the parser can correctly identify the following as a `match` statement in case of a missing `:`:
```py
match foo
	case _:
		pass
```

### Classification table

Here, the token is the one following the `match` token. We use two token lookahead for `not in` because using the `in` keyword we can classify it as an identifier or a keyword.

Token                      | Keyword | Identifier | Either |
--                         | --      | --         | --     |
Name                       | ✅      |            |        |
Int                        | ✅      |            |        |
Float                      | ✅      |            |        |
Complex                    | ✅      |            |        |
String                     | ✅      |            |        |
FStringStart               | ✅      |            |        |
FStringMiddle              | -       | -          | -      |
FStringEnd                 | -       | -          | -      |
IpyEscapeCommand           | -       | -          | -      |
Newline                    |         | ✅         |        |
Indent                     | -       | -          | -      |
Dedent                     | -       | -          | -      |
EndOfFile                  |         | ✅         |        |
`?`                        | -       | -          | -      |
`!`                        |         | ✅         |        |
`(`                        |         |            | ✅     |
`)`                        |         | ✅         |        |
`[`                        |         |            | ✅     |
`]`                        |         | ✅         |        |
`{`                        | ✅      |            |        |
`}`                        |         | ✅         |        |
`:`                        |         | ✅         |        |
`,`                        |         | ✅         |        |
`;`                        |         | ✅         |        |
`.`                        |         | ✅         |        |
`*`                        |         |            | ✅     |
`+`                        |         |            | ✅     |
`-`                        |         |            | ✅     |
Other binary operators     |         | ✅         |        |
`~`                        | ✅      |            |        |
`not`                      | ✅      |            |        |
`not in`                   |         | ✅         |        |
Boolean operators          |         | ✅         |        |
Comparison operators       |         | ✅         |        |
Augmented assign operators |         | ✅         |        |
`...`                      | ✅      |            |        |
`lambda`                   | ✅      |            |        |
`await`                    | ✅      |            |        |
`yield`                    | ✅      |            |        |
Other keywords             |         | ✅         |        |
Soft keywords              | ✅      |            |        |
Singletons                 | ✅      |            |        |


---

_Label `parser` added by @dhruvmanila on 2024-05-20 07:21_

---

_Marked ready for review by @dhruvmanila on 2024-05-20 07:21_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-20 07:21_

---

_@charliermarsh reviewed on 2024-05-20 15:44_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/parser/statement.rs`:129 on 2024-05-20 15:44_

Is this effectively an optimization? E.g., could we _always_ use `try_parse_match_statement` and fall back when it returns `None`? 

---

_@charliermarsh approved on 2024-05-20 15:45_

---

_@dhruvmanila reviewed on 2024-05-20 15:48_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:129 on 2024-05-20 15:48_

Yes, this is an optimization because if we're sure that it's a keyword, we can skip speculative parsing. The reason being that speculative parsing has some overhead of creating the checkpoint and possibly throwing away the parsed expression if it's not a keyword.

---

_@charliermarsh reviewed on 2024-05-20 15:52_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/parser/statement.rs`:129 on 2024-05-20 15:52_

👍 Makes sense!

---

_Merged by @dhruvmanila on 2024-05-21 04:46_

---

_Closed by @dhruvmanila on 2024-05-21 04:46_

---

_Branch deleted on 2024-05-21 04:46_

---
