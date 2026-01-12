```yaml
number: 11401
title: "☂️ Remove dependencies of `Tok` from downstream tools"
type: issue
state: closed
author: dhruvmanila
labels:
  - tracking
assignees: []
created_at: 2024-05-13T09:28:45Z
updated_at: 2024-05-31T07:15:16Z
url: https://github.com/astral-sh/ruff/issues/11401
synced_at: 2026-01-12T15:54:51Z
```

# ☂️ Remove dependencies of `Tok` from downstream tools

---

_@dhruvmanila_

This issue is to keep track of all the tasks required to remove the `Tok` dependency from the downstream tools (linter and formatter).

**Notes:**
* Certain tasks might need to be clubbed with the parser changes
* The implementation logic for certain usages isn't finalized yet

### Linter

- [x] Move these rules to the AST checker:
  - [x] `PLE1300`, `PLE1307` (https://github.com/astral-sh/ruff/pull/11406)
  - [x] `UP031`
  - [x] `W605` (https://github.com/astral-sh/ruff/pull/11402)
- [x] `doc_lines_from_tokens` (https://github.com/astral-sh/ruff/pull/11418)
- [x] Following rules can be updated to directly use `TokenKind` (via https://github.com/astral-sh/ruff/pull/11361):
  - [x] `COM812`, `COM818`, `COM819` (https://github.com/astral-sh/ruff/pull/11420)
  - [x] `E301` .. `E306` (blank line rules) (https://github.com/astral-sh/ruff/pull/11419)
  - [x] `E701` .. `E703` (https://github.com/astral-sh/ruff/pull/11420)
  - [x] `ISC001`, `ISC002` (https://github.com/astral-sh/ruff/pull/11420)
  - [x] `PLE2510`, `PLE2512` .. `PLE2515` (https://github.com/astral-sh/ruff/pull/11420)
  - [x] `UP034` (https://github.com/astral-sh/ruff/pull/11424)
  - [x] `W391` (https://github.com/astral-sh/ruff/pull/11420)
- [x] `remove_import_members` uses `lex` method (https://github.com/astral-sh/ruff/pull/11511)
- [x] `extract_noqa_line_for` uses the `kind` information from the string token
- [x] `locate_cmp_ops` uses the `lex` method (https://github.com/astral-sh/ruff/pull/11562)
- [x] `tokens_and_ranges` extracts the comment ranges from the token stream
- [x] `unsorted_dunder_all` and `unsorted_dunder_slots` extracts the value from the string token (https://github.com/astral-sh/ruff/pull/11511)
- [x] `UP032` uses the `lex_starts_at` method (https://github.com/astral-sh/ruff/pull/11511)
- [x] `I001` (`trailing_comma`) uses the `lex_starts_at` method (https://github.com/astral-sh/ruff/pull/11511)
- [x] `Stylist` extracts the indentation and quote information from the token (https://github.com/astral-sh/ruff/pull/11592)
- [x] `Indexer` extracts trivia and string ranges, line continuation start value (https://github.com/astral-sh/ruff/pull/11592)

### Formatter

- [x] Move away from `lex_starts_at` in `write_suppressed_statements_starting_with_trailing_comment` (https://github.com/astral-sh/ruff/pull/11515)

Internal document: https://www.notion.so/astral-sh/Downstream-work-items-551b86e104a34054b7192675550a6c25?pvs=4

---

_Label `tracking` added by @dhruvmanila on 2024-05-13 09:28_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-05-13 09:28_

---

_Comment by @dhruvmanila on 2024-05-31 04:52_

Closed by #11628 

---

_Closed by @dhruvmanila on 2024-05-31 04:52_

---
