```yaml
number: 7127
title: Rule SIM222 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-04T06:16:59Z
updated_at: 2025-01-17T06:48:37Z
url: https://github.com/astral-sh/ruff/issues/7127
synced_at: 2026-01-12T15:54:46Z
```

# Rule SIM222 cause autofix error

---

_@qarmin_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select SIM222 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
def interpolate_nodes(
    data: "numpy array (n, 3)", mode: "'spline' or 'linear'", interval: float, *, direction: bool = 1
) -> "numpy array (n, 3)":
        return data
```

error
```
/home/rafal/test/tmp_folder/8279128.py:2:40: SIM222 Use `"spline"` instead of `"spline" or ...`
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/8279128.py`, the rule codes SIM222, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12509858/python_compressed.zip)



---

_Label `bug` added by @MichaReiser on 2023-09-04 06:26_

---

_Label `fuzzer` added by @MichaReiser on 2023-09-04 06:26_

---

_Comment by @dhruvmanila on 2023-09-04 07:06_

This seems to be caused because the surrounding quotes aren't being considered while generating the fix. So, `"'spline' or 'linear'"` is getting replaced with `""spline""`.

---

_Closed by @dhruvmanila on 2025-01-17 06:48_

---

_Closed by @dhruvmanila on 2025-01-17 06:48_

---
