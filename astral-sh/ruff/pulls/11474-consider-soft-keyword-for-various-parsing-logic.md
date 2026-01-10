```yaml
number: 11474
title: Consider soft keyword for various parsing logic
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/at-soft-keyword
created_at: 2024-05-20T10:39:06Z
updated_at: 2024-05-27T16:00:29Z
url: https://github.com/astral-sh/ruff/pull/11474
synced_at: 2026-01-10T22:05:26Z
```

# Consider soft keyword for various parsing logic

---

_Pull request opened by @dhruvmanila on 2024-05-20 10:39_

## Summary

This PR updates various checks to consider soft keywords.

1. Update the following methods to also check whether the parser is at a soft keyword token:
	1. `at_simple_stmt`
	2. `at_stmt`
	3. `at_expr`
	4. `at_pattern_start`
	5. `at_mapping_pattern_start`
2. Update various references of `parser.at(TokenKind::Name)` to use `parser.at_name_or_soft_keyword`. In the future, we should update it to also include other keywords on a case-by-case basis.
3. Re-use `parse_name` and `parse_identifier` for literal pattern parsing. We should always use either of these methods instead of doing the same manually to make sure that soft keyword are correctly tranformed.

For (i) and (ii), we could've just extended the `EXPR_SET` with the soft keyword tokens but I think it's better to be explicit about what the method checks and to have separation between the token set corresponding to statement and soft keywords.

## Test Plan

Add a few test cases and update the snapshots. These snapshots were generated through the local fork of the parser which compiles.


---

_Marked ready for review by @dhruvmanila on 2024-05-20 11:29_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-20 11:29_

---

_Converted to draft by @dhruvmanila on 2024-05-21 02:50_

---

_Comment by @dhruvmanila on 2024-05-21 02:51_

(Converting this to draft as there are a few more references to be looked at.)

---

_Renamed from "Consider soft keyword for expr / pattern parsing" to "Consider soft keyword for various parsing logic" by @dhruvmanila on 2024-05-21 03:47_

---

_Label `parser` added by @dhruvmanila on 2024-05-21 05:26_

---

_Marked ready for review by @dhruvmanila on 2024-05-21 05:26_

---

_Comment by @dhruvmanila on 2024-05-21 05:26_

This is ready to review.

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-05-21 05:26_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:1055 on 2024-05-27 12:20_

I think I would just call `p.at_name_or_soft_keyword` `p.at_name` to align it with `parse_name`. The downside is that this can be confusing because there's also a `Name` token. Could we rename `parse_name` to `parse_identifier`?

---

_@MichaReiser approved on 2024-05-27 12:21_

---

_@dhruvmanila reviewed on 2024-05-27 15:58_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:1055 on 2024-05-27 15:58_

> I think I would just call `p.at_name_or_soft_keyword` `p.at_name` to align it with `parse_name`. The downside is that this can be confusing because there's also a `Name` token.

Yeah, I thought of doing that but I found the explicit name better just to avoid this confusion.

> Could we rename `parse_name` to `parse_identifier`?

They both exists because sometimes the parser needs to construct an `ExprName` node while other times it only needs an `Identifier`.

---

_Merged by @dhruvmanila on 2024-05-27 16:00_

---

_Closed by @dhruvmanila on 2024-05-27 16:00_

---

_Branch deleted on 2024-05-27 16:00_

---
