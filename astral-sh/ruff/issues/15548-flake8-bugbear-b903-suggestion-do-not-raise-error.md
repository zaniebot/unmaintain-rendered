---
number: 15548
title: "flake8-bugbear B903 suggestion : Do not raise error if keyword argument is present and target-python version is less or equals than 3.9"
type: issue
state: closed
author: guillaumeLepape
labels:
  - rule
assignees: []
created_at: 2025-01-17T11:28:51Z
updated_at: 2025-01-17T11:50:00Z
url: https://github.com/astral-sh/ruff/issues/15548
synced_at: 2026-01-10T01:22:56Z
---

# flake8-bugbear B903 suggestion : Do not raise error if keyword argument is present and target-python version is less or equals than 3.9

---

_Issue opened by @guillaumeLepape on 2025-01-17 11:28_

Hello,

I have this piece of code in one of my repo
```py
class RuleMetadata:
    def __init__(
        self,
        *,
        function: Union[
            Callable[[Session, UserModel, RuleComputeFinaleFromGroupRank], None],
            Callable[[Session, UserModel, RuleComputePoints], None],
        ],
        attribute: str,
        required_admin: bool = False,
    ) -> None:
        self.function = function
        self.attribute = attribute
        self.required_admin = required_admin
```

When I did this, I started by using dataclass, although I wanted the 3 arguments of the `__init__` function to be keywords only. As dataclass does not support `kw_only` before python 3.10 (reference https://docs.python.org/3/library/dataclasses.html#module-contents), I ended up writing the `__init__` function myself.

With the current implementation of B903, this will raise an error. My suggestion would be not to raise error if an keyword only argument and target-version<=3.9.

I'm currently raising a pull request, if you think my suggestion is ok, I'll let you review.


---

_Comment by @MichaReiser on 2025-01-17 11:39_

This makes sense to me and a PR would be great!

---

_Label `rule` added by @MichaReiser on 2025-01-17 11:39_

---

_Referenced in [astral-sh/ruff#15549](../../astral-sh/ruff/pulls/15549.md) on 2025-01-17 11:40_

---

_Closed by @guillaumeLepape on 2025-01-17 11:50_

---
