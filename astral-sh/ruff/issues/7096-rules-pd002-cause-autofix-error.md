```yaml
number: 7096
title: Rules PD002 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T18:33:35Z
updated_at: 2023-09-03T21:03:57Z
url: https://github.com/astral-sh/ruff/issues/7096
synced_at: 2026-01-10T11:09:49Z
```

# Rules PD002 cause autofix error

---

_Issue opened by @qarmin on 2023-09-03 18:33_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select PD002 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
def get_bar_or_line_data(df, x, c, y, **kwargs):
    if c and y:
        data = df.pivot_table(
                fill_value=None)
    else:
            (data.sort_values(kwargs['sort_c_on'],
                              inplace=True,
                              ascending=kwargs['ascending']))
```

error
```
/home/rafal/test/tmp_folder/3335870.py:7:31: PD002 `inplace=True` should be avoided; it has inconsistent behavior
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/3335870.py`, the rule codes PD002, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```
        
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12506838/python_compressed.zip)


---

_Label `fuzzer` added by @zanieb on 2023-09-03 19:02_

---

_Label `bug` added by @charliermarsh on 2023-09-03 20:54_

---

_Closed by @charliermarsh on 2023-09-03 21:03_

---
