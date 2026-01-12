```yaml
number: 1916
title: "Implement isort's no_lines_before config"
type: issue
state: closed
author: harpener
labels:
  - good first issue
  - isort
assignees: []
created_at: 2023-01-16T13:45:02Z
updated_at: 2023-01-18T16:09:49Z
url: https://github.com/astral-sh/ruff/issues/1916
synced_at: 2026-01-12T15:54:41Z
```

# Implement isort's no_lines_before config

---

_@harpener_

Hi guys,

first of all, ruff is an amazing tool.

I was comparing what isort configuration does it support. Would it be possible to add isort's [no_lines_before](https://pycqa.github.io/isort/docs/configuration/options.html#no-lines-before)?

The behavior is that it is possible to provide a list of sections which are not preceeded with a blank line. The section names are already quite consistent in [ImportType enum](https://github.com/charliermarsh/ruff/blob/v0.0.223/src/rules/isort/categorize.rs#L10). Something like:
```yaml
# config
[tool.ruff.isort]
no-lines-before = ["FUTURE", "STDLIB', "THIRDPARTY", "FIRSTPARTY", "LOCALFOLDER"]
```
```python
# imports before
from __future__ import annotations

from typing import Any

from requests import Session

from my_first_party import my_first_party_object

from . import my_local_folder_object
```
```python
# imports after
from __future__ import annotations
from typing import Any
from requests import Session
from my_first_party import my_first_party_object
from . import my_local_folder_object
```

As for the implementation, I think you only need to add a condition for the type of the import block [here](https://github.com/charliermarsh/ruff/blob/v0.0.223/src/rules/isort/mod.rs#L609).

Thank you for considering this change. :-)

---

_Label `isort` added by @charliermarsh on 2023-01-16 17:19_

---

_Label `good first issue` added by @charliermarsh on 2023-01-16 17:20_

---

_Comment by @charliermarsh on 2023-01-16 17:20_

We can probably support this. It'd be a good first issue for anyone looking to contribute too.

---

_Closed by @charliermarsh on 2023-01-18 16:09_

---

_Closed by @charliermarsh on 2023-01-18 16:09_

---
