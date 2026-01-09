---
number: 11862
title: "False-positive for `N811` for full-caps, but non-constant imports"
type: issue
state: open
author: L4rryFisherman
labels:
  - bug
  - type-inference
assignees: []
created_at: 2024-06-13T18:30:44Z
updated_at: 2024-11-25T23:42:03Z
url: https://github.com/astral-sh/ruff/issues/11862
synced_at: 2026-01-07T13:12:15-06:00
---

# False-positive for `N811` for full-caps, but non-constant imports

---

_Issue opened by @L4rryFisherman on 2024-06-13 18:30_

The line:
```
from uuid import UUID as UUIDType
```
flags as [`N811`](https://docs.astral.sh/ruff/rules/constant-imported-as-non-constant/), whereas UUID is a class and not a constant.

Command used:
```
> ruff check
file.py:1:18: N811 Constant `UUID` imported as non-constant `UUIDType`
```

Ruff version: `0.4.8`


---

_Comment by @zawsq on 2024-06-14 06:11_

Ruff doesn't check type, it only check if it's caps. 
Just mark it with `noqa`

---

_Label `bug` added by @dhruvmanila on 2024-06-14 06:26_

---

_Label `type-inference` added by @dhruvmanila on 2024-06-14 06:26_

---

_Comment by @mmarras on 2024-10-03 22:02_

same false positive here:

`from django.db.models import Q as Query`

imho N811 should not be applied to single character "names", as naming conventions for single character names are ambigous anyways and in this particular case no one can say by just looking at the name if it is a constant or a class. `Q` here is a class, yet the above is flaged with N811, taking `Q` for a constant.

Maybe it would also be good to use no single character names for variables or classes, but that's another topic.


---

_Referenced in [astral-sh/ruff#14584](../../astral-sh/ruff/pulls/14584.md) on 2024-11-25 15:01_

---

_Comment by @snowdrop4 on 2024-11-25 15:08_

Addressing the false positive for `from uuid import UUID as UUIDType` is tricky. It seems to be a violation of PEP8, although a lot people would probably consider it acceptable for the class to be all-caps. A potential solution would be to whitelist common all-caps words like UUID, but obviously that's fragile, and I don't think anyone wants to go down that rabbit hole.

I do, however, have a [PR](https://github.com/astral-sh/ruff/pull/14584) for the following false-positives:

`from foo.bar import A as Apple`
`from foob.bar import Apple as A`

---
