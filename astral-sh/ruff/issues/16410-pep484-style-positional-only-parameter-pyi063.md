```yaml
number: 16410
title: "`pep484-style-positional-only-parameter` (`PYI063`) - false negative on `__new__` method"
type: issue
state: open
author: DetachHead
labels:
  - bug
assignees: []
created_at: 2025-02-27T03:46:26Z
updated_at: 2025-02-28T07:17:42Z
url: https://github.com/astral-sh/ruff/issues/16410
synced_at: 2026-01-12T15:54:55Z
```

# `pep484-style-positional-only-parameter` (`PYI063`) - false negative on `__new__` method

---

_@DetachHead_

### Summary

```py
class Foo:
    def foo(self, __asdf: int): ... # error (correct)

    def __new__(self, __asdf: int): ... # no error (incorrect)
```
https://play.ruff.rs/52ad7e69-3721-4a1a-8ec4-fa7fa99f7282

### Version

0.9.7

---

_Label `bug` added by @dhruvmanila on 2025-02-27 05:38_

---

_Comment by @dhruvmanila on 2025-02-27 05:41_

It also seems like the rule only checks the first parameter: https://play.ruff.rs/8982da12-d242-4794-805c-fa13bc6531f0 which seems like a bug as well.

---

_Comment by @Daverball on 2025-02-27 08:59_

> It also seems like the rule only checks the first parameter: https://play.ruff.rs/8982da12-d242-4794-805c-fa13bc6531f0 which seems like a bug as well.

The old special casing only worked if the first parameter was using double underscores, so any double underscores in a different position would more likely be a false positive. So I'm not convinced the other parameters should be flagged.

But it might make sense to flag all the directly following parameters with double underscores if the first one contains double underscores.

If agree that if we ever wanted to add an unsafe fix for this rule you'd definitely want it to rewrite `def (__a: int, __b: int)` to `def(a: int, b: int, /)`, rather than `def(a: int, /, __b: int)`.

---

_Comment by @DetachHead on 2025-02-27 23:59_

> any double underscores in a different position would more likely be a false positive. So I'm not convinced the other parameters should be flagged.

why's that?

---

_Comment by @Daverball on 2025-02-28 06:54_

> why's that?

Because it wouldn't have been recognized as a positional-only parameter by type checkers, so why should we tell people to turn it into one?

Now of course people can make mistakes and may have intended a parameter that isn't in the first position to be positional-only and just forgot to mark the previous parameter, but it seems just as likely to me that the parameter did indeed have double underscores in the original source code. I have seen it more than once when contributing stubs.

You always have to balance false negatives against false positives. Is it worth it to catch a few more things only to incorrectly flag others? Maybe, maybe not. I think the rule currently strikes a good balance, apart from the false negative with `__new__` you originally reported.

---

_Comment by @DetachHead on 2025-02-28 07:17_

oh right. i didn't realize this is considered invalid:

```py
def foo(a: int, __b: str): ...
```
i just assumed it meant both `a` and `__b` are considered positional-only. [pyright reports an error](https://basedpyright.com/?typeCheckingMode=all&reportUnusedParameter=false&code=CYUwZgBGD20BQEMBcECWA7ALgGggfTwCMUBnTAJwEoUA6OoA) if you do this but [mypy doesn't](https://mypy-play.net/?mypy=latest&python=3.13&flags=show-error-codes%2Cstrict%2Ccheck-untyped-defs%2Cdisallow-any-decorated%2Cdisallow-any-expr%2Cdisallow-any-explicit%2Cdisallow-any-generics%2Cdisallow-any-unimported%2Cdisallow-incomplete-defs%2Cdisallow-subclassing-any%2Cdisallow-untyped-calls%2Cdisallow-untyped-decorators%2Cdisallow-untyped-defs%2Cno-implicit-reexport%2Clocal-partial-types%2Cwarn-redundant-casts%2Cwarn-return-any%2Cwarn-unreachable%2Cwarn-unused-configs%2Cwarn-unused-ignores&gist=379b0feb78c35d25776dbf9e544f46eb)

---
