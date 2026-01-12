```yaml
number: 4077
title: RUF008/RUF009 False positive on ClassVar
type: issue
state: closed
author: mishamsk
labels:
  - bug
assignees: []
created_at: 2023-04-24T13:45:01Z
updated_at: 2023-04-24T23:58:32Z
url: https://github.com/astral-sh/ruff/issues/4077
synced_at: 2026-01-12T15:54:44Z
```

# RUF008/RUF009 False positive on ClassVar

---

_@mishamsk_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Hi, I believe RUF008 should not be triggered for ClassVar's. E.g. for the following snippet:
```python
from dataclasses import dataclass
from typing import ClassVar

@dataclass
class Foo:
    bar: ClassVar[list[str]] = [] # RUF008 Do not use mutable default values for dataclass attributes
```
The command you invoked: `ruff check path/to/snippet.py`
The current Ruff version: `ruff 0.0.262`
The current Ruff settings: "RUF" enabled.

Actually, apparently there is no other way to initialize a classvar. Cause trying to use `field` with default_factory raise TypeError:
```python
from dataclasses import dataclass, field
from typing import ClassVar

@dataclass
class Foo:
    bar: ClassVar[list[str]] = field(default_factory=list)
```

Same applies to function calls, e.g.:
```python
from dataclasses import dataclass
from typing import ClassVar
import weakref

@dataclass
class Foo:
    bar: ClassVar[weakref.WeakValueDictionary[str, object]] = weakref.WeakValueDictionary()
```

---

_Renamed from "RUF008 False positive on ClassVar" to "RUF008/RUF009 False positive on ClassVar" by @mishamsk on 2023-04-24 13:48_

---

_Comment by @charliermarsh on 2023-04-24 15:19_

I think I agree with this.

---

_Comment by @dhruvmanila on 2023-04-24 15:22_

I can pick it up

---

_Label `bug` added by @charliermarsh on 2023-04-24 15:26_

---

_Assigned to @dhruvmanila by @charliermarsh on 2023-04-24 15:26_

---

_Closed by @charliermarsh on 2023-04-24 23:58_

---
