---
number: 7570
title: "When I use `uv add <path>` to add a local package, no errors appear, but the installation is not successful."
type: issue
state: closed
author: vvandk
labels:
  - question
assignees: []
created_at: 2024-09-20T02:36:45Z
updated_at: 2024-09-20T08:53:27Z
url: https://github.com/astral-sh/uv/issues/7570
synced_at: 2026-01-10T01:24:16Z
---

# When I use `uv add <path>` to add a local package, no errors appear, but the installation is not successful.

---

_Issue opened by @vvandk on 2024-09-20 02:36_

When I use `uv add <path>` to add a local package, no errors appear, but the installation is not successful.
```
$ uv --version
uv 0.4.13 (Homebrew 2024-09-19)
$ uv venv
Using Python 3.10.9 interpreter at: /Library/Frameworks/Python.framework/Versions/3.10/bin/python3.10
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
$ uv add ../kinit-config  
Resolved 21 packages in 1ms
Audited 17 packages in 0.12ms
$ uv run config_ex.py 
Traceback (most recent call last):
  File "/Users/ktianc/programming/ktianc/project/kinit_v2/kinit_runtime/config_ex.py", line 1, in <module>
    from kinit_config import settings
ModuleNotFoundError: No module named 'kinit_config'
```

Below is my kinit-config and pyproject.toml configuration:

```
[project]
name = "kinit-config"
version = "0.0.1"
description = "kinit config"
license = { file = "LICENSE" }
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "pydantic-settings>=2.5.2",
    "pydantic>=2.9.2",
]

[tool.setuptools]
packages = ["kinit_config"]
```


Here is the pyproject.toml configuration after I executed the uv add ../kinit-config/ command

```
[project]
name = "kinit_runtime"
version = "0.0.1"
description = "kinit runtime"
license = { file = "LICENSE" }
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "fastapi>=0.115.0",
    "kinit-config",
    "loguru>=0.7.2",
    "pydantic-settings>=2.5.2",
    "rich>=13.8.1",
]


[tool.uv.sources]
kinit-config = { path = "../kinit-config" }


[tool.setuptools]
packages = ["kinit_runtime"]
```

---

_Comment by @zanieb on 2024-09-20 02:40_

Thanks for the report! Can you try `uv sync --reinstall --refresh-package kinit-config`?

---

_Comment by @vvandk on 2024-09-20 02:40_

```
$ uv pip list
Package           Version
----------------- -------
annotated-types   0.7.0
anyio             4.4.0
exceptiongroup    1.2.2
fastapi           0.115.0
idna              3.10
loguru            0.7.2
markdown-it-py    3.0.0
mdurl             0.1.2
pydantic          2.9.2
pydantic-core     2.23.4
pydantic-settings 2.5.2
pygments          2.18.0
python-dotenv     1.0.1
rich              13.8.1
sniffio           1.3.1
starlette         0.38.5
typing-extensions 4.12.2
```

The kinit-config was not found.

---

_Comment by @charliermarsh on 2024-09-20 02:43_

I think you need to add a `[build-system]` to `kinit-config/pyproject.toml`.

---

_Comment by @charliermarsh on 2024-09-20 02:44_

For example:

```toml
[build-system]
requires = ["setuptools>=42"]
build-backend = "setuptools.build_meta"
```

Otherwise, we treat the project as a non-packed project. See https://docs.astral.sh/uv/concepts/projects/#creating-projects.

---

_Label `question` added by @charliermarsh on 2024-09-20 02:44_

---

_Comment by @zanieb on 2024-09-20 02:58_

I thought we just did that for workspace members. Do we do that for path dependencies too? Or is this a workspace member and I'm missing it?

Anyway, we might want to sniff for `[tool.setuptools]` and warn if we don't have a build-system and aren't going to package the project.

---

_Comment by @vvandk on 2024-09-20 08:52_

The issue can be resolved by applying the following configuration after testing. Thank you!

```
[tool.uv]
package = true
```

---

_Closed by @vvandk on 2024-09-20 08:53_

---
