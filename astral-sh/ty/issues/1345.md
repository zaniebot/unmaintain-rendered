```yaml
number: 1345
title: "Allow an attribute declared in a superclass's class body to be overridden by an attribute redeclared in a subclass's `__init__` method"
type: issue
state: open
author: AlexWaygood
labels:
  - attribute access
assignees: []
created_at: 2025-10-13T12:48:49Z
updated_at: 2025-11-28T15:52:18Z
url: https://github.com/astral-sh/ty/issues/1345
synced_at: 2026-01-10T01:58:59Z
```

# Allow an attribute declared in a superclass's class body to be overridden by an attribute redeclared in a subclass's `__init__` method

---

_Issue opened by @AlexWaygood on 2025-10-13 12:48_

### Summary

Consider the following snippet. ty, mypy, pyright and pyrefly all agree on the type of `B1().x`, but ty disagrees with the other three on the type of `B2().x`:

```py
class A:
    x: int

class B1(A):
    x: bool

class B2(A):
    def __init__(self) -> None:
        self.x: bool = True


reveal_type(B1().x)  # Ty, mypy, pyright, pyrefly: bool
reveal_type(B2().x)  # Mypy, pyright, pyrefly: bool. Ty: int!
```

- [Ty](https://play.ty.dev/707e2f40-6672-45ac-8386-ad7956d5b939)
- [Pyright](https://pyright-play.net/?pyrightVersion=1.1.405&strict=true&code=MYGwhgzhAECCBcBYAUNN0Ae9oEsB2ALiiqJDAEICMAFLAJRKrpbQBGA9uyMcqVNOQBMtBinTQAJgFMAZtAD68-DgKLqEKSBl1oAWgB80AHLs8UxuPEatAOhYcu0ALzQAKgCcArlJ4p3UgDcpMBB5AgBPAAcpaipqOjsdaABiNk5uZH8gkLComKF4xLRU-CJkIA)
- [Mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=f505a4fde6645c22604cfa7ce41a7d9e)
- [Pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeSIAxlKnHAAQCCiAOuvR-fovROgC5s21WgwBCARgAUjAJSt2nbvTy4oQ9CLr0xAJhny2nephhh6AfQt8I-K1LgwoYWfQC0APnoA5XOhgKxsaOzoTKqlD0ALz0ACoATgCuMBps8TAAbjCoUBb8pMQwUpJSsmGu9ADEKrhqaZnZufmFxfpl%2BBXVfPwgADQgifzQcCTkiCDVAKpDULak9GCJmkN%2BcBqm5mC48QC2qHboiTvYMPFSyt2unvRw-PGBnOn8ifHsYCwg3kcn9-TA%2BABfD5sPogMjpMBQUiEfi4HZQCjVAAKpAhUJuGBwBHolD8kAA5i99hA-IQ2NUAMowGD0AAW-H4xDgiAA9CzwWYoYRtviWTB0CzMLhKHAWbj0ASiSsBQttvRUBlUNBUNhYDi8RBCfFiX56LhiNKRmwyPxaX43Fl4nASewYh8AMyECS6YHoEAA-qoShDLIAMWgMAoaCweCIZHdQA)

I think for compatibility, it would be better if we followed what the other type checkers do here.

I spotted this by looking at the ecosystem report in https://github.com/astral-sh/ruff/pull/20723#issuecomment-3372209543 (all 4 new `unresolved-attribute` errors on aiohttp in the ecosystem report are caused by this).

### Version

_No response_

---

_Label `attribute access` added by @AlexWaygood on 2025-10-13 12:48_

---

_Renamed from "Ty does not allow an attribute declared in a class body to be overridden by an attribute in an `__init__` method" to "Allow an attribute declared in a class body to be overridden by an attribute in an `__init__` method" by @AlexWaygood on 2025-10-13 12:48_

---

_Renamed from "Allow an attribute declared in a class body to be overridden by an attribute in an `__init__` method" to "Allow an attribute declared in a class body to be overridden by an attribute redeclared in an `__init__` method" by @AlexWaygood on 2025-10-13 12:50_

---

_Renamed from "Allow an attribute declared in a class body to be overridden by an attribute redeclared in an `__init__` method" to "Allow an attribute declared in a superclass's class body to be overridden by an attribute redeclared in a subclass's `__init__` method" by @AlexWaygood on 2025-10-13 12:50_

---

_Comment by @sharkdp on 2025-10-13 13:07_

… but we should still error on the `self.x: bool = True` declaration, correct?

I just recently added some tests that are very similar to this scenario, but re-declaration scenarios are not yet considered: https://github.com/astral-sh/ruff/blob/513d2996ec599d3c1990480e0edd3a7328adcd0f/crates/ty_python_semantic/resources/mdtest/attributes.md?plain=1#L821-L919

---

_Comment by @AlexWaygood on 2025-10-13 13:08_

> … but we should still error on the `self.x: bool = True` declaration, correct?

yes, we also need to implement the Liskov Substitution Principle, correct! (And mutable attributes should be treated as invariant.)

---

_Comment by @AlexWaygood on 2025-10-13 13:22_

(Just noting things down for myself as much as anything else)

It also looks like this is the cause of all 5 new `unresolved-attribute` errors on `boostedblob` in https://github.com/astral-sh/ruff/pull/20723#issuecomment-3372209543

And all the new `unresolved-attribute` errors on `homeassistant`

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-10-17 14:43_

---

_Added to milestone `Beta` by @AlexWaygood on 2025-10-17 14:43_

---

_Removed from milestone `Beta` by @MichaReiser on 2025-11-28 15:52_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-28 15:52_

---
