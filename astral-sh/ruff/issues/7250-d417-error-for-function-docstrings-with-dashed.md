```yaml
number: 7250
title: D417 Error for function docstrings with dashed lines
type: issue
state: closed
author: eronnen
labels: []
assignees: []
created_at: 2023-09-09T01:50:07Z
updated_at: 2023-09-10T16:28:45Z
url: https://github.com/astral-sh/ruff/issues/7250
synced_at: 2026-01-10T11:09:49Z
```

# D417 Error for function docstrings with dashed lines

---

_Issue opened by @eronnen on 2023-09-09 01:50_

```
check.py:8:5: D417 Missing argument description in the docstring for `b`: `y`
Found 1 error.
```

caused by the following script:

```python
"""Test Module."""

import logging

logging.root.setLevel(logging.INFO)

def b(y: int) -> int:
    """Calculate square.

    Args:
    ----
        y: integer to square

    Returns:
    -------
        int: the result.
    """
    return y*y

logging.info(b(4))
```

With the following configuration:

```toml
[tool.ruff]
select = ["D407", "D417"]
```


---

_Comment by @charliermarsh on 2023-09-10 16:27_

For reference, pydocstyle doesn't really support this either. pydocstyle doesn't flag this "by accident", because pydocstyle assumes that the docstring is a NumPy-style docstring, and then doesn't validate the "Args:" section (which isn't a valid NumPy-style section -- for NumPy-style docstrings, it would need to be "Parameters").

I don't mind supporting this specific case since it seems straightforward, but FWIW I don't really recommend mixing conventions like this since it can weird to unusual results.

---

_Closed by @charliermarsh on 2023-09-10 16:28_

---
