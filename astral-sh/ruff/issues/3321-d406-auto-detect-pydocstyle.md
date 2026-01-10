```yaml
number: 3321
title: D406 auto-detect pydocstyle
type: issue
state: closed
author: epenet
labels:
  - docstring
assignees: []
created_at: 2023-03-03T14:54:05Z
updated_at: 2023-03-03T22:13:12Z
url: https://github.com/astral-sh/ruff/issues/3321
synced_at: 2026-01-10T11:09:46Z
```

# D406 auto-detect pydocstyle

---

_Issue opened by @epenet on 2023-03-03 14:54_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
- `command`: `ruff .`
- `version`: `ruff 0.0.247`
- `pyproject.toml`: https://github.com/home-assistant/core/blob/3a34f818e8add8527ec66c85a4066162d2a715d9/pyproject.toml#L267-L274

We currently ignore `D406` on the Home Assistant repository, and I would like to remove that code from the ignore list.

We also currently have a mixture of `numpy` and `google` styles, so we are not setting the convention at this stage.

What I don't understand right now is why `Args:` passes but not `Returns:` in the following docstring:
```python
def get_gas_price(data: EasyEnergyData, hours: int) -> float | None:
    """Get the gas price for a given hour.

    Args:
        data: The data object.
        hours: The number of hours to add to the current time.

    Returns:
        The gas market price value.
    """
```

I would expect either both `Args` and `Returns` to fail, or neither.

---

_Comment by @charliermarsh on 2023-03-03 15:03_

Will take a look at this today.

---

_Label `docstring` added by @charliermarsh on 2023-03-03 15:04_

---

_Comment by @charliermarsh on 2023-03-03 19:12_

`pydocstyle` does have the same behavior, I think:

```py
 foo.py:2 in public function `get_gas_price`:
        D406: Section name should end with a newline ('Returns', not 'Returns:')
```

---

_Comment by @charliermarsh on 2023-03-03 19:14_

Yeah this kind of thing has come up before. Basically, "Returns" is _both_ a NumPy _and_ a Google-compatible section name (but "Args" is not).

So if you have both, we infer "NumPy", and don't validate the "Args" header. `pydocstyle` suffers from the same issue.

---

_Comment by @charliermarsh on 2023-03-03 19:14_

I think a better heuristic would be to see if there are any headers that are only in one of the two conventions (like "Args").

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-03 19:21_

---

_Comment by @charliermarsh on 2023-03-03 19:21_

I'll improve this a bit.

---

_Comment by @epenet on 2023-03-03 21:40_

Cc @frenck, for information.

---

_Comment by @charliermarsh on 2023-03-03 21:42_

#3325 will improve this quite a bit, in that any docstring that contains (e.g.) `Args` will be treated as Google-style, any docstring that contains (e.g.) `Parameters` will be treated as NumPy-style, etc. (assuming you haven't set a `convention` in your config).

---

_Closed by @charliermarsh on 2023-03-03 22:13_

---
