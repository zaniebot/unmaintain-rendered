```yaml
number: 5112
title: "`UP035` false positive on `typing_extensions.dataclass_transform` on python 3.11"
type: issue
state: closed
author: DetachHead
labels:
  - good first issue
  - rule
  - help wanted
assignees: []
created_at: 2023-06-15T04:11:48Z
updated_at: 2023-06-22T15:34:46Z
url: https://github.com/astral-sh/ruff/issues/5112
synced_at: 2026-01-10T11:09:47Z
```

# `UP035` false positive on `typing_extensions.dataclass_transform` on python 3.11

---

_Issue opened by @DetachHead on 2023-06-15 04:11_

```py
from typing_extensions import dataclass_transform
# UP035 Import from `typing` instead: `dataclass_transform`
```

`typing_extensions.dataclass_transform` has the `frozen_default` parameter which won't be in `typing` until python 3.12.

from `typing_extensions`:
```py
if sys.version_info >= (3, 12):
    # dataclass_transform exists in 3.11 but lacks the frozen_default parameter
    dataclass_transform = typing.dataclass_transform
else:
    def dataclass_transform(
        *,
        eq_default: bool = True,
        order_default: bool = False,
        kw_only_default: bool = False,
        frozen_default: bool = False,
        field_specifiers: typing.Tuple[
            typing.Union[typing.Type[typing.Any], typing.Callable[..., typing.Any]],
            ...
        ] = (),
        **kwargs: typing.Any,
    ) -> typing.Callable[[T], T]:
        ...
```

---

_Comment by @charliermarsh on 2023-06-15 04:14_

Ah interesting, okay, thank you.

---

_Label `rule` added by @charliermarsh on 2023-06-15 04:14_

---

_Label `help wanted` added by @charliermarsh on 2023-06-15 13:46_

---

_Label `good first issue` added by @charliermarsh on 2023-06-15 13:47_

---

_Comment by @AlexWaygood on 2023-06-16 12:18_

Looks like ruff will also do this if you run `ruff --fix --select="UP035"`:

```diff
- from typing_extensions import SupportsIndex
+ from typing import SupportsIndex

  isinstance(42, SupportsIndex)
```

But the `isinstance()` check there is 20x faster if you use the `typing_extensions` version, as `typing_extensions` backports a ton of optimisations that have been added to `typing.Protocol` in Python 3.12

---

_Comment by @charliermarsh on 2023-06-22 14:59_

@AlexWaygood - Would you suggest not rewriting `Protocol` imports either for the same reason?

---

_Comment by @AlexWaygood on 2023-06-22 15:02_

> @AlexWaygood - Would you suggest not rewriting `Protocol` imports either for the same reason?

Yes -- and FWIW, pyupgrade recently made that change in https://github.com/asottile/pyupgrade/commit/5059713466c113e8aa09ab5c93b2c82f357ab206, which is included in pyupgrade v3.7 (following my PR https://github.com/asottile/reorder-python-imports/pull/341 to the reorder-python-imports project, which pyupgrade uses).

---

_Comment by @charliermarsh on 2023-06-22 15:03_

Thanks!

---

_Comment by @AlexWaygood on 2023-06-22 15:08_

> following my PR [asottile/reorder-python-imports#341](https://github.com/asottile/reorder-python-imports/pull/341) to the reorder-python-imports project, which pyupgrade uses

...which, looking at it again, it seems I forgot to include `SupportsIndex` in that PR, so I guess pyupgrade will continue to auto-replace `from typing_extensions import SupportsIndex` with `from typing import SupportsIndex` 🙃

Ruff can show its value-add here by not making that mistake :D

---

_Comment by @charliermarsh on 2023-06-22 15:11_

Hahah oh no :joy: 

---

_Closed by @charliermarsh on 2023-06-22 15:34_

---
