```yaml
number: 11123
title: "Failed to build: a package that installs with pip --use-pep517"
type: issue
state: closed
author: severance26
labels:
  - needs-mre
assignees: []
created_at: 2025-01-30T23:49:06Z
updated_at: 2025-01-31T15:15:16Z
url: https://github.com/astral-sh/uv/issues/11123
synced_at: 2026-01-12T16:00:28Z
```

# Failed to build: a package that installs with pip --use-pep517

---

_@severance26_

### Summary

Installation of a private repo fails in uv, succeeds in pip.

Attempt with uv:

```
(ntb-scripts) joey@debian:~/repo/nautobot/ntb-scripts$ uv add example-app --index gitlab=https://token:****@gitlab.com/api/v4/projects/666/packages/pypi/simple
Resolved 13 packages in 744ms
  × Failed to build `example-app @ file:///home/joey/repo/nautobot/ntb-scripts`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta.build_editable` failed (exit status: 1)

      [stdout]
      running egg_info

      [stderr]
      error: error in 'egg_base' option: 'src' does not exist or is not a directory

      hint: This usually indicates a problem with the package or the build environment.
```

src does exist in the directory, obviously.

Here is the pip attempt:

```
(venv) joey@debian:~/repo/test$ pip install --use-pep517 example-app --no-deps --index-url https://token:****gitlab.com/api/v4/projects/666/packages/pypi/simple
Looking in indexes: https://token:****@gitlab.com/api/v4/projects/666/packages/pypi/simple
Collecting example-app
  Using cached https://gitlab.com/api/v4/projects/666/packages/pypi/files/65fe38e/handler_nautobot-1.0.0-py3-none-any.whl (3.1 kB)
Installing collected packages: example-app
Successfully installed example-app-1.0.0
```

The directions on the uv webpage are pretty lengthy and making a Dockerfile for all this is time-prohibitive for me. However, I believe the pyproject.toml file should be enough for this, since the package installs in pip without issue.

```
[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"

[project]
name = "example-app"
version = "1.0.0"
description = "Example App."
requires-python = ">=3.11"
classifiers = [
    "Programming Language :: Python :: 3",
    "Operating System :: OS Independent",
]
dependencies = [
    "requests"
]

# Development dependencies
[project.optional-dependencies]
dev = [
    "coverage==7.6.1",
    "isort",
    "mypy>=1.8.0",
    "types-requests"    
    # Add other type stubs as needed
]

# setuptools configuration:
[tool.setuptools.packages.find]
where = ["src"]

[tool.setuptools.package-data]
"handler_name" = ["py.typed"]  # defines package as typed when imported by workflow

# Mypy global options:
[tool.mypy]
python_version = "3.11"
warn_return_any = true
warn_unused_configs = true

# Ruff configuration
[tool.ruff]
line-length = 120
target-version = "py311"

[tool.ruff.lint]
select = ["PL"]
ignore = ["PLR", "PLW"]
```

The code inside of the root folder is in src/example_app.




### Platform

Debian 12

### Version

0.5.25

### Python version

3.11.2

---

_Label `bug` added by @severance26 on 2025-01-30 23:49_

---

_Comment by @charliermarsh on 2025-01-31 02:33_

Unfortunately I can't reproduce this. I just copied your `pyproject.toml`, and then created `src/example_app/__init__.py`.

```
❯ uv sync
Using CPython 3.13.1
Creating virtual environment at: .venv
Resolved 12 packages in 198ms
   Built example-app @ file:///Users/crmarsh/workspace/puffin/foo
Prepared 2 packages in 502ms
Installed 6 packages in 3ms
 + certifi==2025.1.31
 + charset-normalizer==3.4.1
 + example-app==1.0.0 (from file:///Users/crmarsh/workspace/puffin/foo)
 + idna==3.10
 + requests==2.32.3
 + urllib3==2.3.0
```

---

_Label `bug` removed by @charliermarsh on 2025-01-31 02:34_

---

_Label `needs-mre` added by @charliermarsh on 2025-01-31 02:34_

---

_Comment by @charliermarsh on 2025-01-31 02:35_

What is `example-app` in your comment? Is that a different project than `ntb-scripts`?

---

_Comment by @charliermarsh on 2025-01-31 02:36_

I get that error if I remove the `src` directory. Is that your problem?

```
❯ uv sync --python 3.11 --reinstall -n
Resolved 12 packages in 0.43ms
  × Failed to build `example-app @ file:///Users/crmarsh/workspace/puffin/foo`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta.build_editable` failed (exit status: 1)

      [stdout]
      running egg_info

      [stderr]
      error: error in 'egg_base' option: 'src' does not exist or is not a directory

      hint: This usually indicates a problem with the package or the build environment.
```

---

_Comment by @severance26 on 2025-01-31 12:59_

> What is `example-app` in your comment? Is that a different project than `ntb-scripts`?

Its the same, just trying to whitewash the example.

> I get that error if I remove the src directory. Is that your problem?

No, I most certainly have the src directory in there, its the same package thats installing with pip. It is also unittested and a template that I use often. I've never had trouble with it (pip, poetry) until attempting to install one of my own (uv-built) handlers with `uv add`.

This weekend I will try to make a Dockerfile of it, or otherwise fine a way around (a friend suggested moving away from uv add), and update or close this thread.

---

_Comment by @charliermarsh on 2025-01-31 14:33_

It doesn't need to be a complete Dockerfile. Even just a Git repository that I can clone is fine.

---

_Comment by @severance26 on 2025-01-31 15:15_

I am unable to reproduce this outside of this one package. I just built a test package with the same structure and it succeeded. Will have to re-evaluate.

---

_Closed by @severance26 on 2025-01-31 15:15_

---
