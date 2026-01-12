```yaml
number: 2447
title: Understand literal strings are containers of their characters
type: issue
state: closed
author: MeGaGiGaGon
labels: []
assignees: []
created_at: 2026-01-11T16:16:14Z
updated_at: 2026-01-11T16:24:54Z
url: https://github.com/astral-sh/ty/issues/2447
synced_at: 2026-01-12T02:26:12Z
```

# Understand literal strings are containers of their characters

---

_Issue opened by @MeGaGiGaGon on 2026-01-11 16:16_

### Summary

I recently ran into this when making a custom `in`-based function. It's not the end of the world, but it is annoying. From what I understand both these asserts should pass, but only the first one does.
https://play.ty.dev/ee126e65-f9da-417a-a00b-efb8b64869bb
```py
from collections.abc import Container
from typing import Literal
from ty_extensions import static_assert, is_assignable_to

static_assert(is_assignable_to(tuple[Literal["a"], Literal["b"], Literal["a"]], Container[Literal["a", "b"]]))  # Passess correctly
static_assert(is_assignable_to(Literal["aba"], Container[Literal["a", "b"]]))  # Fails incorrectly
```
With the function I was making looking like this:
https://play.ty.dev/8428cb9f-5759-44f3-86bf-c4a44950cd5d
```py
from collections.abc import Container
from typing import Literal, LiteralString, reveal_type
from ty_extensions import static_assert, is_assignable_to

def custom_in[S: LiteralString](o: object, c: Container[S]) -> S | None: raise NotImplementedError

reveal_type(custom_in("", ("a", "b", "a")))  # Revealed type: `Literal["a", "b"] | None` :)
reveal_type(custom_in("", "aba"))  # Revealed type: `Unknown | None` :(
```

### Version

playground 2c68057c4

---

_Comment by @AlexWaygood on 2026-01-11 16:24_

Thanks! This is the same as https://github.com/astral-sh/ty/issues/2128

---

_Closed by @AlexWaygood on 2026-01-11 16:24_

---
