---
number: 3359
title: TCH (flake8-type-checking) not working as expected
type: issue
state: closed
author: roansong
labels:
  - question
assignees: []
created_at: 2023-03-06T13:45:56Z
updated_at: 2023-03-06T16:12:23Z
url: https://github.com/astral-sh/ruff/issues/3359
synced_at: 2026-01-07T13:12:14-06:00
---

# TCH (flake8-type-checking) not working as expected

---

_Issue opened by @roansong on 2023-03-06 13:45_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

# Overview

Running `ruff` is great overall, but the TCH checks don't appear to be behaving correctly for me.

```
ruff version: 0.0.254
```

## Code Snippet
I'm using poetry here, in a minimal project.

`test.py`
```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import Callable

def func(a: Callable) -> int:
    return 1
```

`pyproject.toml`
```toml
[tool.poetry]
name = "test-issue"
version = "0.1.0"
description = ""
authors = ["Roan Song <roansong@gmail.com>"]
readme = "README.md"
packages = [{include = "test_issue"}]

[tool.poetry.dependencies]
python = "^3.10"
ruff = "^0.0.254"
```

## Command

```bash
poetry run ruff check . --isolated --select TCH
test.py:4:33: TCH004 Move import `collections.abc.Callable` out of type-checking block. Import is used for more than type hinting.
Found 1 error.
```

I didn't manage to reproduce the following error consistently, but here's a screenshot of the opposite happening, where the VS Code plugin correctly says I should move code into a type checking block, but running `ruff check . --select TCH --isolated` showed no errors
<img width="964" alt="image" src="https://user-images.githubusercontent.com/14807642/223126969-4000029b-5c46-443a-8c1b-d6e8c5501190.png">




---

_Comment by @charliermarsh on 2023-03-06 13:53_

I believe the snippet above is actually correct, unless I'm misunderstanding.

If I run this snippet:

```py
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import Callable

def func(a: Callable) -> int:
    return 1
```

I get a runtime error:

```
❯ python foo.py
Traceback (most recent call last):
  File "/Users/crmarsh/workspace/staging/foo.py", line 6, in <module>
    def func(a: Callable) -> int:
NameError: name 'Callable' is not defined. Did you mean: 'callable'?
```

Type definitions used a module scope are actually required at runtime because they are exposed as `__annotations__`.

If you add `from __future__ import annotations`, the TCH error be resolved _and_ that code will work at runtime. But otherwise, they are considered runtime-required annotations.


---

_Label `question` added by @charliermarsh on 2023-03-06 13:53_

---

_Comment by @roansong on 2023-03-06 14:03_

Ah! That's it, thanks @charliermarsh . I didn't realise that `from __future__ import annotations` was the key here.

---

_Closed by @roansong on 2023-03-06 14:03_

---

_Comment by @charliermarsh on 2023-03-06 16:12_

It's super confusing :( As a tooling author, `from __future__ import annotations` is a real challenge.

---
