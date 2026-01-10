```yaml
number: 3290
title: RUF004 appears to be a duplicate of B026
type: issue
state: closed
author: calumy
labels:
  - bug
assignees: []
created_at: 2023-03-01T10:51:43Z
updated_at: 2023-03-01T18:00:04Z
url: https://github.com/astral-sh/ruff/issues/3290
synced_at: 2026-01-10T11:09:46Z
```

# RUF004 appears to be a duplicate of B026

---

_Issue opened by @calumy on 2023-03-01 10:51_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Take the snipped: 

```python 
serializer = self.serializer_class(data=request.data, *args, **kwargs)
```

When running `ruff check .` the following errors are returned for the snippet line: 

```
RUF004 Keyword argument `data` must come after starred arguments
B026 Star-arg unpacking after a keyword argument is strongly discouraged
```

Both the error messages report the same error. I don't believe both should be shown in this case.

Ruff version: 0.0.253

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-01 16:17_

---

_Comment by @charliermarsh on 2023-03-01 16:23_

I _think_ you're right. `RUF004` flags the keyword arguments, while `B026` flags the star argument, but they are effectively the same rule AFAICT.

---

_Label `bug` added by @charliermarsh on 2023-03-01 17:06_

---

_Closed by @charliermarsh on 2023-03-01 18:00_

---
