```yaml
number: 7104
title: Rules UP022 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T18:47:31Z
updated_at: 2023-09-03T21:14:58Z
url: https://github.com/astral-sh/ruff/issues/7104
synced_at: 2026-01-10T11:09:49Z
```

# Rules UP022 cause autofix error

---

_Issue opened by @qarmin on 2023-09-03 18:47_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select UP022 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
import subprocess
def _get_test_output(cmd_list):
    try:
        CP = subprocess.run(cmd_list, timeout=5,
                            check=True, capture_output=True,
                            stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    except Exception:
        return None
```

error
```
/home/rafal/test/tmp_folder/6793334.py:4:14: UP022 Sending `stdout` and `stderr` to `PIPE` is deprecated, use `capture_output`
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/6793334.py`, the rule codes UP022, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```


[python_compressed.zip](https://github.com/astral-sh/ruff/files/12506951/python_compressed.zip)



---

_Label `fuzzer` added by @zanieb on 2023-09-03 19:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 21:02_

---

_Label `bug` added by @charliermarsh on 2023-09-03 21:02_

---

_Closed by @charliermarsh on 2023-09-03 21:14_

---
