```yaml
number: 10992
title: "`uv add` silently ignores `--editable` when used with a Git/VCS dependency"
type: issue
state: closed
author: edmorley
labels:
  - bug
assignees: []
created_at: 2025-01-27T16:37:44Z
updated_at: 2025-01-27T19:37:25Z
url: https://github.com/astral-sh/uv/issues/10992
synced_at: 2026-01-10T04:27:58Z
```

# `uv add` silently ignores `--editable` when used with a Git/VCS dependency

---

_Issue opened by @edmorley on 2025-01-27 16:37_

### Summary

Hi

Currently `uv add` will silently drop the `--editable` option when used with a VCS URL, rather than warning or erroring about it not being supported.

This is low priority - I just happened to spot it as part of porting some of our existing pip/Poetry/Pipenv tests to uv.

### Steps

1. `uv init testcase && cd $_ && uv add --editable 'git+https://github.com/benoitc/gunicorn'`
2. `rg git pyproject.toml`

### Actual

`uv add` silently ignores `--editable` and adds the dependency to `[tool.uv.sources]` in `pyproject.toml` without the `editable = true`:

```
$ uv init testcase && cd $_ && uv add --editable 'git+https://github.com/benoitc/gunicorn'
Initialized project `testcase` at `/Users/emorley/src/testcase`
Using CPython 3.12.8 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtual environment at: .venv
Resolved 3 packages in 59ms
Installed 2 packages in 3ms
 + gunicorn==23.0.0 (from git+https://github.com/benoitc/gunicorn@bacbf8aa5152b94e44aa5d2a94aeaf0318a85248)
 + packaging==24.2

$ rg git pyproject.toml
12:gunicorn = { git = "https://github.com/benoitc/gunicorn" }
```

I checked the docs here but they don't mention that editable mode isn't supported for VCS/Git dependencies:
https://docs.astral.sh/uv/concepts/projects/dependencies/#editable-dependencies
https://docs.astral.sh/uv/reference/cli/#uv-add

I only found out it wasn't supported by manually adding `editable = true` to the gunicorn entry in `[tool.uv.sources]`, which then resulted in this error:

```
error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 12, column 12
   |
12 | gunicorn = { git = "https://github.com/benoitc/gunicorn", editable = true }
   |            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
cannot specify both `git` and `editable`
```

After some GitHub issue searching I found this which suggests this combination is intentionally not supported (which makes sense; it is a bit of a counter-intuitive combination):
https://github.com/astral-sh/uv/issues/5442

### Expected

One of:
- `uv add` to fail, with an error message saying ~`"--editable cannot be used with VCS/Git dependencies"`
- `uv add` to succeed, and omit the `editable = true` from the entry added to `[tool.uv.sources]`, but emit a warning saying ~`"ignoring --editable since it's not supported when using VCS/Git dependencies"`

For example, when using the `uv pip` interface, an error message is given:

```
$ uv pip install -e 'git+https://github.com/benoitc/gunicorn'
error: Editable must refer to a local directory, not a Git URL: `git+https://github.com/benoitc/gunicorn`
```

It would also be good to document that editable is only supported for path dependencies, at:
https://docs.astral.sh/uv/concepts/projects/dependencies/#editable-dependencies
https://docs.astral.sh/uv/reference/cli/#uv-add
https://docs.astral.sh/uv/pip/packages/#editable-packages

Plus it looks like this difference between `pip` and `uv pip` is missing from:
https://docs.astral.sh/uv/pip/compatibility/

### Context

I'm writing initial testcases for our support of uv, based on those I have already for our pip/Poetry/Pipenv support - one of which tests editable VCS installations, since:
- editable installs are particularly tricky in our legacy build system (since the builds occur at one path and then the assets replayed from another at app boot), so we have to perform path rewriting of eg `.pth` files, which has been known to need adjustments across package manager and/or setuptools version changes, so I'm extra cautious with testing
- the other package managers support the editable-VCS combination. (I personally think it's a combination that perhaps doesn't make sense when using a lockfile/managed environment, so it's fine to me if uv doesn't support it fwiw. And if uv doesn't support it, then I don't need tests for it etc.)

### Platform

macOS 15.2 ARM

### Version

uv 0.5.24 (Homebrew 2025-01-23)

### Python version

3.12.8

---

_Label `bug` added by @edmorley on 2025-01-27 16:37_

---

_Comment by @charliermarsh on 2025-01-27 16:53_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-27 17:41_

---

_Closed by @charliermarsh on 2025-01-27 19:37_

---

_Closed by @charliermarsh on 2025-01-27 19:37_

---
