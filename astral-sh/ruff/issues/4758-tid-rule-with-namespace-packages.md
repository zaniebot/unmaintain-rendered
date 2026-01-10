```yaml
number: 4758
title: TID rule with namespace packages
type: issue
state: closed
author: Saelyos
labels:
  - question
assignees: []
created_at: 2023-05-31T15:35:39Z
updated_at: 2023-06-01T19:48:44Z
url: https://github.com/astral-sh/ruff/issues/4758
synced_at: 2026-01-10T11:09:47Z
```

# TID rule with namespace packages

---

_Issue opened by @Saelyos on 2023-05-31 15:35_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I wanted to replace `absolufy-imports` with ruff `TID` rule, but it doesn't seems to handle namespaces correctly:

### A minimal code snippet that reproduces the bug
Structure
```
mynamespace/
    mypackage/
        __init__.py
        foo.py
        bar.py
pyproject.toml
```
foo.py
```python
from .bar import Bar
b = Bar()
```
bar.py
```python
class Bar:
    pass
```
###  The command you invoked
ruff ./mynamespace/mypackage/foo.py --fix

###  Expected fix 
```python
from mynamespace.mypackage.bar import Bar 
```

###  Actual fix
```python
from mypackage.bar import Bar 
```

###  The current Ruff settings
```toml
[tool.ruff]
namespace-packages = ["mynamespace/mypackage"]
select = ["TID"]

[tool.ruff.flake8-tidy-imports]
ban-relative-imports = "all"
```

###  The current Ruff version
0.0.270

---

_Comment by @charliermarsh on 2023-05-31 15:47_

Have you tried marking your namespace packages explicitly via [`namespace-packages`](https://beta.ruff.rs/docs/settings/#namespace-packages)?

---

_Label `question` added by @charliermarsh on 2023-05-31 15:48_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-31 15:50_

---

_Comment by @Saelyos on 2023-06-01 07:22_

I have indeed forgotten to add namespace-packages (sorry about that I didn't search enough), but unfortunately even when I add it I still have the same issue. 

---

_Comment by @charliermarsh on 2023-06-01 19:48_

I believe you need to set:

```toml
namespace-packages = ["mynamespace"]
```

I reproduced the above, and after that change, I get:

```py
from mynamespace.mypackage.bar import Bar
b = Bar()
```

---

_Closed by @charliermarsh on 2023-06-01 19:48_

---
