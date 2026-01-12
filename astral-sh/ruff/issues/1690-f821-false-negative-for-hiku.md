```yaml
number: 1690
title: F821 – false negative for hiku
type: issue
state: closed
author: hyzyla
labels:
  - bug
assignees: []
created_at: 2023-01-06T14:28:33Z
updated_at: 2023-01-14T01:39:55Z
url: https://github.com/astral-sh/ruff/issues/1690
synced_at: 2026-01-12T15:54:41Z
```

# F821 – false negative for hiku

---

_@hyzyla_

I'm using [hiku](https://github.com/evo-company/hiku) package for building GraphQL server, and that package use class bracket syntax for defining input parameters. 



Here is minimal example of code that I'm trying to check:

main.py
```python
from typing import Optional

from hiku.expr.core import define
from hiku.types import Any as HikuAny
from hiku.types import Optional as HikuOptional
from hiku.types import Record


@define(HikuOptional[Record[{'name': HikuAny}]])
def foo() -> Optional[str]:
    pass
``` 
requirements.txt:
```txt
hiku==0.6.0
ruff==0.0.212
```

And when I'm trying to check that code by `ruff`, I receive error:

```txt
main.py:9:30: F821 Undefined name `name`
``` 






---

_Label `bug` added by @charliermarsh on 2023-01-06 15:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-06 21:21_

---

_Comment by @charliermarsh on 2023-01-06 21:24_

Thank you! This is revealing a bug in our import tracking (related to having both `from typing import Optional` and `from hiku.types import Optional as HikuOptional` in the same file -- it thinks that `HikuOptional` is an alias for the `Optional` from `typing`, which is of course a bug).

---

_Comment by @hyzyla on 2023-01-06 22:01_

Here is a link [1] that point to Optional definition  in `hiku` library 

1. https://github.com/evo-company/hiku/blob/1c45037b4c2434eb73824d9506f2df9eabecf223/hiku/types.py#L119

---

_Comment by @charliermarsh on 2023-01-06 22:38_

Thanks. Gotta think on this! Requires some refactoring, won't go out tonight but should be fixed in the next few days.

---

_Comment by @charliermarsh on 2023-01-13 04:23_

Working on this now. It's going to be a significant refactor of how we track imports.

---

_Closed by @charliermarsh on 2023-01-14 01:39_

---
