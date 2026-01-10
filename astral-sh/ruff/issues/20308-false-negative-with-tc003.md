```yaml
number: 20308
title: "False-negative with `TC003`"
type: issue
state: closed
author: osauldmy
labels:
  - question
assignees: []
created_at: 2025-09-08T18:08:40Z
updated_at: 2025-09-08T19:40:09Z
url: https://github.com/astral-sh/ruff/issues/20308
synced_at: 2026-01-10T11:09:59Z
```

# False-negative with `TC003`

---

_Issue opened by @osauldmy on 2025-09-08 18:08_

### Summary

Example to reproduce. In this case `Any` could be moved into `if TYPE_CHECKING` block, as it's not runtime-required import.

```python
from __future__ import annotations

from typing import TYPE_CHECKING, Any

if TYPE_CHECKING:
    from collections.abc import Iterator

def some_function(value: Any) -> Iterator[int]:
    del value
    yield from range(5)
```

If I remove the `Iterator` import outside, similarly to `Any`, `TC003` is correctly fired.

```python
from __future__ import annotations

from collections.abc import Iterator
from typing import TYPE_CHECKING, Any
```

`TC003 Move standard library import `collections.abc.Iterator` into a type-checking block`

Should also happen to `Any` from first snippet.

### Version

0.12.12

---

_Renamed from "False-negative with TC003" to "False-negative with `TC003`" by @osauldmy on 2025-09-08 18:08_

---

_Comment by @second-ed on 2025-09-08 18:49_

Is your expected behaviour this?

```python
from __future__ import annotations

from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import Iterator
    from typing import Any
```


---

_Comment by @osauldmy on 2025-09-08 18:59_

Yes

---

_Comment by @ntBre on 2025-09-08 19:37_

`typing` is one of the default [exempt-modules](https://docs.astral.sh/ruff/settings/#lint_flake8-type-checking_exempt-modules). If you set that to `[]`, you get the behavior you expect: [playground](https://play.ruff.rs/1cef8b0a-1259-49a1-b52c-68c36c2c666f).

---

_Label `question` added by @ntBre on 2025-09-08 19:37_

---

_Comment by @osauldmy on 2025-09-08 19:40_

One more setting I didn't know about. Thanks. Solves my problem, closing the issue.

---

_Closed by @osauldmy on 2025-09-08 19:40_

---
