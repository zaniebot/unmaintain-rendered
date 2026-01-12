```yaml
number: 713
title: Check all valid import forms during import tracking
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2022-11-12T23:41:00Z
updated_at: 2022-11-13T05:10:14Z
url: https://github.com/astral-sh/ruff/issues/713
synced_at: 2026-01-12T15:54:40Z
```

# Check all valid import forms during import tracking

---

_@charliermarsh_

Assume that we're trying to import `typing.re.Match` (and, also, that `re.Match` doesn't exist, just for sake of example).

All of these forms (marked `# OK` need to work ):

```py
# OK
from typing import re

print(re.Match)

# Error
from typing import *

print(re.Match)

# OK
from typing.re import *

print(Match)

# OK
from typing.re import Match

print(Match)

# OK
import typing

print(typing.re.Match)

# OK
import typing.re

print(typing.re.Match)
```

In particular, we don't catch this:

```py
from typing import re

re.Match["foo"]
```

---

_Label `bug` added by @charliermarsh on 2022-11-12 23:41_

---

_Closed by @charliermarsh on 2022-11-13 05:10_

---
