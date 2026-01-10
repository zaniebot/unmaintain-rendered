```yaml
number: 14018
title: "RUF013 undocumented limitation `x: CustomClass = None` is not caught"
type: issue
state: open
author: jond01
labels:
  - documentation
  - rule
  - type-inference
  - preview
assignees: []
created_at: 2024-10-31T13:26:21Z
updated_at: 2024-11-06T04:58:40Z
url: https://github.com/astral-sh/ruff/issues/14018
synced_at: 2026-01-10T11:09:55Z
```

# RUF013 undocumented limitation `x: CustomClass = None` is not caught

---

_Issue opened by @jond01 on 2024-10-31 13:26_

It seems like the following should be caught or at least be added to the [documented limitations](https://docs.astral.sh/ruff/rules/implicit-optional/#limitations):

```py
from enum import Enum
from typing import Optional


class Letter(Enum):
    """ok."""

    A = "A"


class CatchMe:
    """class."""


def f(
    letter: Letter = None,  # <--- should be caught
    word: Optional[str] = None,
    catch: CatchMe = None,  # <--- should be caught
) -> None:
    """Doc."""
```

* List of keywords you searched for before creating this issue: RUF013
* A minimal code snippet that reproduces the bug: https://play.ruff.rs/afaf262e-0615-4732-83aa-4cc32af930c2
* The command you invoked: `ruff check file.py --select RUF013`
* The current Ruff settings: in the snippet.
* The current Ruff version: 0.7.1

---

_Label `documentation` added by @MichaReiser on 2024-10-31 13:38_

---

_Label `rule` added by @MichaReiser on 2024-10-31 13:38_

---

_Label `preview` added by @MichaReiser on 2024-10-31 13:38_

---

_Comment by @MichaReiser on 2024-10-31 13:40_

Thanks. I agree that this is surprising. I assume this is "intentional" because Ruff doesn't know to what types `Letter` resolves. E.g. `Letter` could be defined as `type Letter = str | None` in which case the assignment is valid. Not sure if there's another reason for it as well (CC: @charliermarsh )

Documenting this restriction is a good idea. 

---

_Label `type-inference` added by @dylwil3 on 2024-11-06 04:22_

---

_Comment by @dhruvmanila on 2024-11-06 04:58_

> I assume this is "intentional" because Ruff doesn't know to what types `Letter` resolves. E.g. `Letter` could be defined as `type Letter = str | None` in which case the assignment is valid. Not sure if there's another reason for it as well

Yeah, I think this is pretty much the reason. The logic considers all custom type to be similar to `Any` which is the case here. 

For context, the fallback branch tries to resolve the name `Letter` to a qualified name which doesn't exists and thus fallback to using `Unknown` (similar to `Any`).

https://github.com/astral-sh/ruff/blob/4ece8e5c1e674eb0d751fa55b5a72950aa9d20f0/crates/ruff_linter/src/rules/ruff/typing.rs#L116-L119

---
