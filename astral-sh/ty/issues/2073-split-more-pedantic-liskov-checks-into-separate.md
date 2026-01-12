```yaml
number: 2073
title: Split more pedantic Liskov checks into separate error codes (and have them disabled by default)
type: issue
state: open
author: AlexWaygood
labels:
  - diagnostics
assignees: []
created_at: 2025-12-18T14:29:16Z
updated_at: 2025-12-18T14:29:16Z
url: https://github.com/astral-sh/ty/issues/2073
synced_at: 2026-01-12T15:54:26Z
```

# Split more pedantic Liskov checks into separate error codes (and have them disabled by default)

---

_@AlexWaygood_

Our rule enforcing the Liskov Substitution Principle on method overrides (currently called `invalid-method-override`) is currently much stricter than any other type checker's equivalent rule. We should split the more pedantic parts of this rule out into separate error codes, so that it is easier for users switching from other type checkers to incrementally adopt ty: they will then be able to suppress the specific error codes that cover the parts pyright/mypy did not complain about on a per-module basis.

One situation where we are stricter than other type checkers currently is the situation where the subclass method parameter has the same type annotation to the superclass method parameter, but the parameter has a different name. Mypy [does not](https://mypy-play.net/?mypy=latest&python=3.12&gist=44806f2f7a48b90407ebfe700b029c41) complain about this; pyright [does](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.13&reportUnreachable=true&code=MYGwhgzhAECCBcAoaLoBMCmAzaBbDALgBYD2aAFBBiFgDTQAe80AlgHYECU0AtAHzQAciTYYkqCdAAOkCIkShZ0AELlYncakw58xMpWp1oAT2bsuvAcNGbJ02YiA), but only if the method is [not a dunder method](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.13&reportUnreachable=true&code=MYGwhgzhAECCBcAoaLoBMCmAzaB9XAthgC4AWA9mvgBQQYhYA00AHvNAJYB2xAlNAFoAfNABy5LhiSoZ0AA6QIiRKEXQAQtVi9pqTDnxEylGnQbMAnu259BI8ZN2z5ixEA):

```py
class A:
    def method(self, x: int) -> None:
        pass

class B(A):
    def method(self, y: int) -> None:
        pass
```

Another situation where we are stricter than other type checkers is the case where the superclass parameter is positional-or-keyword, but the subclass parameter is positional-only. Mypy [does not](https://mypy-play.net/?mypy=latest&python=3.12&gist=81eb1a0b42ad71aaeb24d13e2c054476) complain about this, but pyright [does](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.13&reportUnreachable=true&code=MYGwhgzhAECCBcAoaLoBMCmAzaBbDALgBYD2aAFBBiFgDTQAe80AlgHYECU0AtAHzQAciTYYkqCdAAOkCIkShZ0AELlYncakw58xMpWp1GzdgXoB6bvyEixySShlREQA) (and for this one, pyright [does not distinguish](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.13&reportUnreachable=true&code=MYGwhgzhAECCBcAoaLoBMCmAzaB9XAthgC4AWA9mvgBQQYhYA00AHvNAJYB2xAlNAFoAfNABy5LhiSoZ0AA6QIiRKEXQAQtVi9pqTDnxEylGnQbM2nHswD0-YWIlTkslAqiIgA) between dunder methods and non-dunder methods).

```py
class A:
    def method(self, x: int) -> None:
        pass

class B(A):
    def method(self, x: int, /) -> None:
        pass
```

Similar to https://github.com/astral-sh/ty/issues/1644, it's very hard to tackle this without propagating the reason for a subtyping failure out of our `Type::has_relation_to_impl` methods. So this is blocked by https://github.com/astral-sh/ty/issues/163 currently.

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-18 14:29_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-18 14:29_

---
