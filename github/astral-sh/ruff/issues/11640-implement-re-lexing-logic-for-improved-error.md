---
number: 11640
title: Implement re-lexing logic for improved error recovery
type: issue
state: closed
author: dhruvmanila
labels:
  - parser
assignees: []
created_at: 2024-05-31T14:29:12Z
updated_at: 2024-06-17T06:47:02Z
url: https://github.com/astral-sh/ruff/issues/11640
synced_at: 2026-01-07T13:12:15-06:00
---

# Implement re-lexing logic for improved error recovery

---

_Issue opened by @dhruvmanila on 2024-05-31 14:29_

https://github.com/astral-sh/ruff/pull/11457 builds up the necessary infrastructure to completely synchronize the lexer and parser. The final step is to implement re-lexing logic in the lexer and update the parser to use it during error recovery for list parsing.

Internal document: https://www.notion.so/astral-sh/Lexer-Parser-feedback-loop-dcd653ea94d64629a388530f13393424

---

_Label `parser` added by @dhruvmanila on 2024-05-31 14:29_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-05-31 14:29_

---

_Referenced in [astral-sh/ruff#11845](../../astral-sh/ruff/pulls/11845.md) on 2024-06-12 17:42_

---

_Closed by @dhruvmanila on 2024-06-17 06:47_

---
