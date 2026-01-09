---
number: 11946
title: Improve f-string error recovery within list parsing
type: issue
state: open
author: dhruvmanila
labels:
  - parser
assignees: []
created_at: 2024-06-20T03:33:50Z
updated_at: 2024-06-20T03:56:03Z
url: https://github.com/astral-sh/ruff/issues/11946
synced_at: 2026-01-07T13:12:15-06:00
---

# Improve f-string error recovery within list parsing

---

_Issue opened by @dhruvmanila on 2024-06-20 03:33_

F-string error recovery within list parsing is good but it breaks when it's within a parenthesized context.

For example,
```py
bad_function_call(
    param1=f'test
    param2='test'
)
```

The re-lexing will reduce the nesting level when recovering from an unclosed f-string but the lexer isn't in an expression element so the nesting level it reduced is the one from the outer context. This means that it'll emit an `Indent` token before `param2` which is incorrect.

Another example is when the parser is recovering from within a format spec,
```py
bad_function_call(
    param1=f'test{x:.3f,
    param2='test'
)
```

Here, the parser uses the same list parsing logic for parsing both the outer f-string elements and the ones in the format specifier. This means that when it tries to recover from an unclosed f-string, it reduces the nesting level twice (1) when re-lexing from the format spec and (2) when re-lexing from the outer f-string.

### Todo

- [ ] Reduce the nesting level only if the parser is recovering from within an expression element of an f-string (`fstring.is_in_expression`)
- [ ] Avoid calling into re-lexing when recovering from the format specifier. Let the recovery from the outer context (f-string) take care of that.

---

_Label `parser` added by @dhruvmanila on 2024-06-20 03:34_

---

_Comment by @dhruvmanila on 2024-06-20 03:56_

(1) is difficult because by the time we're re-lexing, the f-string context is no longer on the stack because the lexer removed it when encountering a newline character. Another potential reason to do https://github.com/astral-sh/ruff/issues/11916.

---

_Referenced in [astral-sh/ruff#12004](../../astral-sh/ruff/issues/12004.md) on 2024-06-24 06:45_

---

_Referenced in [astral-sh/ruff#21895](../../astral-sh/ruff/pulls/21895.md) on 2025-12-11 05:36_

---

_Referenced in [astral-sh/ruff#21896](../../astral-sh/ruff/issues/21896.md) on 2025-12-11 09:30_

---
