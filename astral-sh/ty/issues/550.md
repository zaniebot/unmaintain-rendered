```yaml
number: 550
title: "[ty] `__all__` should be respected for completions on items defined in stub files"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
  - server
  - completions
assignees: []
created_at: 2025-05-30T13:06:26Z
updated_at: 2025-10-03T12:20:11Z
url: https://github.com/astral-sh/ty/issues/550
synced_at: 2026-01-10T02:06:24Z
```

# [ty] `__all__` should be respected for completions on items defined in stub files

---

_Issue opened by @BurntSushi on 2025-05-30 13:06_

At present, `all_members` in `ty_extensions` doesn't respect `__all__` when listing members of a module with a stub file. But it should, because items not in `__all__` are not available at runtime.

There is a test for this behavior (currently asserting the wrong thing with a `FIXME` comment) in `crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md`.

-----

I'd argue that this is actually the correct behavior here as during runtime those attributes are available on the module.

But, I think we would need to consider it for stub files because that would provide an accurate picture of what symbols are actually present during runtime. Consider the following sources:

`module.py`:

```py
def evaluate(x=None):
    if x is None:
        return 0
    return x
```

`module.pyi`:

```py
from typing import Optional

__all__ = ["evaluate"]

def evaluate(x: Optional[int] = None) -> int: ...
```

`play.py`:

```py
import module

# This doesn't exists at runtime
module.Optional
# This exists
module.evaluate
```

Currently, it would include `Optional`:

```py
from ty_extensions import all_members

import module

# ty: Revealed type: `tuple[Literal["Optional"], Literal["__all__"], ..., Literal["evaluate"]]` [revealed-type]
reveal_type(all_members(module))
```

_Originally posted by @dhruvmanila in https://github.com/astral-sh/ruff/pull/18251#discussion_r2115160897_
            

---

_Label `bug` added by @BurntSushi on 2025-05-30 13:06_

---

_Label `server` added by @BurntSushi on 2025-05-30 13:06_

---

_Comment by @BurntSushi on 2025-05-30 13:15_

Derp. I created this issue before realizing that @dhruvmanila's [literal next comment](https://github.com/astral-sh/ruff/pull/18251#discussion_r2115177028) provided a fix for this, so I just applied it in https://github.com/astral-sh/ruff/pull/18251

---

_Closed by @BurntSushi on 2025-05-30 13:15_

---

_Label `completions` added by @AlexWaygood on 2025-10-03 12:20_

---
