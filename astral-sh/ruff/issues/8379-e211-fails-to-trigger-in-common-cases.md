```yaml
number: 8379
title: E211 fails to trigger in common cases
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-10-31T14:14:42Z
updated_at: 2023-10-31T14:30:17Z
url: https://github.com/astral-sh/ruff/issues/8379
synced_at: 2026-01-12T15:54:48Z
```

# E211 fails to trigger in common cases

---

_@charliermarsh_

Here's a test file where E211 seems to fail

```
from typing import Union
import sys

from logging import Logger


def fetch_name () -> Union[str, None]:
    """Fetch name from --person-name in sys.argv.

   Returns:
        name of the person if available, otherwise None
    """
    test = len(5)
    Logger.info(test)
    # test commented code
    # Logger.info("test code")
    for i in range (0, len (sys.argv)) :
        if sys.argv[i] == "--name" :
            return sys.argv[i + 1]
    return None
```
    
ruff v0.1.3 doesn't show any errors.
    
Here's my ruff.toml

```
line-length = 88

[lint]
preview = true
select = ["E", "F", "B", "W", "I", "PL", "C90", "RUF"]
ignore = ["E203","E231"]

[mccabe]
max-complexity = 10
```

_Originally posted by @chaitanya2692 in https://github.com/astral-sh/ruff/issues/3313#issuecomment-1787218716_
            

---

_Label `bug` added by @charliermarsh on 2023-10-31 14:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-31 14:14_

---

_Closed by @charliermarsh on 2023-10-31 14:30_

---
