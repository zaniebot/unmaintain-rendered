```yaml
number: 1461
title: "Signatures of methods in generic classes should not use `Unknown` specialization"
type: issue
state: open
author: sharkdp
labels:
  - bug
  - generics
assignees: []
created_at: 2025-10-31T12:38:16Z
updated_at: 2025-10-31T18:00:17Z
url: https://github.com/astral-sh/ty/issues/1461
synced_at: 2026-01-10T02:06:25Z
```

# Signatures of methods in generic classes should not use `Unknown` specialization

---

_Issue opened by @sharkdp on 2025-10-31 12:38_

The signature of `C.f` should include the `T` type parameter, not `Unknown`:
```py
from typing import reveal_type

class C[T]:
    def f(self, x: T) -> None:
        pass

reveal_type(C.f)  # ty: def f(self, x: Unknown) -> None

C.f(C[int](), "foo")  # this should be an error
```
https://play.ty.dev/83603661-a572-4022-b155-e560f5bbbe85

---

_Label `bug` added by @sharkdp on 2025-10-31 12:38_

---

_Label `generics` added by @sharkdp on 2025-10-31 12:38_

---

_Comment by @carljm on 2025-10-31 17:57_

This is interesting, because it implies that when accessing an unbound method from a generic class, we should implicitly create a generic function with its own generic context and bound typevar instance.

Behavior of other type checkers seems to be a bit all over the place here. [Pyright](https://pyright-play.net/?pyrightVersion=1.1.405&strict=true&code=MYGwhgzhAEDCDaAVAugLgLACho%2BgEwFMAzaAWwIBcALAezwAoICQiAaaAD1WkQEpoAtAD4eGbLgkAnSgFdJAO05Ys0gG4EwIAPoUAngAcC9WADpy1Or2WZT52gwQBLeRWT1e7AEREaNT1cwgA) just uses `Unknown`, like we do currently. [Mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=a4474cbc042f17218e0d3ea6517e6399) seems to attempt to do the thing suggested in this issue, but fails to do it correctly -- it ends up with `T` in the generic context but `T'1` in the signature, and then fails to solve for `T` when you try to call it. [Pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeSIAxlKnHAAQDCA2gCoC6iAOuvX-Zhhh6AWxgAXABa5MACjgwoYADT18ieqwCU9ALQA%2BTd179TAJwkBXM73w8eFgG4xUUAPrjSxGLMaExUjJa9uh%2BAdJyLBDo4uyyWqpc4Li4SVogyiCW4tBwJOSIIADE9ACqOVAQnvRgluiUObjocCGCwmC4ZiKo4m7oliLYMGay6vTR4joG9HDiZsbmVjY1SQByA0Pz9MD4AL5JPBkgZBZgUKSE4rgiUBQlAAqkp%2BczGDgE9JRNkADm1j0QJqEHglADKMBg9Ek4nExDgiAA9AiTkJzoROj8ETB0AjMLhKHAEV90L9-o0cTVOvRUI5UNBUNhYJ9vhA-mYAU16LhiOS8jwyIF0LpnGY4IDeABeehJADMhAAjAAmA7oEC7TKoBoQZwAMWgMAoaCweCIZDVQA) appears to do the right thing.

I think the pyrefly behavior is what we want, but the wide range of behaviors suggests this isn't a high priority.

---

_Comment by @carljm on 2025-10-31 18:00_

The thing that's a bit strange here is that normally an un-specialized reference to `C` where `C` is a generic class, implies `C[Unknown]` (aka the default specialization). Our current behavior, and pyright's behavior, are consistent with that. In order to fix this issue, it sort of seems like we'd be special-casing `C.f` so that we _don't_ apply the default specialization when we are about to access an attribute of a class. This would definitely complicate the question of when the default specialization should be applied; it's not immediately clear to me what rules should be applied.

---
