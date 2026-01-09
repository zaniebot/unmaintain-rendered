---
number: 10641
title: "Error referencing local package: Failed to parse metadata from built wheel, relative path without a working directory"
type: issue
state: open
author: ejstembler
labels:
  - question
assignees: []
created_at: 2025-01-15T20:14:00Z
updated_at: 2025-11-28T08:27:29Z
url: https://github.com/astral-sh/uv/issues/10641
synced_at: 2026-01-07T13:12:18-06:00
---

# Error referencing local package: Failed to parse metadata from built wheel, relative path without a working directory

---

_Issue opened by @ejstembler on 2025-01-15 20:14_

I have a local package which needs to be referenced.  For example, in pip it looks like this:

requirements.txt

```
./deps/mypackage-1.0.94-py3-none-any.whl
```

For uv, I have it in pyproject.toml as:

```
dependencies = [
# Local wheel file:
  "mypackage @ file://./deps/mypackage-1.0.94-py3-none-any.whl"
]
```

However, running `uv sync` results in this error:

```console
error: Failed to build: `my-project @ file:///Users/me/Projects/my-project`
  Caused by: Failed to parse metadata from built wheel
  Caused by: relative path without a working directory: ./deps/mypackage-1.0.94-py3-none-any.whl
mypackage@ file://./deps/mypackage-1.0.94-py3-none-any.whl
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

I've tried changing the reference to a few variations, none worked:

- `"mypackage @ ./deps/mypackage-1.0.94-py3-none-any.whl"`
- `"mypackage @ ./deps/mypackage-1.0.94-py3-none-any.whl"`
- `"mypackage @ myproject/deps/mypackage-1.0.94-py3-none-any.whl"`

I don't want to hard-code an absolute path since it will differ between local development and deployment.

---

_Comment by @zanieb on 2025-01-15 20:33_

I don't think relative paths are allowed there per the specification https://github.com/pypa/setuptools/discussions/2951

It sounds like you're looking for https://docs.astral.sh/uv/concepts/projects/dependencies/#path

---

_Label `question` added by @zanieb on 2025-01-15 20:33_

---

_Comment by @ejstembler on 2025-01-15 20:40_

`uv add ./deps/mypackage-1.0.94-py3-none-any.whl` worked.  Thanks @zanieb !

---

_Closed by @ejstembler on 2025-01-15 20:40_

---

_Comment by @vikramcee on 2025-02-07 18:19_

I believe relative paths are allowed with `pip` as OP specified above. Is there a way to enable `uv` to allow relative paths as follows:

`package @ file:relative/path/to/proj`

The above works with pip but throws an error when `sync`ing with `uv`. The purpose of this feature would be to allow a single `pyproject.toml` to be interoperable with `uv` and `pip`.

---

_Comment by @zanieb on 2025-02-08 01:37_

@vikramcee Do you have a working example?

---

_Comment by @vikramcee on 2025-02-11 18:44_

@zanieb apologies for the delay in getting back to you but yes, I have a minimal working example. The following `pyproject.toml` worked for me with `pip install -e ".[dev]"` but not with `uv sync --all-extras`:

```
[project]
name = "test_repo"
version = "0.0.1"
description = "A test repo"

requires-python = ">=3.9,<3.13"
dependencies = [
    "numpy",
    "scipy"
]

[project.optional-dependencies]
dev = [
    "pytest",
    "tqdm @ file:shared/tqdm"
]

[build-system]
requires = ["setuptools>=61"]
build-backend = "setuptools.build_meta"
```

I received the following error with `uv`:
```
  × Failed to build `test-repo @ file:///{my working directory}`
  ├─▶ Failed to parse metadata from built wheel
  ╰─▶ relative path without a working directory: shared/tqdm
      tqdm@ file:shared/tqdm ; extra == "dev"
            ^^^^^^^^^^^^^^^^
```

---

_Comment by @zanieb on 2025-02-12 14:05_

Thanks!

---

_Reopened by @zanieb on 2025-02-12 14:05_

---

_Comment by @scotthaleen on 2025-04-26 18:07_

I was running into the same issue `relative path without a working directory:` I was attempting to define a relative package in a script like this
```python
# /// script
# dependencies = [
#   "foo @ file://./foo-0.tar.gz",
#   "rich",
# ]
# ///
```

I had missed this https://github.com/astral-sh/uv/issues/10641#issuecomment-2593878620 

the `[tool.uv.sources]` does works for anyone else trying to figure this out for a script


```python
# /// script
# dependencies = [
#   "foo",
#   "rich",
# ]
# [tool.uv.sources]
# foo = { path = "foo.tar.gz" }
# ///

import foo
from rich.pretty import pprint
```

---

_Comment by @ArthurFerreiraRodrigues on 2025-11-22 19:31_

This one above solved. Thank you.

---

_Comment by @konstin on 2025-11-28 08:27_

If you have multiple local wheels, consider using a [flat index](https://docs.astral.sh/uv/concepts/indexes/#flat-indexes).

---
