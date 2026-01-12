```yaml
number: 3763
title: "noqa comments for N812 don't apply to the correct line in multi-line imports"
type: issue
state: closed
author: ehdr
labels:
  - bug
assignees: []
created_at: 2023-03-27T20:48:13Z
updated_at: 2023-03-28T15:48:20Z
url: https://github.com/astral-sh/ruff/issues/3763
synced_at: 2026-01-12T15:54:44Z
```

# noqa comments for N812 don't apply to the correct line in multi-line imports

---

_@ehdr_

When trying to apply a `noqa` comment for rule N812 (`lowercase-imported-as-non-lowercase`) on a multi-line import, it does not seem to apply to the correct line. For example, I would expect the following to not flag any errors:

```python
from mod import (
    lower_case as NonLowerCase,  # noqa: F401, N812
)
```

but it gives

```shell
n812.py:1:1: N812 Lowercase `lower_case` imported as non-lowercase `NonLowerCase`
n812.py:2:34: RUF100 [*] Unused `noqa` directive (unused: `N812`)
Found 2 errors.
```

The following however gives no errors:
```python
from mod import (  # noqa: N812
    lower_case as NonLowerCase,  # noqa: F401
)
```


---

_Label `bug` added by @charliermarsh on 2023-03-28 14:51_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-28 15:18_

---

_Closed by @charliermarsh on 2023-03-28 15:41_

---

_Comment by @charliermarsh on 2023-03-28 15:41_

Thanks! Will go out in the next release. (Either version will work.)

---

_Comment by @ehdr on 2023-03-28 15:48_

Lightning fast as always! Thank you! 🙏

---
