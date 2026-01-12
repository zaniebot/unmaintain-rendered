```yaml
number: 6941
title: Dependency resolution unnecessarily falls back to a very old pylint version
type: issue
state: closed
author: hofrob
labels:
  - question
assignees: []
created_at: 2024-09-02T19:19:13Z
updated_at: 2024-09-10T11:19:50Z
url: https://github.com/astral-sh/uv/issues/6941
synced_at: 2026-01-12T15:59:09Z
```

# Dependency resolution unnecessarily falls back to a very old pylint version

---

_@hofrob_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

# versions

- ubuntu: 22.04
- uv: 0.4.2

# pyproject.toml

```toml
[build-system]
requires = [
    "setuptools",
    "wheel",
]

[project]
name = "uv-pylint-resolution"
version = "1"
requires-python = ">=3.12"
optional-dependencies.dev = [
    "pylint[spelling]",
]
optional-dependencies.test = [
    "astroid",
]
```

# command

Run `uv sync --all-extras` installs the following dependency tree: 

```
pylint v2.3.0
├── astroid v3.3.2
├── isort v5.13.2
└── mccabe v0.7.0
```

There is no reason to use pylint 2.3.0 as far as I can see. The current pylint version is 3.2.7.

# `uv sync --verbose --all-extras`

[uv_pylint_verbose.txt](https://github.com/user-attachments/files/16840777/uv_pylint_verbose.txt)



---

_Comment by @charliermarsh on 2024-09-02 19:31_

You get the resolution you expect with:
```toml
[build-system]
requires = [
    "setuptools",
    "wheel",
]

[project]
name = "uv-pylint-resolution"
version = "1"
requires-python = ">=3.12"
optional-dependencies.dev = [
    "pylint[spelling]",
]
optional-dependencies.test = [
    "astroid<3.3.0",
]
```

The issue is that the resolver is prioritizing solving for `astroid`, so you get a newer version of `astroid` (`3.3.2`) than is supported by newer versions of `pylint`. This isn't any less valid than a resolution that gives you `pylint==v3.2.7` and `astroid==v3.2.4`, though that resolution makes more sense from a user perspective.


---

_Comment by @charliermarsh on 2024-09-02 19:32_

This also works, perhaps more intuitive:

```toml
[build-system]
requires = [
    "setuptools",
    "wheel",
]

[project]
name = "uv-pylint-resolution"
version = "1"
requires-python = ">=3.12"
optional-dependencies.dev = [
    "pylint[spelling]>=3",
]
optional-dependencies.test = [
    "astroid",
]
```

---

_Label `question` added by @charliermarsh on 2024-09-02 19:32_

---

_Comment by @hofrob on 2024-09-02 19:48_

Ok, the problem is that `astroid` is an explicit dependency. If it wouldn't be, uv would solve for the newest pylint version and get whatever `astroid` version it requires. Makes sense.

I can't come up with a better idea on how to solve this (that's also simple), so I guess this issue can be closed.

FTR: In those cases, you could calculate a score from the version numbers. And then use the combination of packages with the highest score. **But** who knows what side effects this would produce and how long it would take.

---

_Closed by @hofrob on 2024-09-02 19:48_

---

_Comment by @hofrob on 2024-09-10 11:19_

I've had a similar problem with [vcrpy](https://pypi.org/project/vcrpy/) and its outdated pytest plugin [pytest-vcr](https://pypi.org/project/pytest-vcr/). 

---
