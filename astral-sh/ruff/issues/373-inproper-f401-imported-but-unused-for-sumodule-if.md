```yaml
number: 373
title: "Inproper F401: \"imported but unused\" for sumodule if parent module is imported with an alias."
type: issue
state: closed
author: ghuls
labels:
  - bug
assignees: []
created_at: 2022-10-09T19:59:23Z
updated_at: 2022-10-09T21:37:59Z
url: https://github.com/astral-sh/ruff/issues/373
synced_at: 2026-01-10T15:56:05Z
```

# Inproper F401: "imported but unused" for sumodule if parent module is imported with an alias.

---

_Issue opened by @ghuls on 2022-10-09 19:59_

Inproper F401: "imported but unused" for sumodule if parent module is imported with an alias.

```python
import pyarrow as pa

import pyarrow.csv

print(pa.csv.read_csv("test.csv"))
```

```bash
$ ruff test_import.py 
No pyproject.toml found.
Falling back to default configuration...
Found 1 error(s).
test_import.py:3:1: F401 `pyarrow.csv` imported but unused
1 potentially fixable with the --fix option.
```

---

_Label `bug` added by @charliermarsh on 2022-10-09 20:01_

---

_Comment by @charliermarsh on 2022-10-09 20:01_

Good catch, will fix this.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-10-09 20:01_

---

_Comment by @charliermarsh on 2022-10-09 20:20_

(Found the offending code, working on fix now.)

---

_Closed by @charliermarsh on 2022-10-09 21:01_

---

_Comment by @ghuls on 2022-10-09 21:21_

Thanks. It solves the problem.
Maybe it would be good to invalidate the ruff cache in the ruff binary is not the same between invocations as first I thought you pushed unrelated changes to master as the errors were the same after recompilation.

Found another one:
```
$ ruff test_import.py 
No pyproject.toml found.
Falling back to default configuration...
Found 1 error(s).
test_import.py:3:1: F401 `numpy` imported but unused
1 potentially fixable with the --fix option.
```

It might be more tricky to solve as in involves type annotations in quoted strings.
```python
import sys

import numpy as np

if sys.version_info >= (3, 10):
    from typing import TypeAlias
else:
    from typing_extensions import TypeAlias


CustomInt: TypeAlias = "np.int8 | np.int16"
```

---

_Comment by @charliermarsh on 2022-10-09 21:27_

That one should work too, let me file a separate issue.

---

_Comment by @charliermarsh on 2022-10-09 21:37_

> Maybe it would be good to invalidate the ruff cache in the ruff binary is not the same between invocations as first I thought you pushed unrelated changes to master as the errors were the same after recompilation.

Hmm yeah. We do put the Cargo version in the cache key, so it's invalidated between _releases_. But I can see how this would be confusing when running against Git etc.


---
