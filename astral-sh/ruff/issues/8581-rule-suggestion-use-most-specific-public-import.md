```yaml
number: 8581
title: "Rule suggestion: Use most specific public import"
type: issue
state: closed
author: chbndrhnns
labels: []
assignees: []
created_at: 2023-11-09T11:43:52Z
updated_at: 2023-11-09T17:05:52Z
url: https://github.com/astral-sh/ruff/issues/8581
synced_at: 2026-01-12T15:54:48Z
```

# Rule suggestion: Use most specific public import

---

_@chbndrhnns_

I'd like to see ruff convert imports from private modules to imports from the parent package if the member is declared in `__all__`.

Maybe an example makes it clearer:

```py
# ./client.py
from .myp._private import Member
from .myp._private import OtherMember

assert Member

# ./myp/__init__.py
__all__ = ["Member"]

from ._private import Member

# ./myp/_private.py
class Member: ...

class OtherMember: ...

```

I consider modules starting with an underscore private. Based on the rule, ruff would fix this snippet to:

```py
# ./client.py
from .myp import Member
from .myp._private import OtherMember

assert Member

# ./myp/__init__.py
__all__ = ["Member"]

from ._private import Member

# ./myp/_private.py
class Member: ...

class OtherMember: ...

```



---

_Renamed from "Rule suggestion: Convert imports from private modules " to "Rule suggestion: Use most specific public import" by @chbndrhnns on 2023-11-09 11:44_

---

_Comment by @charliermarsh on 2023-11-09 17:05_

Thanks for writing this up! I think it's roughly equivalent to #1107.

---

_Closed by @charliermarsh on 2023-11-09 17:05_

---
