```yaml
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
synced_at: 2026-01-12T15:54:56Z
```

# Formatter: Parenthesized expression with comment on right hand side in assignment produces invalid syntax

---

_@MichaReiser_

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

_Closed by @dylwil3 on 2025-11-17 15:11_

---
