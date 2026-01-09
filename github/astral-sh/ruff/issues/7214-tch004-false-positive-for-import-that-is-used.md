---
number: 7214
title: "TCH004: false positive for import that is used only in type hints"
type: issue
state: closed
author: WhyNotHugo
labels:
  - type-inference
assignees: []
created_at: 2023-09-07T09:36:40Z
updated_at: 2024-10-24T14:34:08Z
url: https://github.com/astral-sh/ruff/issues/7214
synced_at: 2026-01-07T13:12:15-06:00
---

# TCH004: false positive for import that is used only in type hints

---

_Issue opened by @WhyNotHugo on 2023-09-07 09:36_

My `test.py`:

```py
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from sentry_sdk._types import Event
    from sentry_sdk._types import Hint

def before_send(event: Event, hint: Hint) -> Event | None:
    pass
```

Steps to reproduce:

```console
> ruff --select TCH test.py
test.py:4:35: TCH004 [*] Move import `sentry_sdk._types.Event` out of type-checking block. Import is used for more than type hinting.
test.py:5:35: TCH004 [*] Move import `sentry_sdk._types.Hint` out of type-checking block. Import is used for more than type hinting.
Found 2 errors.
[*] 2 potentially fixable with the --fix option.
```

In this particular case, `sentry_sdk` only exports types `if TYPE_CHECKING`, so I cannot move this out of the type checking block.

---

_Comment by @WhyNotHugo on 2023-09-07 09:39_

Oh, this is half-wrong. I need to include `from __future__ import annotations` in this file for the example to do what I mean.

I'm not sure that the auto-fix is correct here; the auto-fix moves the `imports` out of the `if TYPE_CHECKING` block, but this is not valid in many cases (including this particular one).

The correct fix in this case is to include `from __future__ import annotations`. However, I suspect that this fix is invalid in some other situations.

---

_Comment by @charliermarsh on 2023-09-07 13:38_

Yeah, unfortunately Python requires that type annotations on function signatures are available at runtime. (This isn't true of all annotations -- for example, annotated assignments outside of the top-level are not evaluated at runtime IIRC.)

The current fix is limited to moving imports as it attempts to avoid changing the semantics of the code. We've explored supporting automatic quoting for annotations (https://github.com/astral-sh/ruff/pull/6001), which would let us handle this case without adding a `from __future__ import annotations` import. We could _also_ consider a setting to automatically add `from __future__ import annotations` in such cases, but that's an even bigger change to user code and would not be safe to do by default.

(If you _do_ want to use `__future__` annotations here, I might recommend using this with `from __future__ import annotations` as a [required imports](https://beta.ruff.rs/docs/settings/#isort-required-imports).)

---

_Comment by @WhyNotHugo on 2023-09-08 09:56_

The main problem here is that the auto-fix suggests moving the imports out of the `if TYPE_CHECKING`, but this is not correct because the imported types are themselves defined in a `if TYPE_CHECKING` block (and cannot be imported when `TYPE_CHECKING == false`.

I do agree that including `from __future__ import annotations` might have unintended consequences, but the current fix simply generates broken code.

Perhaps both fixes can be offered as LSP code actions but none applied automatically with `--fix`, given that which one is correct is very contextual (unless you think that it’s feasible to determine which of both fixes is correct in each scenario).


---

_Comment by @zanieb on 2023-09-08 16:19_

We can't know that the imported types are only available when type checking (without adding multifile analysis) and that seems like a relatively rare case. Perhaps it's worth opening a bug report at Sentry? I'm curious why they made that design decision.

We are already using the "suggested" applicability for this fix so once #4185 lands we will not apply this when `--fix` is used.



---

_Comment by @aqeelat on 2023-09-20 11:09_

I have an example that shows another false positive with TCH004:

`pyproject.toml`:
```toml
[tool.ruff]
extend-select = ["TCH"]
line-length = 120
target-version = "py311"
```

`base.py`:
```python
import abc
from typing import Generic
from typing import TypeVar


T = TypeVar("T")


class MyBaseClass(abc.ABC, Generic[T]):
    pass
```

`concrete.py`:
```python
from __future__ import annotations

from typing import TYPE_CHECKING

from .base import MyBaseClass

if TYPE_CHECKING:
    from collections.abc import Iterator


class MyConcreteClass(MyBaseClass[list[Iterator]]):
    def __init__(self, items: list[Iterator]):
        self.items = items

```

Error:
```shell
concrete.py:8:33: TCH004 [*] Move import `collections.abc.Iterator` out of type-checking block. Import is used for more than type hinting.
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

Tbh, I'm not sure if this is a false positive or is a correct error. 
We've been using it as `class MyConcreteClass(MyBaseClass[list["Iterator"]])` in production without any issues, but when I removed the quotations, this error appeared.

---

_Comment by @charliermarsh on 2023-09-20 13:17_

@aqeelat - Thanks for the clear write-up. I _believe_ that error is correct, since `class MyConcreteClass(MyBaseClass[list[Iterator]]):` requires that the type is available at runtime? So moving it into a `TYPE_CHECKING` block is unsafe.

---

_Label `type-inference` added by @zanieb on 2023-10-03 17:23_

---

_Label `multifile-analysis` added by @zanieb on 2023-10-03 17:23_

---

_Referenced in [astral-sh/ruff#7769](../../astral-sh/ruff/pulls/7769.md) on 2023-10-03 17:27_

---

_Closed by @zanieb on 2023-10-06 03:41_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:34_

---

_Referenced in [populationgenomics/ClinvArbitration#17](../../populationgenomics/ClinvArbitration/pulls/17.md) on 2025-04-29 23:34_

---

_Referenced in [glass-dev/glass#619](../../glass-dev/glass/pulls/619.md) on 2025-05-07 12:56_

---
