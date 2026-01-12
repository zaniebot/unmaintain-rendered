```yaml
number: 17555
title: "Identical import inside+outside of `TYPE_CHECKING` block does not emit `F811`"
type: issue
state: open
author: injust
labels:
  - bug
assignees: []
created_at: 2025-04-22T14:58:51Z
updated_at: 2025-12-10T17:55:18Z
url: https://github.com/astral-sh/ruff/issues/17555
synced_at: 2026-01-12T15:54:56Z
```

# Identical import inside+outside of `TYPE_CHECKING` block does not emit `F811`

---

_@injust_

### Summary

```python
from __future__ import annotations

from collections.abc import AsyncGenerator, Iterable
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import AsyncGenerator, Iterable

async def foo(bar: Iterable[int]) -> AsyncGenerator[int]:
    pass
```

https://play.ruff.rs/5b99b487-12b4-45f7-b212-b8e5bdfc63bd

The `from collections.abc` import is repeated inside and outside the `if TYPE_CHECKING` block. I expected this to trigger some combination of [TC003](https://docs.astral.sh/ruff/rules/typing-only-standard-library-import/) and/or [F811](https://docs.astral.sh/ruff/rules/redefined-while-unused/), but Ruff doesn't report any errors.

### Version

0.11.6

---

_Comment by @ntBre on 2025-04-22 17:48_

Thanks for the report! After looking a bit at both rules, I'm not exactly sure which rule the bug is in, but it does look like a bug to me.

---

_Label `bug` added by @ntBre on 2025-04-22 17:48_

---

_Comment by @Daverball on 2025-04-22 21:11_

This does seem like it should trigger `F811`. Triggering `TC003` doesn't really make sense to me, since the reference points to the typing only import, not the original import.

I assume `F811` doesn't trigger because the second import happens inside a conditional block, which usually means that ruff can't treat the earlier binding as unused, since the conditional binding might not happen. But since `if TYPE_CHECKING` is special, it probably shouldn't be part of this exception.

There are however more questionable permutations of the same problem:

```python
from __future__ import annotations

from collections.abc import Iterable
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import Iterable

def foo(bar: Iterable[int]) -> None:
    assert isinstance(bar, Iterable)
```

Now we have a runtime use of `Iterable`, so `TC004` will trigger, but if we change the semantics for `F811` it will also trigger. So now the two violations/fixes kind of contradict one another (although it's not that bad, since whichever order the fixes happens in, you still end up with the same result: a single import outside the type checking block).

---

_Comment by @Daverball on 2025-04-22 21:16_

There is however also a world, where adding a new `TC` scoped rule to catch this sort of duplicate import makes more sense, than changing the scope of existing rules. I haven't really thought through the implications of changing how `F811` interacts with redefined imports inside a `if TYPE_CHECKING` block. But I can't think of any real problems off the top of my head. There are however problems for the opposite case, where a typing only import gets shadowed by a runtime import.

---

_Comment by @ntBre on 2025-04-23 15:15_

I don't have much to add, I think you summarized the issues and options very well!

I think this is technically a duplicate of #12270, which I didn't notice before, and also related to #5952, but if anything I'd probably mark those as the duplicates in light of the additional context here.

---

_Comment by @Daverball on 2025-04-23 15:29_

I think #5952 has actually been fixed and can be closed, the `default` parameter on `TypeVar` is being visited as a type expression in the current version of Ruff (I assume this has been the case at least since Ruff added support for Python 3.13). But it looks very much like the opposite problem I want to avoid.

---

_Comment by @ntBre on 2025-04-23 15:46_

Oh good catch, I didn't find the exact commit/PR but it looks like #5952 was fixed between 0.1.11 and 0.1.12.

---

_Comment by @wgmo on 2025-06-11 15:09_

(To avoid the confusion which I just experienced reading the bottom of this comment thread, ...)

> Oh good catch, I didn't find the exact commit/PR but it looks like this was fixed between 0.1.11 and 0.1.12.

The "this" here isn't _this_ issue #17555, it's #5952 as mentioned [here](https://github.com/astral-sh/ruff/issues/17555#issuecomment-2824698993)

The problem described here, [in this issue](#17555) is still present in ruff 0.11.13!


---

_Comment by @ntBre on 2025-06-11 16:43_

Thanks, will update my comment!

---

_Renamed from "Identical import inside+outside of `TYPE_CHECKING` block does not error" to "Identical import inside+outside of `TYPE_CHECKING` block does not emit `F811`" by @ntBre on 2025-12-10 17:55_

---
