```yaml
number: 18619
title: "UP007 crashes with TypeError on all version of Python prior to 3.14 for Optional[typing.NamedTuple]"
type: issue
state: closed
author: tleonhardt
labels:
  - rule
  - help wanted
assignees: []
created_at: 2025-06-11T00:37:08Z
updated_at: 2025-06-30T21:22:24Z
url: https://github.com/astral-sh/ruff/issues/18619
synced_at: 2026-01-12T15:54:56Z
```

# UP007 crashes with TypeError on all version of Python prior to 3.14 for Optional[typing.NamedTuple]

---

_@tleonhardt_

### Summary

Rule [UP007](https://docs.astral.sh/ruff/rules/non-pep604-annotation-union/) which replaces `Union` type hints with `|` and `Optional[foo]` with `foo | None` crashes on all versions of Python including the latest ones under the very specific circumstance of replacing `Optional[typing.NamedTuple]` with `typing.NamedTuple | None`.

The error is:
```sh
TypeError: unsupported operand type(s) for |: `function` and `NoneType`
```

I tested on Python 3.11, 3.13 saw identical behavior in all cases. It does work fine on 3.14 due to deferred type evaluation.

### Version

ruff 0.11.13 as installed from uv

---

_Comment by @ntBre on 2025-06-11 01:13_

This sounds very similar to #17711. Based on the answers there, I think this is kind of expected because `typing.NamedTuple`  is a function, not a type. It sounds like you can work around the error by adding `from __future__ import annotations`:

```shell
$ cat <<'EOF' | python -
from __future__ import annotations

import typing

x: typing.NamedTuple | None
print("okay")
EOF
okay
```

That works on Python 3.13, but I think that should be the default behavior on 3.14, so I'm surprised you ran into an issue:

```shell
$ cat <<'EOF' | uvx python@3.14 -
import typing

x: typing.NamedTuple | None
print("okay")
EOF
okay
```

it looks like my 3.14 was 3.14.0a6 in that command, but I get the same results with 3.14.0b1.

---

_Label `question` added by @ntBre on 2025-06-11 01:13_

---

_Renamed from "UP007 crashes with TypeError on all version of Python for Optional[typing.NamedTuple]" to "UP007 crashes with TypeError on all version of Python prior to 3.14 for Optional[typing.NamedTuple]" by @tleonhardt on 2025-06-11 01:28_

---

_Comment by @tleonhardt on 2025-06-11 01:30_

@ntBre I was wrong about 3.14, sorry about that. I tested again and it works fine there.

While importing from future annotations works well in the vast majority of cases, it isn't truly generically safe. There are some edge cases where it breaks things, in particular related to frozen dataclasses. For the application I am working on, attempting to use the future annotations completely breaks everything.

Would it be within reason to mark some or all fixes for `UP007` as **hidden**/**unsafe** to signal to users there is the potential of breaking things?

---

_Comment by @ntBre on 2025-06-11 01:36_

Ah good to know, thanks!

And interesting. We may need to call in the experts, that's about the limit of my typing advice 😄 

cc @AlexWaygood and @Daverball 

> Would it be within reason to mark some or all fixes for UP007 as hidden/unsafe to signal to users there is the potential of breaking things?

That sounds reasonable to me. The fix is already [unsafe](https://docs.astral.sh/ruff/rules/non-pep604-annotation-union/#fix-safety) for Python versions before 3.10 but unconditionally safe after 3.10. I'd be interested in others' thoughts on that too, though.

---

_Comment by @tleonhardt on 2025-06-11 01:47_

The [ruff documentation for UP007](https://docs.astral.sh/ruff/rules/non-pep604-annotation-union/) actually states:
> This rule's fix is marked as unsafe, as it may lead to runtime errors when alongside libraries that rely on runtime type annotations, like Pydantic, on Python versions prior to Python 3.10. It may also lead to runtime errors in unusual and likely incorrect type annotations where the type does not support the | operator.

However, I just verified using the latest version of `ruff` installed from the latest version of `uv` that it is treated as a safe fix.

Here is the toy code I used to test:

```Python
from typing import NamedTuple, Optional


def foo() -> Optional[NamedTuple]:
    """Test of weird type error."""
```

---

_Comment by @ntBre on 2025-06-11 01:54_

Yeah, I think the documentation is also slightly misleading here unless you read it in _just_ the right way ("This rule's fix is marked as unsafe, [...], on Python versions prior to Python 3.10"). In the implementation it's clear that it's only unsafe before 3.10:

https://github.com/astral-sh/ruff/blob/a2de81cb27541b7f09d57644539cea566cc0ece1/crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_annotation.rs#L143-L147

I should have included that before too.

---

_Comment by @tleonhardt on 2025-06-11 01:55_

Output I get from running `uv ruff check --no-fix` on above example using a Python 3.14 venv and `ruff` 0.11.13:

```sh
main.py:5:14: UP007 [*] Use `X | Y` for type annotations
  |
5 | def foo() -> Optional[NamedTuple]:
  |              ^^^^^^^^^^^^^^^^^^^^ UP007
6 |     """Test of weird type error."""
  |
  = help: Convert to `X | Y`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

And oops looks like I misread the documentation ;-) 

**So I guess what I'm asking is could it be marked as `unsafe` for all versions of Python prior to 3.14?**

---

_Comment by @tleonhardt on 2025-06-11 01:59_

While I'm taking the time to create an issue and comment ... I really want to reach out and give my gratitude to everyone at Astral and all of the open-source developers working on both `uv` and `ruff`. 

I have found these to be ground breaking tools that have enhanced my Python workflow quite noticeably.

So thanks to all of you and please keep up the great work!

---

_Comment by @Daverball on 2025-06-11 06:02_

This is one of those cases where we can't ever be 100% sure that `__or__`/`__lor__` won't crash at runtime (except for the very specific case of an annotated assignment inside a function scope, since that expression will never be evaluated and is completely discarded by the compiler).

But like everything it's a trade-off. Everything that's a real subclass of `type` and doesn't do something esoteric will work starting with 3.10.

`typing.NamedTuple` is not valid as a type, that's why bitwise or will not work on it. With type inference and multi-file analysis we could detect this and mark the fix as unsafe.

Type checkers will complain about using `typing.NamedTuple` as a type, whether it's spelled as `Optional[NamedTuple]` or `NamedTuple | None` does not change that (unless you're using a mypy plugin that gives `typing.NamedTuple` a meaning as a type).

That being said, there are still some rare valid use-cases where you have to use non-types in type annotations. `zope.interface` is a good example of this. `Interface` instances are not subclasses of `type`, they're pure `object` instances, so they're technically not valid types, however there is a mypy plugin to recognize them as such and until `zope.interface` manually added support for `__or__`/`__lor__` using bitwise or on an interface was an error, so you needed to use `Union`/`Optional`.

But either way I don't think it's worth to carve out any further exceptions for this rule. People that are doing something weird can manually mark the fix as unsafe in their ruff config or disable the rule entirely. Once we have type inference and multi-file analysis we can do a better job in more of those esoteric uses, but until then I personally wouldn't change anything.

---

_Comment by @AlexWaygood on 2025-06-11 11:29_

> Type checkers will complain about using `typing.NamedTuple` as a type, whether it's spelled as `Optional[NamedTuple]` or `NamedTuple | None` does not change that (unless you're using a mypy plugin that gives `typing.NamedTuple` a meaning as a type).

Unfortunately this isn't true @Daverball: due to inaccuracies in typeshed, type checkers don't currently complain if you use `typing.NamedTuple` as a type:

- https://mypy-play.net/?mypy=latest&python=3.12&gist=1508ed6c1e19109c32379a366788c31f
- https://pyright-play.net/?pythonVersion=3.12&strict=true&enableExperimentalFeatures=true&code=JYWwDg9gTgLgBDAnmYA7A5gKEwEwKYBmcBAFAB4BcCya6AdAHICGIeOAKgK5gA2eAlFTrCgA
- https://play.ty.dev/3e58b039-8d24-4509-90f2-75519449b758

[I strongly agree that they should](https://discuss.python.org/t/removing-type-checker-internals-from-typeshed/87960/4), however. And I agree with this:

> `typing.NamedTuple` is not valid as a type, that's why bitwise or will not work on it. With type inference and multi-file analysis we could detect this and mark the fix as unsafe.

`NamedTuple` is not a type; it's a type constructor. Using it in a type annotation doesn't make much sense IMO. But since type checkers will currently (incorrectly!) _not_ complain about it being used in a type annotation, I suppose it would be reasonable to specifically detect `NamedTuple` in this Ruff rule and not emit the diagnostic. It's a somewhat unique situation.

I don't think there's a need to mark the fix as unsafe for other type annotations. Situations like this are very rare, and I don't think we need to go out of our way to worry about annotations that would already be rejected by type checkers under the "normal" rules. The only reason why we might specifically consider carving out an exception in this case IMO is because of the current false negatives from type checkers regarding `NamedTuple`.

---

_Label `question` removed by @MichaReiser on 2025-06-11 11:40_

---

_Label `rule` added by @MichaReiser on 2025-06-11 11:40_

---

_Label `help wanted` added by @MichaReiser on 2025-06-11 11:40_

---

_Closed by @ntBre on 2025-06-30 21:22_

---
