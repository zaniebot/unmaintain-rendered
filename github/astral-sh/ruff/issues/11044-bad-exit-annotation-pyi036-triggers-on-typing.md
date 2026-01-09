---
number: 11044
title: "`bad-exit-annotation` (`PYI036`) triggers on `typing.overload` definitions"
type: issue
state: closed
author: tjkuson
labels:
  - bug
  - accepted
assignees: []
created_at: 2024-04-19T14:32:29Z
updated_at: 2024-04-24T15:48:53Z
url: https://github.com/astral-sh/ruff/issues/11044
synced_at: 2026-01-07T13:12:15-06:00
---

# `bad-exit-annotation` (`PYI036`) triggers on `typing.overload` definitions

---

_Issue opened by @tjkuson on 2024-04-19 14:32_

Keywords searched in the issue tracker: PYI036, overload

Running `ruff check --select PYI036 --isolated` using Ruff version 0.4.0 on the file

```python
import typing
from types import TracebackType


class Foo:
    def __enter__(self) -> typing.Self:
        return self

    @typing.overload
    def __exit__(self, exc_type: None, exc_val: None, exc_tb: None) -> None: ...

    @typing.overload
    def __exit__(
        self,
        exc_type: type[BaseException],
        exc_val: BaseException,
        exc_tb: TracebackType,
    ) -> None: ...

    def __exit__(
        self,
        exc_type: type[BaseException] | None,
        exc_val: BaseException | None,
        exc_tb: TracebackType | None,
    ) -> None:
        pass
```

produces six `PYI036` errors (three for each overload definition). The expected behaviour is that no `PYI036` errors are raised, as these are overloads that describe legal calls rather than the complete implementation. This approach was recommended on [this blog post](https://adamj.eu/tech/2021/07/04/python-type-hints-how-to-type-a-context-manager/).

---

_Comment by @charliermarsh on 2024-04-20 01:50_

@AlexWaygood - do you mind taking this one?

---

_Label `needs-decision` added by @charliermarsh on 2024-04-20 01:50_

---

_Comment by @AlexWaygood on 2024-04-20 10:14_

Yeah, good call — I think we should just skip any functions decorated with `@overload` for this check. Thanks for the report! 

---

_Label `needs-decision` removed by @AlexWaygood on 2024-04-20 10:14_

---

_Label `bug` added by @AlexWaygood on 2024-04-20 10:14_

---

_Label `accepted` added by @AlexWaygood on 2024-04-20 10:14_

---

_Comment by @AlexWaygood on 2024-04-20 12:41_

On second thought, we can provide _some_ validation for overloaded functions. Slightly more fiddly, but definitely doable.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-04-20 12:41_

---

_Comment by @charliermarsh on 2024-04-20 13:36_

Thanks Alex!

---

_Referenced in [astral-sh/ruff#11057](../../astral-sh/ruff/pulls/11057.md) on 2024-04-20 13:55_

---

_Comment by @johnslavik on 2024-04-22 13:41_

Some "related off-topic" from me, since I recently went through a similar thing.

There should be no need to write overloads if we return `None` in either case.
That's the thing for a PEP to propose the ability of unpacking unions of same-sized tuples:

```py
from __future__ import annotations

from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from _typeshed import OptExcInfo  # tuple[None, None, None] | tuple[type[BaseException], BaseException, TracebackType]
    from typing_extensions import Unpack


class Foo:
    def __exit__(self, *args: Unpack[OptExcInfo]) -> None:  # should be legal, but isn't
        pass
```

What Ruff might or might not want to do is [allow unpacking `_typeshed.ExcInfo` in the annotations of the `__exit__` signature](https://play.ruff.rs/9cd359a5-7745-4013-85b9-2e31c1b28e29), not require either `object` or full signature. But that would only make sense if we always expect an error to occur 🤷‍♀️

---

_Closed by @AlexWaygood on 2024-04-24 15:48_

---
