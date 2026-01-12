```yaml
number: 21023
title: "ISC001 panic: begin <= end (1 <= 0) when slicing `'`"
type: issue
state: closed
author: correctmost
labels:
  - bug
assignees: []
created_at: 2025-10-21T18:01:32Z
updated_at: 2025-10-28T19:14:59Z
url: https://github.com/astral-sh/ruff/issues/21023
synced_at: 2026-01-12T15:54:57Z
```

# ISC001 panic: begin <= end (1 <= 0) when slicing `'`

---

_@correctmost_

### Summary

`ruff check --select=ISC001 s.py` panics with the following code:

**s.py**
```python
'' '
```

Output:
```
panic: Panicked at crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs:200:25 when checking `/tmp/ruff/s.py`: `begin <= end (1 <= 0) when slicing `'``
--> s.py:1:1
```

Version info:
- Python 3.13.7 on Arch Linux

### Version

ruff 0.14.1

---

_Comment by @ntBre on 2025-10-21 18:56_

Thank you for the report and such a minimal example!

This bisects to #20848, but I'm a bit surprised that we run any rules when there's a parse error:

```
> uvx ruff@0.14.1 check - <<<"'' '"
invalid-syntax: missing closing quote in string literal
 --> -:1:4
  |
1 | '' '
  |    ^
  |

Found 1 error.
```

But I guess we only skip the AST-based checks instead of token-based checks like ISC001.

https://github.com/astral-sh/ruff/blob/e18ead10e16e2945bd81330e4732a372321a802a/crates/ruff_linter/src/linter.rs#L192-L193

Maybe we need to skip these too? Otherwise it seems like there could be a lot of surprising errors like this.

---

_Label `bug` added by @ntBre on 2025-10-21 18:56_

---

_Comment by @TaKO8Ki on 2025-10-21 20:53_

I'll take this one.

---

_Comment by @MichaReiser on 2025-10-22 06:15_

Yeah, I suspected that this one is on me when I saw the error message. 

> Maybe we need to skip these too? Otherwise it seems like there could be a lot of surprising errors like this.

Our long-term goal is to do the opposite. Run all checks (maybe with fixes disabled) when there are syntax errors. One motivation for this is that it's very annoying when lints "flicker" as you type because they disappear and reappear, even in places that are entirely unrelated to the changed code. 

> I'll take this one.

Thank you. 

---

_Closed by @MichaReiser on 2025-10-28 19:14_

---
