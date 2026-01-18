```yaml
number: 2549
title: "`collections.namedtuple`: support passing `typename` and `field_names` by keyword argument"
type: issue
state: closed
author: AlexWaygood
labels:
  - runtime semantics
  - namedtuples
assignees: []
created_at: 2026-01-17T16:38:09Z
updated_at: 2026-01-18T00:02:35Z
url: https://github.com/astral-sh/ty/issues/2549
synced_at: 2026-01-18T00:17:55Z
```

# `collections.namedtuple`: support passing `typename` and `field_names` by keyword argument

---

_@AlexWaygood_

Pyright supports this, so ideally I think we would too (though mypy and pyrefly do not):

```py
from collections import namedtuple

NT = namedtuple(typename="NT", field_names="x y")
NT(x=1, y=2)
```

Note that these can only be passed by keyword for `collections.namedtuple`, not for `typing.NamedTuple`.

Cc. @charliermarsh 

---

_Label `runtime semantics` added by @AlexWaygood on 2026-01-17 16:38_

---

_Label `namedtuples` added by @AlexWaygood on 2026-01-17 16:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-17 17:03_

---

_Closed by @charliermarsh on 2026-01-18 00:02_

---
