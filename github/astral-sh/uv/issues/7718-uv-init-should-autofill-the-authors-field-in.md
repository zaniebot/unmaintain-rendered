---
number: 7718
title: "`uv init` should autofill the `authors` field in pyproject.toml"
type: issue
state: closed
author: Ravencentric
labels: []
assignees: []
created_at: 2024-09-26T17:02:52Z
updated_at: 2024-10-08T19:06:39Z
url: https://github.com/astral-sh/uv/issues/7718
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv init` should autofill the `authors` field in pyproject.toml

---

_Issue opened by @Ravencentric on 2024-09-26 17:02_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

## Feature Request

`poetry init` autofills the `authors` field:
```shell
❯ cat .\pyproject.toml
[tool.poetry]
name = "example"
version = "0.1.0"
description = ""
authors = ["Ravencentric <my@example.email>"]
```
I find this quite helpful. This is usually boiler-plate that I'll have to fill anyway and not something that frequently changes so I doubt this will be controversial either. Would love to see this in `uv init`.


## Wish
I would also like it if `uv init` would autofill [license](https://peps.python.org/pep-0639/#project-source-metadata) field. [MIT](https://choosealicense.com/licenses/mit/) seems to be the [popular option as of 2018](https://snyk.io/blog/over-10-of-python-packages-on-pypi-are-distributed-without-any-license/). This might be harder to get a consensus on so I don't mind if this gets rejected. This can also be tied to a specific mode, like say `--package` and `--lib` but not with `--app` since the former two are supposed to be published and the latter isn't. Prior art example for this would be `hatch` which upon `hatch new` creates this:
```toml
[project]
name = "hello"
dynamic = ["version"]
description = ''
readme = "README.md"
requires-python = ">=3.8"
license = "MIT"
keywords = []
authors = [
  { name = "Ravencentric", email = "my@example.email" },
]
classifiers = [
  "Development Status :: 4 - Beta",
  "Programming Language :: Python",
  "Programming Language :: Python :: 3.8",
  "Programming Language :: Python :: 3.9",
  "Programming Language :: Python :: 3.10",
  "Programming Language :: Python :: 3.11",
  "Programming Language :: Python :: 3.12",
  "Programming Language :: Python :: Implementation :: CPython",
  "Programming Language :: Python :: Implementation :: PyPy",
]
dependencies = []
```
along with the `license.txt`
```shell
hello
├── src
│   └── hello
│       ├── __about__.py
│       └── __init__.py
├── tests
│   └── __init__.py
├── LICENSE.txt
├── README.md
└── pyproject.toml
```

---

_Renamed from "`uv init` should add `author` field in pyproject.toml" to "`uv init` should autofill the `authors` field in pyproject.toml" by @Ravencentric on 2024-09-26 17:03_

---

_Comment by @charliermarsh on 2024-09-27 12:25_

How would we determine the `authors` in this case?

---

_Comment by @Ravencentric on 2024-09-27 13:06_

So I just checked poetry, pdm, and hatch. Both poetry and hatch pull the author from `git`. pdm is an exception here since pdm's init interactively asks the user for author instead.

## Case 1: No git on the system OR unconfigured git

You can check that git (if installed) doesn't have a configured author by running:
```shell
$ git config user.name # no output
$ git config user.email # no output
```

Both hatch and poetry use a placeholder in this case:

- poetry
    ```
    $ poetry new --src hello-poetry && cat hello-poetry/pyproject.toml
    ```
    ```toml
    [tool.poetry]
    name = "hello-poetry"
    version = "0.1.0"
    description = ""
    authors = ["Your Name <you@example.com>"]
    readme = "README.md"
    packages = [{include = "hello_poetry", from = "src"}]

    [tool.poetry.dependencies]
    python = "^3.12"


    [build-system]
    requires = ["poetry-core"]
    build-backend = "poetry.core.masonry.api"
    ```

- hatch
    ```shell
    $ hatch new hello-hatch && cat hello-hatch/pyproject.toml
    ```

    ```toml
    [build-system]
    requires = ["hatchling"]
    build-backend = "hatchling.build"

    [project]
    name = "hello-hatch"
    dynamic = ["version"]
    description = ''
    readme = "README.md"
    requires-python = ">=3.8"
    license = "MIT"
    keywords = []
    authors = [
    { name = "U.N. Owen", email = "void@some.where" },
    ]
    classifiers = [
    "Development Status :: 4 - Beta",
    "Programming Language :: Python",
    "Programming Language :: Python :: 3.8",
    "Programming Language :: Python :: 3.9",
    "Programming Language :: Python :: 3.10",
    "Programming Language :: Python :: 3.11",
    "Programming Language :: Python :: 3.12",
    "Programming Language :: Python :: Implementation :: CPython",
    "Programming Language :: Python :: Implementation :: PyPy",
    ]
    dependencies = []
    ```

## Case 2: Configured git exists on the system

Configure git:
```
$ git config --global user.name "raven"
$ git config --global user.email "raven@email.com"
$ git config user.name
raven
$ git config user.email
raven@email.com
```
Now both poetry and hatch will pull the author field from the above git configuration:

- poetry

    ```shell
    $ poetry new --src hello-poetry && cat hello-poetry/pyproject.toml
    ```
    ```toml
    [tool.poetry]
    name = "hello-poetry"
    version = "0.1.0"
    description = ""
    authors = ["raven <raven@email.com>"]
    readme = "README.md"
    packages = [{include = "hello_poetry", from = "src"}]

    [tool.poetry.dependencies]
    python = "^3.12"


    [build-system]
    requires = ["poetry-core"]
    build-backend = "poetry.core.masonry.api"
    ```
- hatch

    ```shell
    $ hatch new hello-hatch && cat hello-hatch/pyproject.toml
    ```
    ```toml
    [build-system]
    requires = ["hatchling"]
    build-backend = "hatchling.build"

    [project]
    name = "hello-hatch"
    dynamic = ["version"]
    description = ''
    readme = "README.md"
    requires-python = ">=3.8"
    license = "MIT"
    keywords = []
    authors = [
    { name = "raven", email = "raven@email.com" },
    ]
    classifiers = [
    "Development Status :: 4 - Beta",
    "Programming Language :: Python",
    "Programming Language :: Python :: 3.8",
    "Programming Language :: Python :: 3.9",
    "Programming Language :: Python :: 3.10",
    "Programming Language :: Python :: 3.11",
    "Programming Language :: Python :: 3.12",
    "Programming Language :: Python :: Implementation :: CPython",
    "Programming Language :: Python :: Implementation :: PyPy",
    ]
    dependencies = []
    ```

---

_Comment by @Ravencentric on 2024-09-27 13:14_

Relevant poetry snippet: 
https://github.com/python-poetry/poetry/blob/150ff09e5609f9ea9f5502e7f8b4720458e5126b/src/poetry/console/commands/init.py#L152-L157

Relevant hatch snippet(s):
https://github.com/pypa/hatch/blob/5352e4422636cf1238017a74f0c67d689ccee558/src/hatch/config/model.py#L460-L483
https://github.com/pypa/hatch/blob/5352e4422636cf1238017a74f0c67d689ccee558/src/hatch/config/model.py#L491-L514

---

_Referenced in [astral-sh/uv#7756](../../astral-sh/uv/pulls/7756.md) on 2024-09-28 11:34_

---

_Comment by @FishAlchemist on 2024-09-28 16:45_

As I prefer not to expose my personal email in the Git commit history, I usually set up accounts with  email addresses like ``@users.noreply.github.com`` and ``@users.noreply.gitlab.com``.
Therefore, I hope that when encountering these two types of email addresses, they won't be set as the author's email, or a warning will be initialized.

---

_Comment by @dpprdan on 2024-09-28 19:59_

> How would we determine the `authors` in this case?

> pdm's init interactively asks the user for author instead.

I'd prefer pdm's approach (also w.r.t. @FishAlchemist's comment), plus a way to store this info in the user-level configuration (e.g., `~/.config/uv/uv.toml`).

For a non-Python precedent see [`use_description()` from the {usethis} R 📦](https://usethis.r-lib.org/reference/use_description.html) (especially the paragraph starting with "If you create a lot of packages, ..."). lt can also set a licence and package (or project) language. (In R, the `DESCRIPTION` file is the equivalent to `pyproject.toml`.)

---

_Comment by @Ravencentric on 2024-09-28 20:40_

I like the idea of being able to define commonly shared project metadata in user level uv.toml

---

_Comment by @zanieb on 2024-09-29 15:24_

Defining metadata in the `uv.toml` seems nice, as does reading from `git` by default. I think that we won't make this command interactive just for this feature, but I think an `--interactive / -i` flag would be nice in the future.

---

_Closed by @zanieb on 2024-10-08 19:06_

---

_Closed by @zanieb on 2024-10-08 19:06_

---

_Referenced in [astral-sh/uv#8098](../../astral-sh/uv/issues/8098.md) on 2024-10-10 15:48_

---
