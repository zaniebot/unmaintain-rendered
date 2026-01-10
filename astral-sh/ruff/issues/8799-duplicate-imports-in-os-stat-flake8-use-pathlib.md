```yaml
number: 8799
title: Duplicate Imports In os-stat flake8-use-pathlib Documentation
type: issue
state: closed
author: jgarte
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2023-11-21T02:25:09Z
updated_at: 2023-11-30T20:13:31Z
url: https://github.com/astral-sh/ruff/issues/8799
synced_at: 2026-01-10T11:09:51Z
```

# Duplicate Imports In os-stat flake8-use-pathlib Documentation

---

_Issue opened by @jgarte on 2023-11-21 02:25_

There are duplicate imports in the pathlib documentation.

See this doc:

https://docs.astral.sh/ruff/rules/os-stat/

```python
import os

import os
from pwd import getpwuid
from grp import getgrgid

stat = os.stat(file_name)
owner_name = getpwuid(stat.st_uid).pw_name
group_name = getgrgid(stat.st_gid).gr_name
```

etc

---

_Renamed from "Duplicate Imports In pathlib Documentation" to "Duplicate Imports In os-stat flake8-use-pathlib Documentation" by @jgarte on 2023-11-21 02:25_

---

_Label `documentation` added by @zanieb on 2023-11-21 16:21_

---

_Label `good first issue` added by @zanieb on 2023-11-21 16:21_

---

_Comment by @zanieb on 2023-11-21 16:21_

Pull request welcome; thanks for the issue!

---

_Comment by @jgarte on 2023-11-21 18:48_

Will try, this weekend. Thanks

---

_Closed by @charliermarsh on 2023-11-30 20:13_

---
