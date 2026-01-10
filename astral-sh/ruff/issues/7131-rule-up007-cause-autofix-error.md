---
number: 7131
title: Rule UP007 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-04T11:50:53Z
updated_at: 2023-09-05T11:41:55Z
url: https://github.com/astral-sh/ruff/issues/7131
synced_at: 2026-01-10T01:22:46Z
---

# Rule UP007 cause autofix error

---

_Issue opened by @qarmin on 2023-09-04 11:50_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select UP007 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
from __future__ import annotations
from typing import Any, Dict, List, Optional  # noqa: F401
from telus_bulk.models.tmf_645.related_entity_ref_or_value import (
    RelatedEntityRefOrValue,
)
class ServiceRefOrValue(CamelModel):
    service_characteristic: Optional[
        List[Characteristic] | List[Any] | Characteristic
    ] = None
    service_specification: Optional[
        List[ServiceSpecificationRef]
        | List[ServiceSpecification]
    ] = None
```

error
```
/home/rafal/test/tmp_folder/1004304.py:7:29: UP007 Use `X | Y` for type annotations
/home/rafal/test/tmp_folder/1004304.py:10:28: UP007 Use `X | Y` for type annotations
Found 2 errors.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/1004304.py`, the rule codes UP007, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/12513459/python_compressed.zip)


---

_Comment by @charliermarsh on 2023-09-04 12:14_

We need to parenthesize these in some cases.

---

_Label `bug` added by @charliermarsh on 2023-09-04 12:15_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-04 12:15_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-04 21:43_

---

_Referenced in [astral-sh/ruff#7137](../../astral-sh/ruff/pulls/7137.md) on 2023-09-04 21:44_

---

_Closed by @charliermarsh on 2023-09-05 11:41_

---
