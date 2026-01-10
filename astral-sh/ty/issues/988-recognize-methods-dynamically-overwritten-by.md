```yaml
number: 988
title: Recognize methods dynamically overwritten by metaclass
type: issue
state: open
author: CharlesPerrotMinot
labels:
  - feature
  - wish
assignees: []
created_at: 2025-08-14T15:42:01Z
updated_at: 2025-11-18T16:10:35Z
url: https://github.com/astral-sh/ty/issues/988
synced_at: 2026-01-10T01:58:59Z
```

# Recognize methods dynamically overwritten by metaclass

---

_Issue opened by @CharlesPerrotMinot on 2025-08-14 15:42_

### Summary

We have a generic class, where we use a metaclass and overwrite the `__init__` function, to accept any argument (and generate random data), that we use to be a random generator of models ; we use it in our tests to have randomly generated inputs for pydantic models.

I simplified the code for readability, but the following code triggers the error:

```py
from typing import Any, Callable


def define_builder_init(class_dict: dict[str, Any]) -> Callable[..., Any]:
    def func(self: Any, **kwargs: dict[str, Any]) -> None: ...
    return func

class BuilderMeta(type):
    def __new__(
        cls: type[Any],
        name: str,
        bases: tuple[type, ...],
        dct: dict[str, Any],
    ) -> "BuilderMeta":
        dct["__init__"] = define_builder_init(dct, )
        return super().__new__(cls, name, bases, dct)

class Builder(metaclass=BuilderMeta): ...

class ConfigValueBuilder(Builder):
    name: str

ConfigValueBuilder(name="123")
```

The error we're getting is
```
error[unknown-argument]: Argument `name` does not match any known parameter of bound method `__init__`
  --> test.py:27:27
   |
27 | test = ConfigValueBuilder(name="123")
   |                           ^^^^^^^^^^
   |
info: rule `unknown-argument` is enabled by default
```

Works without issues with mypy 1.17.1, with this config:
```
allow_subclassing_any=true
allow_untyped_decorators=true
ignore_missing_imports = true
no_warn_return_any=true
strict=true
```

### Version

0.0.1-alpha.18

---

_Comment by @carljm on 2025-08-14 16:36_

Mypy seems to issue the same error we issue: https://mypy-play.net/?mypy=latest&python=3.12&gist=334365bcee6c5792b44e177b8f56efca

(This is in non-strict mode so I think all the "allow" options you gave are implied.)

Are you sure that mypy actually works with this minimized example, or is there something else going on in your real codebase? (Like for instance `Builder` ends up being `Any` where you inherit from it, making `ConfigValueBuilder.__init__` accept anything at all.)

I would be quite surprised if any existing Python type checker actually supports dynamically setting `__init__` in a metaclass like that. [Pyright](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoCCKcANFAMICGANtZQEbUCmAsAFDvsAmTwUPwVEwD69AK5JqPEMNRIYACgDGdAM6rhXJEpgAuftpgBtVTBBkicALoBKKAFoAfBRp1GTIwDpvF4ld3sUEH8vFDAYihKCqpM1MD6lmQAVEkA1gDulCBoqvpaOiZmvtZ2TlAAcmAoTPrenoHBIEwwYiAoYRFKnGwqlOpQAEISUkwgALLNlArwCEw2AWzBIXzCwtXpqwoNS0EqubCIHpZWJNs7KJQQNVCm5mdL9H1M%2By0IzEYzTGR1J-fBXDo8oZCuZCH5TotgqVnAAiIaSaQTGCUGELHb-Aow1ZyGCrGFWKAAXmWQlEw2kshQ8gUAJgZBsfyCTRabRuYlmIAUNk8q3Wmz2ZAuVzIjxiqjItIZHB6alUg3JowUV2RvXUhPhI3Gk3mUDq3VVcvIVUEaAAajQxEwNdIFNbRvMzkLrrdukaUCbzdRLXbOU7CTCAIwAJgAzDCpUA) can't handle it either.

I don't think it's _entirely_ out of the realm of possibility to support it someday, but it'd be more like a long-term wishlist feature.

---

_Label `wish` added by @carljm on 2025-08-14 16:36_

---

_Renamed from "Overwritten __init__ not recognized when used with a metaclass" to "Recognize methods overwritten by metaclass" by @carljm on 2025-08-14 16:37_

---

_Renamed from "Recognize methods overwritten by metaclass" to "Recognize methods dynamically overwritten by metaclass" by @carljm on 2025-08-14 16:37_

---

_Label `feature` added by @carljm on 2025-08-14 16:37_

---

_Comment by @CharlesPerrotMinot on 2025-08-14 20:51_

Thanks for checking @carljm!
You're absolutely right, tried gettign a working example (with mypy and not ty), until I figured I somehow had completely missed that those calls (in that library) are marked with `# type: ignore[call-arg]`
Would be awesome if `ty` could support this though!

About marking the issue to ignore, there is # ty: ignore[unknown-argument]
But can we mark multiple call at one, like we can do with `# type: ignore[call-arg]`?
```
MyBuilder(
    name=name,
    value=value,
    value_translations=translations,
)  # type: ignore[call-arg]
```
I tried adding `ty: ignore[unknown-argument]` to the comment (or even replace the mypy one with it) but doesn't seem to work, only doing it line per line does

---

_Comment by @carljm on 2025-08-14 21:03_

Hmm, we may need to tweak how we handle silencing some errors that we give pretty precise locations to. Could you maybe create a new issue specific to that?

---

_Comment by @CharlesPerrotMinot on 2025-08-14 21:35_

More than happy to, thank you!

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
