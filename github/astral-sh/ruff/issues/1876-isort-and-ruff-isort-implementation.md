---
number: 1876
title: Isort and Ruff Isort implementation incompatibilities
type: issue
state: closed
author: dbritto-dev
labels: []
assignees: []
created_at: 2023-01-14T22:19:46Z
updated_at: 2023-01-14T22:47:36Z
url: https://github.com/astral-sh/ruff/issues/1876
synced_at: 2026-01-07T13:12:14-06:00
---

# Isort and Ruff Isort implementation incompatibilities

---

_Issue opened by @dbritto-dev on 2023-01-14 22:19_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

**test_linting.py**

```python
import json
import datetime as dt

_ = json.dumps({})
__ = dt.datetime.now()
```
**pyproject.toml** 

```toml
[project]
version = "1.0.0"
description = "<some-description>"
name = "<some-name>"
requires-python = ">=3.8"
dependencies = [
  "fastapi", "python-dotenv", "beanie", "auth0-python", "auth0-jwt-validator",
  "hypercorn", "slack-sdk"
]

[project.optional-dependencies]
dev = [
  "nox", "ruff", "black", "pytest", "pytest-cov", "safety", "bandit", "debugpy"
]
lint = ["ruff", "black"]
test = ["nox", "pytest", "pytest-cov"]
security-test = ["safety", "bandit"]

[tool.setuptools.packages]
find = {}

[tool.ruff]
line-length = 99
target-version = "py310"
select = ["B", "C", "E", "F", "W"]
ignore = ["E501", "B904", "B008"]
exclude = [
  "build", "configs", "*.egg-info", ".github", ".githooks", ".vscode", ".nox"
]

[tool.ruff.pydocstyle]
convention = "google"

[tool.black]
line-length = 99
target-version = ['py310']
preview = true

[tool.pytest.ini_options]
minversion = "6.0"
addopts = "-ra -q"
testpaths = ["tests"]

[tool.coverage.run]
branch = true
source = ["app"]
```
**Result**

![image](https://user-images.githubusercontent.com/1697714/212499482-2903dcfd-d6cb-4aba-b4df-be601eb3b636.png)

**Info**

![image](https://user-images.githubusercontent.com/1697714/212499536-97aee9f3-0c44-4df8-bf85-aa487320b33d.png)

**Note:**

Since ruff wasn't able to find any issue with the imports, it wasn't able to fix the sort imports

---

_Comment by @charliermarsh on 2023-01-14 22:27_

You need to add `"I"` to your `select`, to enable import sorting -- like:

```toml
[tool.ruff]
line-length = 99
target-version = "py310"
select = ["B", "C", "E", "F", "W", "I"]
ignore = ["E501", "B904", "B008"]
exclude = [
  "build", "configs", "*.egg-info", ".github", ".githooks", ".vscode", ".nox"
]
```

---

_Comment by @charliermarsh on 2023-01-14 22:28_

I think that should resolve the issue -- but let me know if you see otherwise!

```
❯ ruff --select I foo.py --diff
--- foo.py
+++ foo.py
@@ -1,5 +1,5 @@
+import datetime as dt
 import json
-import datetime as dt

 _ = json.dumps({})
 __ = dt.datetime.now()

Would fix 1 error(s).
```

---

_Closed by @charliermarsh on 2023-01-14 22:28_

---

_Comment by @dbritto-dev on 2023-01-14 22:47_

Thanks, I would try it

---
