```yaml
number: 7142
title: Rule PLW3301 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - help wanted
  - fuzzer
assignees: []
created_at: 2023-09-05T06:22:46Z
updated_at: 2023-09-06T03:27:06Z
url: https://github.com/astral-sh/ruff/issues/7142
synced_at: 2026-01-10T11:09:49Z
```

# Rule PLW3301 cause autofix error

---

_Issue opened by @qarmin on 2023-09-05 06:22_


Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select PLW3301 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
class FreeTypePen(BasePen):
    """Pen to rasterize paths with FreeType. Requires `freetype-py` module.
    """
    def __init__(self, glyphSet):
        """Converts the current contours to ``FT_Outline``.
        """
        if contain_x or contain_y:
                    dx = dx - min(min(*px), 0.0)
```

error
```
/home/rafal/test/tmp_folder/PY_FILE_TEST_10276102824.py:8:31: PLW3301 Nested `min` calls can be flattened
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/PY_FILE_TEST_10276102824.py`, the rule codes PLW3301, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/12519543/python_compressed.zip)


---

_Label `bug` added by @MichaReiser on 2023-09-05 07:01_

---

_Label `help wanted` added by @MichaReiser on 2023-09-05 07:01_

---

_Label `fuzzer` added by @MichaReiser on 2023-09-05 07:01_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-09-06 02:24_

---

_Closed by @dhruvmanila on 2023-09-06 03:27_

---
