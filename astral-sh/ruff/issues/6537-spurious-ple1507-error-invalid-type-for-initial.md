```yaml
number: 6537
title: "Spurious PLE1507 error: Invalid type for initial `os.getenv` argument"
type: issue
state: closed
author: DimitriPapadopoulos
labels: []
assignees: []
created_at: 2023-08-13T17:10:16Z
updated_at: 2023-08-13T19:52:11Z
url: https://github.com/astral-sh/ruff/issues/6537
synced_at: 2026-01-12T15:54:46Z
```

# Spurious PLE1507 error: Invalid type for initial `os.getenv` argument

---

_@DimitriPapadopoulos_

* A minimal code snippet that reproduces the bug:
   ```py
  import os
  
  
  def f(using_clear_path):
      return os.getenv("PATH_TEST" if using_clear_path else "PATH_ORIG")
  ```
* The command you invoked:
  ```
  ruff --select PLE file.py
  ```
* The current Ruff settings: no `pyproject.toml` file
* The current Ruff version (`ruff --version`):
  ```
   ruff 0.0.284
  ```

---

_Renamed from "Invalid type for initial `os.getenv` argument" to "PLE1507: Invalid type for initial `os.getenv` argument" by @DimitriPapadopoulos on 2023-08-13 17:10_

---

_Renamed from "PLE1507: Invalid type for initial `os.getenv` argument" to "Spurious PLE1507 error: Invalid type for initial `os.getenv` argument" by @DimitriPapadopoulos on 2023-08-13 17:11_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-13 18:00_

---

_Comment by @charliermarsh on 2023-08-13 18:02_

Thanks, fixing...

---

_Closed by @charliermarsh on 2023-08-13 19:52_

---
