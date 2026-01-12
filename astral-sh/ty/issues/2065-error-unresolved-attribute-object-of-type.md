```yaml
number: 2065
title: "error[unresolved-attribute] Object of type `Callable` has no attribute `__name__`"
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2025-12-18T12:14:51Z
updated_at: 2025-12-18T12:26:27Z
url: https://github.com/astral-sh/ty/issues/2065
synced_at: 2026-01-12T15:54:26Z
```

# error[unresolved-attribute] Object of type `Callable` has no attribute `__name__`

---

_@spaceone_

### Summary

```sh
$ ty check t.py 
t.py:4:12: error[unresolved-attribute] Object of type `(...) -> int` has no attribute `__name__`
Found 1 diagnostic
$ cat t.py
```
```python
from typing import Callable

def foo(cb: Callable[..., int]):
    return cb.__name__
```

### Version

0.0.3

---

_Comment by @sharkdp on 2025-12-18 12:26_

As explained [here](https://github.com/astral-sh/ty/issues/491#issuecomment-3669859197), I think everything is working as expected here.

See also: https://github.com/astral-sh/ty/issues/2038 and https://github.com/astral-sh/ty/issues/1495

---

_Closed by @sharkdp on 2025-12-18 12:26_

---
