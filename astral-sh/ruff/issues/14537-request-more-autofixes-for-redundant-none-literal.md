```yaml
number: 14537
title: "Request: More autofixes for `redundant-none-literal`/`PYI061`"
type: issue
state: closed
author: Avasam
labels:
  - fixes
  - help wanted
  - accepted
assignees: []
created_at: 2024-11-22T19:04:27Z
updated_at: 2025-01-28T21:26:26Z
url: https://github.com/astral-sh/ruff/issues/14537
synced_at: 2026-01-12T15:54:53Z
```

# Request: More autofixes for `redundant-none-literal`/`PYI061`

---

_@Avasam_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
* A minimal code snippet that reproduces the bug.
https://docs.astral.sh/ruff/rules/redundant-none-literal mentions that the rule is sometimes autofixable, but I landed on the following cases which are not autofixed and I believe should:
```py
def get_rotation(rotation: float | Literal[None, "horizontal", "vertical"]) -> float: ...

def spy(
    Z,
    precision: float | Literal["present"] = ...,
    marker=...,
    markersize=...,
    aspect: Literal["equal", "auto", None] | float = ...,
    origin: Literal["upper", "lower"] = ...,
    **kwargs,
) -> tuple[AxesImage, Line2D]: ...

class Axes(_AxesBase):
    def spy(
        self,
        Z,
        precision: float | Literal["present"] = 0,
        marker=...,
        markersize=...,
        aspect: Literal["equal", "auto", None] | float = "equal",
        origin: Literal["upper", "lower"] = ...,
        **kwargs,
    ) -> AxesImage | Line2D: ...
```

* The command you invoked 
`ruff check --fix --preview --select=PYI061 --isolated`

* The current Ruff version (`ruff --version`).
ruff 0.8.0

---

_Label `fixes` added by @AlexWaygood on 2024-11-22 19:07_

---

_Label `help wanted` added by @AlexWaygood on 2024-11-22 19:08_

---

_Label `accepted` added by @AlexWaygood on 2024-11-22 19:08_

---

_Comment by @kiran-4444 on 2024-11-27 17:51_

I’d like to take this up.

---

_Assigned to @kiran-4444 by @MichaReiser on 2024-11-27 18:01_

---

_Comment by @kiran-4444 on 2024-12-06 05:53_

So, if I understand this correctly the rule `PYI061` is working for snippets like this:
```python
from typing import Literal

a: Literal[None]
b: Literal[1, 2, 3, "foo", 5, None]
```
which gets auto-fixed to:
```python
from typing import Literal

a: None
b: Literal[1, 2, 3, "foo", 5, None]
```
Here, I can see that the second statement isn't getting fixed. Is this the issue about? To fix things like this adhering to PYI061?




---

_Comment by @Avasam on 2024-12-06 16:58_

@kiran-4444 Exactly. Even the error message alludes to this being possible to autofix, just not implemented:
```py
PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
  |
3 | a: None
4 | b: Literal[1, 2, 3, "foo", 5, None]
  |                               ^^^^ PYI061
  |
  = help: Replace with `Literal[...] | None`
```

---

_Comment by @InSyncWithFoo on 2024-12-15 02:09_

This seems to have been resolved by #14872.

---

_Comment by @Avasam on 2024-12-15 03:30_

Yep, Looks like this is closed by https://github.com/astral-sh/ruff/pull/14872 !

---

_Closed by @Avasam on 2024-12-15 03:30_

---

_Comment by @Avasam on 2025-01-28 20:53_

Hmmm, I'm on `0.9.3` and all three of my example cases don't yet autofix:
`ruff check --fix --unsafe-fixes --preview`
```
stubs\matplotlib\axes\_axes.pyi:577:42: PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
    |
575 |         marker=...,
576 |         markersize=...,
577 |         aspect: Literal["equal", "auto", None] | float = "equal",
    |                                          ^^^^ PYI061
578 |         origin: Literal["upper", "lower"] = ...,
579 |         **kwargs,
    |
    = help: Replace with `Literal[...] | None`

stubs\matplotlib\pyplot.pyi:715:38: PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
    |
713 |     marker=...,
714 |     markersize=...,
715 |     aspect: Literal["equal", "auto", None] | float = ...,
    |                                      ^^^^ PYI061
716 |     origin: Literal["upper", "lower"] = ...,
717 |     **kwargs,
    |
    = help: Replace with `Literal[...] | None`

stubs\matplotlib\text.pyi:16:44: PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
   |
14 | from .transforms import Bbox, Transform
15 |
16 | def get_rotation(rotation: float | Literal[None, "horizontal", "vertical"]) -> float: ...
   |                                            ^^^^ PYI061
17 |
18 | class Text(Artist):
   |
   = help: Replace with `Literal[...] | None`

Found 3 errors.
```

---

_Comment by @charliermarsh on 2025-01-28 20:54_

Fixes just fine for me in the playground: https://play.ruff.rs/fd2b0091-14e2-44e2-9208-983d8e9c1862

---

_Comment by @Avasam on 2025-01-28 21:06_

I get it: on Python 3.9 the fix isn't offered at all rather than being "unsafe" (or using `Union`). I must've tested on 3.10 when I marked the issue as closed.
https://github.com/astral-sh/ruff/pull/14872#pullrequestreview-2500627900
> We can't safely offer the fix with the `|` syntax on Python <=3.9, as the `|` syntax for unions was added in Python 3.10. Possibly we could in the future use `typing.Union` for the fix and offer it even on older versions of Python, but I think we can leave that to another PR :-)

So I guess the fix being marked as unsafe but available was also left aside in that PR. I'll open a different issue for that specific use-case !

Done here: https://github.com/astral-sh/ruff/issues/15795

---
