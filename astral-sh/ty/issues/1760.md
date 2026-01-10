```yaml
number: 1760
title: "Auto-import should treat `__all__` as invalid if it references a symbol that does not exist"
type: issue
state: open
author: BurntSushi
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-04T16:54:20Z
updated_at: 2025-12-04T16:54:20Z
url: https://github.com/astral-sh/ty/issues/1760
synced_at: 2026-01-10T01:56:41Z
```

# Auto-import should treat `__all__` as invalid if it references a symbol that does not exist

---

_Issue opened by @BurntSushi on 2025-12-04 16:54_

Ref https://github.com/astral-sh/ruff/pull/21779/files/d314aa859dcf9a6ffb006a594d7ca0431dd4fab2#r2589451722

For example, consider this `foo.py`:

```python
__all__ = ['__all__', 'TRICKSY']
```

And this `test.py`:

```python
from foo import *
TRICKSY = 1
```

Even though `foo.py`'s `__all__` value is technically valid for `test.py`, `TRICKSY` itself does not exist in `foo.py`. This means `from foo import *` will fail at runtime.

We don't recognize this in auto-import, which means we'll offer an import for `TRICKSY` from `test.py`. But importing from `test.py` will result in a runtime error. So we probably shouldn't offer the import.

(I think this is a low priority issue. And we should be careful about how we go about this, because if we get the check wrong and treat `__all__` as invalid when it is valid, we'll get the exported interface of a module wrong. That could be worse than potentially offering an incorrect import in the rare case that `__all__` is itself incorrect. So the decision here might indeed to leave the status quo be.)

---

_Label `server` added by @BurntSushi on 2025-12-04 16:54_

---

_Label `completions` added by @BurntSushi on 2025-12-04 16:54_

---
