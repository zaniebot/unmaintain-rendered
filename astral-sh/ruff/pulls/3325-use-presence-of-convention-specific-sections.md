```yaml
number: 3325
title: Use presence of convention-specific sections during docstring inference
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/docstrings
created_at: 2023-03-03T21:35:45Z
updated_at: 2023-03-07T08:29:25Z
url: https://github.com/astral-sh/ruff/pull/3325
synced_at: 2026-01-12T15:55:12Z
```

# Use presence of convention-specific sections during docstring inference

---

_@charliermarsh_

Previously, if you had a docstring like:

```py
def get_gas_price(data: EasyEnergyData, hours: int) -> float | None:
    """Get the gas price for a given hour.

    Args:
        data: The data object.
        hours: The number of hours to add to the current time.

    Returns:
        The gas market price value.
    """
```

We'd first check for any NumPy-style sections. `Returns` is a valid section header for both NumPy- and Google-style docstrings, so we'd infer that this is a NumPy-style docstring, and validate it as such. _However_, `Args` is a Google-style header, and isn't part of the NumPy convention.

This PR thus improves the inference logic by treating docstrings that contain `Args` or `Arguments` as Google-style, and docstrings that contain `Parameters` or `Other Parameters` as NumPy-style. If none of those sections exist, we then user the convention with the most matches (there could be weird and subtle cases here in which users are unintentionally mixing header conventions by accident, but this seems like a strict improvement).

Closes #3321.

---

_Merged by @charliermarsh on 2023-03-03 22:13_

---

_Closed by @charliermarsh on 2023-03-03 22:13_

---

_Branch deleted on 2023-03-03 22:13_

---

_Comment by @epenet on 2023-03-07 08:29_

Thanks @charliermarsh 👍 🥳 

---
