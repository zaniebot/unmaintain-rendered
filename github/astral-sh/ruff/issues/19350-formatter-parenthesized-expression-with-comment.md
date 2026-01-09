---
number: 19350
title: "Formatter: Parenthesized expression with comment on right hand side in assignment produces invalid syntax"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
created_at: 2025-07-15T10:58:20Z
updated_at: 2025-11-17T15:11:37Z
url: https://github.com/astral-sh/ruff/issues/19350
synced_at: 2026-01-07T13:12:16-06:00
---

# Formatter: Parenthesized expression with comment on right hand side in assignment produces invalid syntax

---

_Issue opened by @MichaReiser on 2025-07-15 10:58_

> ```python
> variable = (
>     (something)  # a comment
>     .fist_method("some string")
> )
> ```
> gets reformatted to
> ```python
> variable = (something)  # a comment
> .fist_method("some string")
> ```
> which is a syntax error, so there's a bug in ruff here.
>  

 _Originally posted by @olofahlen in [#8598](https://github.com/astral-sh/ruff/issues/8598#issuecomment-3072991517)_

---

_Label `bug` added by @MichaReiser on 2025-07-15 10:58_

---

_Label `formatter` added by @MichaReiser on 2025-07-15 10:58_

---

_Referenced in [astral-sh/ruff#20418](../../astral-sh/ruff/pulls/20418.md) on 2025-09-15 14:53_

---

_Referenced in [astral-sh/ruff#20482](../../astral-sh/ruff/issues/20482.md) on 2025-09-19 13:37_

---

_Closed by @dylwil3 on 2025-11-17 15:11_

---

_Referenced in [Stars1233/ruff#435](../../Stars1233/ruff/pulls/435.md) on 2025-11-17 18:13_

---
