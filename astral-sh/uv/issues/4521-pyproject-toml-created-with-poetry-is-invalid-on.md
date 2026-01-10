```yaml
number: 4521
title: pyproject.toml created with poetry is invalid on versions after 0.2.11
type: issue
state: closed
author: lskbr
labels:
  - bug
  - configuration
assignees: []
created_at: 2024-06-25T16:34:57Z
updated_at: 2024-06-25T18:53:16Z
url: https://github.com/astral-sh/uv/issues/4521
synced_at: 2026-01-10T05:31:37Z
```

# pyproject.toml created with poetry is invalid on versions after 0.2.11

---

_Issue opened by @lskbr on 2024-06-25 16:34_

I updated my uv to version 0.2.15 today, and I noticed some configurations in my pyproject.toml were not being applied, especially extra indexes. Running in debug mode, I noticed it complains that the pyproject.toml is malformed and misses a project section.
In version 0.2.15, the error is the same, but after it, the section [tool.uv.pip] is ignored. In version 0.2.11, the project section is not present and the error reported is the same, but [tool.uv.pip] is honored.

Up to version 0.2.11, the same toml works without any problem.
From version 0.2.12 up to 0.2.15, the pyproject.toml is declared invalid.

I'm using Azure ADO artifact repository, so I use pyproject.toml to setup the repo URL.

Example pyproject.toml with tainted extra-index-url, it must fail.


```
[tool.poetry]
name = "nilo"
version = "0.1.0"
description = ""
authors = ["Nilo Menezes <x@mail.com>"]
readme = "README.md"

[tool.poetry.dependencies]
python = "^3.10"
rich = "^13.7.1"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"

[tool.uv.pip]
extra-index-url=["https://dummy.com"]
```

Run with uv 0.2.15:
```
$ uv pip compile pyproject.toml --verbose
DEBUG uv 0.2.15
DEBUG Starting toolchain discovery for any Python
DEBUG Looking for exact match for request any Python
DEBUG Searching for Python interpreter in system toolchains
DEBUG Found cpython 3.10.12 at `/home/nilo/.pyenv/shims/python3` (search path)
DEBUG Using Python 3.10.12 interpreter at /usr/bin/python3 for builds
DEBUG Using request timeout of 30s
DEBUG Acquired lock for `/home/nilo/.cache/uv/built-wheels-v3/path/0916cbb14bd2b497`
DEBUG Preparing metadata for: file:///home/nilo/tmp/
DEBUG No static `PKG-INFO` available for: file:///home/nilo/tmp/ (MissingPkgInfo)
DEBUG No static `pyproject.toml` available for: file:///home/nilo/tmp/ (PyprojectToml(FieldNotFound("project")))
INFO Ignoring empty directory
DEBUG Solving with installed Python version: 3.10.12
DEBUG Adding direct dependency: poetry-core*
DEBUG Found stale response for: https://pypi.org/simple/poetry-core/
DEBUG Sending revalidation request for: https://pypi.org/simple/poetry-core/
DEBUG Found not-modified response for: https://pypi.org/simple/poetry-core/
DEBUG Searching for a compatible version of poetry-core (*)
DEBUG Selecting: poetry-core==1.9.0 (poetry_core-1.9.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a1/8d/85fcf9bcbfefcc53a1402450f28e5acf39dcfde3aabb996a1d98481ac829/poetry_core-1.9.0-py3-none-any.whl.metadata
DEBUG Tried 1 versions: poetry-core 1
DEBUG Installing in poetry-core==1.9.0 in /home/nilo/.cache/uv/environments-v0/.tmpZXul8h
```

Run with uv 0.2.11:
```
$ uv pip compile pyproject.toml --verbose
DEBUG Starting toolchain discovery for any Python
DEBUG Looking for exact match for request any Python
DEBUG Searching for Python interpreter in all sources
DEBUG Found CPython 3.10.12 at `/home/nilo/.pyenv/shims/python3` (search path)
DEBUG Using Python 3.10.12 interpreter at /usr/bin/python3 for builds
DEBUG Using request timeout of 30s
DEBUG Acquired lock for `/home/nilo/.cache/uv/built-wheels-v3/path/0916cbb14bd2b497`
DEBUG Preparing metadata for: file:///home/nilo/tmp/
DEBUG No static `PKG-INFO` available for: file:///home/nilo/tmp/ (MissingPkgInfo)
DEBUG No static `pyproject.toml` available for: file:///home/nilo/tmp/ (PyprojectToml(FieldNotFound("project")))
INFO Ignoring empty directory
DEBUG Solving with installed Python version: 3.10.12
DEBUG Adding direct dependency: poetry-core*
DEBUG No cache entry for: https://dummy.com/poetry-core/
DEBUG Searching for a compatible version of poetry-core (*)
DEBUG No compatible version found for: poetry-core
```

With uv 0.2.15 and pyproject using poetry sections, build fails, as tool.uv.pip section is ignored.
Same pyproject.toml with uv 0.2.11, no problem at all.

It looks like some sections of pyproject are being ignored when the error (PyprojectToml(FieldNotFound("project"))) is reported.
Up to version 0.2.11, the tool.uv.pip section was used, even after (PyprojectToml(FieldNotFound("project"))). Starting on 0.2.12, the tool.uv.pip is ignored. Changing the pyproject to include a [project] sections fix the issue, but require the migration of the pyproject.toml and stop using poetry.





---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-25 16:42_

---

_Comment by @charliermarsh on 2024-06-25 16:42_

I can take a look at it. I'm surprised by this.

---

_Label `bug` added by @charliermarsh on 2024-06-25 16:45_

---

_Label `configuration` added by @charliermarsh on 2024-06-25 16:45_

---

_Comment by @charliermarsh on 2024-06-25 17:59_

Oh, we stopped reading configuration from `pyproject.toml` files that aren't part of the workspace (and that file doesn't comprise a valid workspace because it's lacking a `project` table), instead requiring the use of `uv.toml`. We may want to reconsider though for this use-case.


---

_Closed by @charliermarsh on 2024-06-25 18:53_

---

_Closed by @charliermarsh on 2024-06-25 18:53_

---
