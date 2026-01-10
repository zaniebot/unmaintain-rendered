---
number: 9761
title: "PYI019 causes false positives on `.py` files if you support Python <=3.10"
type: issue
state: closed
author: AlexWaygood
labels:
  - rule
assignees: []
created_at: 2024-02-01T19:07:49Z
updated_at: 2025-04-28T18:57:38Z
url: https://github.com/astral-sh/ruff/issues/9761
synced_at: 2026-01-10T01:22:49Z
---

# PYI019 causes false positives on `.py` files if you support Python <=3.10

---

_Issue opened by @AlexWaygood on 2024-02-01 19:07_

I just tried enabling ruff's PYI rules on [typeshed-stats](https://github.com/AlexWaygood/typeshed-stats), a hobby project of mine, and ran into a false positive with PYI019. I have the following class in the project:

```py
from enum import Enum
from typing import TypeVar

_NiceReprEnumSelf = TypeVar("_NiceReprEnumSelf", bound="_NiceReprEnum")

class _NiceReprEnum(Enum):
    """Base class for several public-API enums in this package."""

    def __new__(cls: type[_NiceReprEnumSelf], doc: str) -> _NiceReprEnumSelf:
        assert isinstance(doc, str)
        member = object.__new__(cls)
        member._value_ = member.__doc__ = doc
        return member
```

PYI019 tells me off about this, saying:

```
src\typeshed_stats\gather.py:88:60: PYI019 Methods like `__new__` should return `typing.Self` instead of a custom `TypeVar`
```

But I still support Python 3.10 (`typing.Self` is new in Python 3.11), and don't have `typing_extensions` as a dependency, so this violation isn't actionable.

(This isn't an issue for `.pyi` files, as type checkers think `typing_extensions` is part of the stdlib due to some white lies we tell them in typeshed. That means `Self` can always be imported from `typing_extensions` in a `.pyi` file, even if the stubs package doesn't explicitly declare a dependency on `typing_extensions`.)

---

_Label `rule` added by @AlexWaygood on 2024-02-01 19:07_

---

_Referenced in [astral-sh/ruff#14183](../../astral-sh/ruff/issues/14183.md) on 2024-11-08 00:48_

---

_Referenced in [astral-sh/ruff#14184](../../astral-sh/ruff/issues/14184.md) on 2024-11-08 00:59_

---

_Referenced in [astral-sh/ruff#14217](../../astral-sh/ruff/pulls/14217.md) on 2024-11-09 11:10_

---

_Referenced in [astral-sh/ruff#14230](../../astral-sh/ruff/pulls/14230.md) on 2024-11-09 17:49_

---

_Comment by @Bibo-Joshi on 2025-04-08 18:19_

Hi. I'd like to bump this issue again.
I see that #14230 by @charliermarsh addressed this issue and that PR was included in [v0.7.4](https://github.com/astral-sh/ruff/releases/tag/0.7.4). Indeed, running that version doesn't suggest using `typing.Self` for me. However, it seems to be back in the latest version, i.e. a regression:

foo.py
```python
from typing import TypeVar

Self = TypeVar("Self", bound="Foo")

class Foo:
    def return_self(self: Self) -> Self:
        ...
        return self
```

```shell
$ ruff --version
ruff 0.11.4
$ ruff check .\foo.py --isolated --target-version py39 --select PYI
foo.py:6:20: PYI019 Use `Self` instead of custom TypeVar `Self`
  |
5 | class Foo:
6 |     def return_self(self: Self) -> Self:
  |                    ^^^^^^^^^^^^^^^^^^^^ PYI019
7 |         ...
8 |         return self
  |
  = help: Replace TypeVar `Self` with `Self`

Found 1 error.
```

Am I missing something or is this indeed a regression?
Looking forward to feedback :)

---

_Referenced in [python-telegram-bot/python-telegram-bot#4748](../../python-telegram-bot/python-telegram-bot/pulls/4748.md) on 2025-04-08 18:21_

---

_Comment by @MichaReiser on 2025-04-09 06:44_

@Bibo-Joshi the error message here is a bit confusing because you named your `TypeVar` Self. But the rule recommends you to use `typing.Self` over your own custom `TypeVar`, see https://peps.python.org/pep-0673/

If you still think this isn't working as expected, please open a new issue

https://play.ruff.rs/ef57ccbf-05d1-4e54-9c6c-3e2365da3d5b

---

_Comment by @MichaReiser on 2025-04-09 06:46_

This should also be a relatively simple fix, I think. All it needs is to early return from the function body if the current file isn't a stub file and the target version is 3.10 or older

https://github.com/astral-sh/ruff/blob/66cae0a3ec9b0d0d5a9540908f41fbf2238c1e13/crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_for_self.rs#L106

Edit: Oh, I think the challenge here is that we have no good way of knowing whether a project has `typing_extensions` as a dependency or not. 

I think my suggestion here would be to use `per-file-ignores` and disable the rule for all `*.py` files

---

_Label `good first issue` added by @MichaReiser on 2025-04-09 06:46_

---

_Label `good first issue` removed by @MichaReiser on 2025-04-09 06:47_

---

_Comment by @Bibo-Joshi on 2025-04-09 08:50_

Hi, thanks for the quick reply. However I do not think that the problem is the name of the `TypeVar`. Renaming to `T` still gives the issue:

https://play.ruff.rs/8c7fc9c8-af0e-42fa-87ac-b3b9c86ede79

Or did I misunderstand your point? Please let me know if I should open a separate issue.


---

_Comment by @MichaReiser on 2025-04-09 08:56_

Oh no, sorry. You're right and this is the same issue. 

---

_Comment by @AlexWaygood on 2025-04-09 17:43_

> Edit: Oh, I think the challenge here is that we have no good way of knowing whether a project has `typing_extensions` as a dependency or not.

yeah. And we have lots of rules that assume that you have `typing_extensions` as a dependency. Whatever we do here, we should fix it for all rules that have this problem at once, rather than applying a fix just to this rule.

Assuming you have `typing_extensions` as a dependency isn't an unreasonable assumption! Many, many projects have it either as an implicit or explicit dependency, since it is depended on by many core libraries and frameworks. So I wouldn't _necessarily_ want to disable this rule, and other rules like it, on lower Python versions.

It would be ideal if we could automatically detect whether or not the user has it listed as a dependency. Failing that, maybe we could consider a dedicated config option that you could toggle to tell Ruff whether you have it listed as a dependency, and use in rules like this...?

---

_Comment by @Bibo-Joshi on 2025-04-09 20:53_

I see how parsing the dependencies can be difficult with all those different options out there 😁 introducing a config option to override the auto-detect sounds sane to me at First glance, though I have no idea in what ruffs policy on adding new configs is.
What I'd like to point out is that having `typing_extensions` as _implicit_ dependency should _not_ count towards this auto-detection IMO, as that's bad practice in the first place. 

---

_Comment by @Avasam on 2025-04-10 01:52_

I always avoid using `typing_extensions` as a runtime dependency unless I absolutely have to, especially for libraries. But it can still be used for annotations no? (unless you need it at runtime).

```py
from __future__ import annotations  # Or manually wrap Self in a string.

from enum import Enum
from typing import TYPE_CHECKING  # or TYPE_CHECKING = False

if TYPE_CHECKING:
    from typing_extensions import Self

class _NiceReprEnum(Enum):
    """Base class for several public-API enums in this package."""

    def __new__(cls, doc: str) -> Self:
        assert isinstance(doc, str)
        member = object.__new__(cls)
        member._value_ = member.__doc__ = doc
        return member
```

That's how I've been doing it for various types taken from `typing_extensions`.

So I don't think saying this isn't actionable is true. The error message however can be updated to mention `typing_extensions` for relevant Python versions. And documentation can be updated to better teach users about typing-only imports and string-annotations.

---

_Comment by @Bibo-Joshi on 2025-04-10 10:14_

Typing-only dependencies are an interesting edge case, true. And they make an auto-detection of `typing_extensions` presence even harder. Still, I would also not like for ruff to throw an error telling me to switch to `typing_extensions` instead of using my custom `TypeVar` 😅

---

_Comment by @AlexWaygood on 2025-04-10 10:16_

Yeah, I agree with @Bibo-Joshi -- this rule is purely stylistic... I'm not sure the awkwardness of having to add an `if TYPE_CHECKING` block for the typing-only import is really better stylistically than defining a custom TypeVar! Though if you already have an `if TYPE_CHECKING` block in your module, that's perhaps a different case

---

_Comment by @Avasam on 2025-04-10 15:18_

When I want to avoid rewriting `if TYPE_CHECKING` constantly, I throw the relevant names in a compat module and use https://docs.astral.sh/ruff/settings/#lint_typing-modules to make Ruff happy if needed.
Type-checkers understand the symbols through re-exports.

Something like: `_compat/311.py` or `_typing.py`:
```py
if TYPE_CHECKING:
    from typing_extensions import ...
    from _typeshed import ...
else:
    # simplified symbol stubs, or re-implement what typing_extensions does
```

If you go a bit more verbose and use a `sys.version_info` check, you can even have [outdated-version-block (UP036)](https://docs.astral.sh/ruff/rules/outdated-version-block/#outdated-version-block-up036) tell you when that code is outdated.

Btw I'm not saying you should absolutely write it this way. Having a stylistic preference for `Typevar("Self")` is valid. I'm just making a point that rules assuming you can use `typing_extensions` isn't wrong or unactionable. (although it may not be *preferable* to you)

As an argument *for* this rule on Python <= 3.10: You may want your code to look as close as possible to "modern" annotations. Meaning that when dropping Python 3.10 support, you'd only have imports to change, rather than updating `self: Self` and removing `TypeVar`s everywhere.

Short of having a config option, my *personal* suggestion would be:
```toml
[lint.per-file-ignores]
# Keep using custom TypeVars until Python 3.10 support is dropped
"*.py" = ["PYI019"]
```
Which is just as verbose as having a dedicated config option 😉 

There's a small handful of `PYI` rules (namely regarding docstrings, default args, and variable length) that are nothing more than stylistic preferences coming from Typeshed and I myself disable in my projects.

---

_Comment by @MichaReiser on 2025-04-10 16:25_

A good first step would be to at least make the fix unsafe for `py` files when targeting <= 3.10 to avoid breaking tests

---

_Referenced in [scikit-hep/boost-histogram#996](../../scikit-hep/boost-histogram/pulls/996.md) on 2025-04-10 16:30_

---

_Referenced in [astral-sh/ruff#17340](../../astral-sh/ruff/pulls/17340.md) on 2025-04-10 18:18_

---

_Assigned to @ntBre by @ntBre on 2025-04-10 20:05_

---

_Referenced in [astral-sh/ruff#17611](../../astral-sh/ruff/pulls/17611.md) on 2025-04-24 15:31_

---

_Closed by @ntBre on 2025-04-28 18:57_

---

_Referenced in [astral-sh/ruff#17821](../../astral-sh/ruff/issues/17821.md) on 2025-05-03 18:37_

---
