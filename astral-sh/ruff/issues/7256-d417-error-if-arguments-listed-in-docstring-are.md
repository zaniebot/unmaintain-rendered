```yaml
number: 7256
title: "D417: Error if arguments listed in docstring are not vertically aligned with 'Args:'"
type: issue
state: closed
author: Laurent-Zerah
labels:
  - docstring
assignees: []
created_at: 2023-09-10T08:13:22Z
updated_at: 2023-09-10T16:28:16Z
url: https://github.com/astral-sh/ruff/issues/7256
synced_at: 2026-01-12T15:54:46Z
```

# D417: Error if arguments listed in docstring are not vertically aligned with 'Args:'

---

_@Laurent-Zerah_

Following command:
```
ruff check .\script.py
```
With `script.py` containing:
```
def calculate_speed(distance: float, time: float) -> float:
    """Calculate speed as distance divided by time.

    Args:
    ----
        distance: Distance traveled.
        time: Time spent traveling.

    Returns:
    -------
        Speed as distance divided by time.

    """
    return distance / time
```
Reports the following violation:
```
script.py:3:5: D417 Missing argument descriptions in the docstring for `calculate_speed`: `distance`, `time`
```
Where Ruff version is `0.0.286` and the settings in `pyproject.toml` are as follows:
```
[tool.ruff]
select = ["ALL"]
ignore= ["D203", "D213", "FA102", "FIX002", "N817", "S101", "S311", "S314", "T201", "TD002", "TD003"]
```
But if arguments are vertically aligned with `Args:`:
```
def calculate_speed(distance: float, time: float) -> float:
    """Calculate speed as distance divided by time.

    Args:
    ----
    distance: Distance traveled.
    time: Time spent traveling.

    Returns:
    -------
        Speed as distance divided by time.

    """
    return distance / time
```
Then Ruff does not complain.



---

_Label `docstring` added by @charliermarsh on 2023-09-10 16:25_

---

_Comment by @charliermarsh on 2023-09-10 16:28_

Thanks! Closing as a duplicate of https://github.com/astral-sh/ruff/issues/7256. I wrote a comment there, but FWIW I don't necessary recommend mixing docstring conventions (e.g., this snippet uses NumPy-style underlines but Google-style indentation and section names) as it can lead to unintended results.

---

_Closed by @charliermarsh on 2023-09-10 16:28_

---
