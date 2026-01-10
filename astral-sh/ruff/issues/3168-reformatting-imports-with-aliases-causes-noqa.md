```yaml
number: 3168
title: Reformatting imports with aliases, causes noqa comments of long lines (E501) to be on the wrong line
type: issue
state: closed
author: zkaddach
labels:
  - bug
  - suppression
assignees: []
created_at: 2023-02-23T11:46:26Z
updated_at: 2023-02-24T22:11:06Z
url: https://github.com/astral-sh/ruff/issues/3168
synced_at: 2026-01-10T11:09:46Z
```

# Reformatting imports with aliases, causes noqa comments of long lines (E501) to be on the wrong line

---

_Issue opened by @zkaddach on 2023-02-23 11:46_

Ruff version: 0.0.252
Ruff command: `ruff --fix --isolated --select E501,I001 /path/to/file.py`

Reformatting imports with aliases, causes **noqa comments of long lines (E501)** to be on the wrong line hence raising long line errors.

Before: 
```python
from myproject.my_very_long_sub_module_name.my_long_data_models_name.my_model_name import (  # noqa: E501
    MyModel as SomethingElse,
)
```

After: 
```python
from myproject.my_very_long_sub_module_name.my_long_data_models_name.my_model_name import (
    MyModel as SomethingElse,  # noqa: E501
)
```


PS: This doesn't happen when alias is not used. 

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).

-->


---

_Renamed from "Reformatting imports with aliases, causes **noqa comments of long lines (E501)** to be on the wrong line" to "Reformatting imports with aliases, causes noqa comments of long lines (E501) to be on the wrong line" by @zkaddach on 2023-02-23 11:49_

---

_Label `bug` added by @charliermarsh on 2023-02-23 12:37_

---

_Label `noqa` added by @charliermarsh on 2023-02-23 12:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-24 20:01_

---

_Comment by @charliermarsh on 2023-02-24 20:01_

Will take a look today, thanks for filing.

---

_Comment by @charliermarsh on 2023-02-24 20:29_

Def a bug, will require some refactoring.

---

_Closed by @charliermarsh on 2023-02-24 22:11_

---
