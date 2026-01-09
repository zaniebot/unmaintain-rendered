---
number: 7143
title: Rule PT018 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-05T06:23:25Z
updated_at: 2023-09-05T14:50:18Z
url: https://github.com/astral-sh/ruff/issues/7143
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule PT018 cause autofix error

---

_Issue opened by @qarmin on 2023-09-05 06:23_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select PT018 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
class UnetOnnxModel(BertOnnxModel):
            assert not (
                self.find_graph_output(node.output[0])
                or self.find_graph_input(node.input[0])
                or self.find_graph_output(node.input[0])
            )
```

error
```
/home/rafal/test/tmp_folder/PY_FILE_TEST_123571023.py:2:13: PT018 Assertion should be broken down into multiple parts
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/PY_FILE_TEST_123571023.py`, the rule codes PT018, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12519549/python_compressed.zip)


---

_Label `bug` added by @MichaReiser on 2023-09-05 07:02_

---

_Label `fuzzer` added by @MichaReiser on 2023-09-05 07:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-05 11:49_

---

_Referenced in [astral-sh/ruff#7151](../../astral-sh/ruff/pulls/7151.md) on 2023-09-05 11:59_

---

_Closed by @charliermarsh on 2023-09-05 14:50_

---
