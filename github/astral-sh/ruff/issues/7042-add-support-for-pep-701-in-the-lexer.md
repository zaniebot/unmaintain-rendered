---
number: 7042
title: Add support for PEP 701 in the lexer
type: issue
state: closed
author: dhruvmanila
labels:
  - parser
  - python312
assignees: []
created_at: 2023-09-01T13:28:39Z
updated_at: 2023-09-15T07:30:16Z
url: https://github.com/astral-sh/ruff/issues/7042
synced_at: 2026-01-07T13:12:15-06:00
---

# Add support for PEP 701 in the lexer

---

_Issue opened by @dhruvmanila on 2023-09-01 13:28_

The task is to update the lexer to emit the new tokens for PEP 701: `FStringStart`, `FStringMiddle` and `FStringEnd`. Along with these, a new token `Exclamation` needs to be added for conversion flag (`f"{foo!s}"`) as it's now part of the expression.

Some of the error handling which was previously done in the parser will need to be moved into the lexer.
- `UnterminatedString`
- `UnterminatedTripleQuotedString`
- `UnclosedLbrace` - special case as [implemented in the CPython tokenizer](https://github.com/python/cpython/blob/24b9bdd6eaf0b04667b6cd4c8154f7c7c10c2398/Parser/tokenizer.c#L2444-L2455)
- `SingleRbrace`

---

_Referenced in [astral-sh/ruff#6502](../../astral-sh/ruff/issues/6502.md) on 2023-09-01 13:28_

---

_Renamed from "Update the lexer to emit new tokens (, , )" to "Add support for PEP 701 in the lexer" by @dhruvmanila on 2023-09-01 13:29_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-09-01 13:32_

---

_Referenced in [astral-sh/ruff#6659](../../astral-sh/ruff/pulls/6659.md) on 2023-09-01 13:33_

---

_Label `parser` added by @dhruvmanila on 2023-09-01 13:34_

---

_Label `python312` added by @dhruvmanila on 2023-09-01 13:34_

---

_Referenced in [astral-sh/ruff#7043](../../astral-sh/ruff/issues/7043.md) on 2023-09-01 13:37_

---

_Closed by @dhruvmanila on 2023-09-15 07:30_

---
