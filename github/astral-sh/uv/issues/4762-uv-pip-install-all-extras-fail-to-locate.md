---
number: 4762
title: "`uv pip install --all-extras .` fail to locate `pyproject.toml`"
type: issue
state: closed
author: kdeldycke
labels:
  - question
assignees: []
created_at: 2024-07-03T10:03:15Z
updated_at: 2024-10-21T21:25:55Z
url: https://github.com/astral-sh/uv/issues/4762
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv pip install --all-extras .` fail to locate `pyproject.toml`

---

_Issue opened by @kdeldycke on 2024-07-03 10:03_

Using the latest version of `uv`:
```shell-session
$ uv --version
uv 0.2.21 (ebfe6d8fc 2024-07-03)
```

When I try to install in my local venv all extras, I get the following error:
```shell-session
$ uv pip install --all-extras .
error: Requesting extras requires a `pyproject.toml`, `setup.cfg`, or `setup.py` file.
``` 

Still, [I have a `pyproject.toml` at the root](https://github.com/kdeldycke/mail-deduplicate/blob/main/pyproject.toml), so I expect `uv` to use it:
```shell-session
$ ls -lah ./pyproject.toml
Permissions Size User Date Modified    Name
.rw-r--r--  4.5k kde  2024-07-03 10:16  ./pyproject.toml
```

This can be reproduced from scratch with my project with the following commands:
```shell-session
$ cd ~
$ git clone https://github.com/kdeldycke/mail-deduplicate
$ cd mail-deduplicate
$ uv venv
$ source .venv/bin/activate
$ uv pip install --all-extras .
```

This seems to be a follow up of #260.

---

_Comment by @charliermarsh on 2024-07-03 11:21_

That API requires that you pass the pyproject.toml directly on the command line, rather than the path to the source tree — since if you’re doing the latter, there’s already standardized syntax for it (putting the extras in brackets).

---

_Comment by @kdeldycke on 2024-07-03 12:03_

> That API requires that you pass the pyproject.toml directly on the command line, rather than the path to the source tree — since if you’re doing the latter, there’s already standardized syntax for it (putting the extras in brackets).

So there is no way to tell `uv` to install all extras, whatever their numbers or IDs?

I tried different ways like:
```shell-session
$ uv pip install ".[*]"
error: Failed to parse: `.[*]`
  Caused by: Expected an alphanumeric character starting the extra name, found '*'
.[*]
  ^
```
```shell-session
$ uv pip install ".[all]"
Resolved 23 packages in 1.13s
   Built meta-package-manager @ file:///Users/kde/meta-package-manager
Prepared 1 package in 797ms
Uninstalled 1 package in 0.93ms
Installed 1 package in 2ms
 - meta-package-manager==5.17.0 (from file:///Users/kde/meta-package-manager)
 + meta-package-manager==5.17.0 (from file:///Users/kde/meta-package-manager)
warning: The package `meta-package-manager @ file:///Users/kde/meta-package-manager` does not have an extra named `all`.
```

---

_Label `question` added by @zanieb on 2024-07-03 12:50_

---

_Comment by @charliermarsh on 2024-07-03 22:36_

The typical thing would be to define an extra called `all` in your package, and recursively reference the others, like:

```toml
[project]
name = "black"

[project.optional-dependencies]
colorama = ["colorama>=0.4.3"]
uvloop = ["uvloop>=0.15.2"]
all = [
  "black[d]",
  "black[uvloop]",
]
```

---

_Comment by @kdeldycke on 2024-07-05 06:18_

Oh ok I see. Thanks @charliermarsh for the hint. Now I get it. The `all` keyword is more like a convention than a technical feature. And so I'm pretty sure that this is not specified or enforced in any PEP XXX.

Given the diversity of packages I'm likely to stumble upon, and because I'm [trying to implement the most generic reusable workflows](https://github.com/kdeldycke/workflows/), I prefer the safety of the `--all-extras` parameter.

So for example, my strategy to implement [a generic `mypy` type checking job](https://github.com/kdeldycke/workflows/blob/495fb192b43dd54c91fe31c294576df6877612d8/.github/workflows/lint.yaml#L83-L113), consist in running the following command to get all dependencies installed, including the optional ones from the various [`typeshed`](https://github.com/python/typeshed) packages:

```shell-session
$ uv pip install --all-extras ${{ needs.project-metadata.outputs.uv_requirement_params }}
```

Where `needs.project-metadata.outputs.uv_requirement_params` is a variable produce by my `gha-utils` CLI:
```shell-session
$ pipx install gha-utils
$ gha-utils metadata
(...)
uv_requirement_params=--requirement pyproject.toml --requirement requirements.txt --requirement requirements/yamllint.txt --requirement requirements/mypy.txt --requirement requirements/format-python.txt --requirement requirements/build.txt --requirement requirements/nuitka.txt --requirement requirements/mdformat.txt --requirement requirements/pipdeptree.txt --requirement requirements/uv.txt --requirement requirements/changelog.txt --requirement requirements/gha-utils.txt
```

It's quite convoluted, but it works.

---

_Closed by @zanieb on 2024-10-21 21:25_

---

_Referenced in [astral-sh/uv#8416](../../astral-sh/uv/issues/8416.md) on 2024-10-21 21:26_

---

_Referenced in [pypa/pip#12963](../../pypa/pip/issues/12963.md) on 2024-10-26 16:03_

---

_Referenced in [langflow-ai/langflow#6732](../../langflow-ai/langflow/pulls/6732.md) on 2025-02-21 14:48_

---

_Referenced in [oraios/serena#9](../../oraios/serena/issues/9.md) on 2025-04-04 00:38_

---
