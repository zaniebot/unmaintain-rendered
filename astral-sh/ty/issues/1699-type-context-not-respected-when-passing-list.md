```yaml
number: 1699
title: "Type context not respected when passing list literal to `Sequence` parameter"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - bidirectional inference
assignees: []
created_at: 2025-12-01T10:10:02Z
updated_at: 2025-12-01T10:12:47Z
url: https://github.com/astral-sh/ty/issues/1699
synced_at: 2026-01-10T01:58:59Z
```

# Type context not respected when passing list literal to `Sequence` parameter

---

_Issue opened by @sharkdp on 2025-12-01 10:10_

> @sharkdp I have a similar problem trying to use ty with DRF typings that started from 27 alpha, should I create a separate issue for that? 
> 
> ```py
> from __future__ import annotations
> 
> from typing import TYPE_CHECKING, Literal
> 
> if TYPE_CHECKING:
>     from collections.abc import Sequence
> 
> 
> type _Method = Literal["GET", "POST", "PUT", "DELETE"]
> 
> 
> def test(method: Sequence[_Method]) -> None:
>     return None
> 
> 
> test(["POST"])
> ```
> 
> 
> ```bash
> uvx ty check  a.py
> error[invalid-argument-type]: Argument to function `test` is incorrect
>   --> a.py:16:6
>    |
> 16 | test(["POST"])
>    |      ^^^^^^^^ Expected `Sequence[_Method]`, found `list[Unknown | str]`
>    |
> info: Function defined here
>   --> a.py:12:5
>    |
> 12 | def test(method: Sequence[_Method]) -> None:
>    |     ^^^^ ------------------------- Parameter declared here
> 13 |     return None
>    |
> info: rule `invalid-argument-type` is enabled by default
> 
> Found 1 diagnostic
> ``` 

 _Originally posted by @kalekseev in [#1689](https://github.com/astral-sh/ty/issues/1689#issuecomment-3595518452)_

---

_Label `bug` added by @sharkdp on 2025-12-01 10:10_

---

_Label `bidirectional inference` added by @sharkdp on 2025-12-01 10:10_

---

_Comment by @AlexWaygood on 2025-12-01 10:12_

This looks like it's the same as https://github.com/astral-sh/ty/issues/1576

---

_Comment by @sharkdp on 2025-12-01 10:12_

Looks like this has the same root cause as https://github.com/astral-sh/ty/issues/1576? 

---

_Closed by @sharkdp on 2025-12-01 10:12_

---
