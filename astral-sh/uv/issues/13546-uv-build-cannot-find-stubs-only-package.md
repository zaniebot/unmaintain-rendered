```yaml
number: 13546
title: "`uv_build` cannot find stubs-only package"
type: issue
state: closed
author: jorenham
labels:
  - enhancement
assignees: []
created_at: 2025-05-19T21:56:36Z
updated_at: 2025-05-22T17:02:19Z
url: https://github.com/astral-sh/uv/issues/13546
synced_at: 2026-01-12T16:01:31Z
```

# `uv_build` cannot find stubs-only package

---

_@jorenham_

### Summary

in my `pyproject.toml` I have something like

```toml
[project]
name = "numpy-stubs"
# --snip--

[build-system]
requires = ["uv_build>=0.7.5,<0.8"]
build-backend = "uv_build"
# --snip--
```

then I run `uv build`, and see

```
Building source distribution...
Error: Missing module directory for `numpy_stubs` in `src`. Found: `numpy-stubs`, `numtype`, `_numtype`
  × Failed to build `/home/joren/Workspace/numtype`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `uv_build.build_sdist` failed (exit status: 1)
      hint: This usually indicates a problem with the package or the build environment.
```

I also tried 

```toml
[tool.uv.build-backend]
module-name = "numpy-stubs"
```

but `uv build` then says

```
Building source distribution...
  × Failed to build `/home/joren/Workspace/numtype`
  ├─▶ Failed to parse: `pyproject.toml`
  ╰─▶ TOML parse error at line 7, column 15
        |
      7 | module-name = "numpy-stubs"
        |               ^^^^^^^^^^^^^
      Invalid character `-` at position 6 for identifier `numpy-stubs`, expected an underscore or an alphanumeric character
```

For what it's worth; here's the `pyproject.toml` source:
https://github.com/numpy/numtype/blob/3bbf56e28b4bfb8812529c9f1f207e8f1cb01b17/pyproject.toml

### Platform

Ubuntu 22.04

### Version

0.7.5

### Python version

_No response_

---

_Label `bug` added by @jorenham on 2025-05-19 21:56_

---

_Label `bug` removed by @konstin on 2025-05-20 21:35_

---

_Label `enhancement` added by @konstin on 2025-05-20 21:35_

---

_Closed by @konstin on 2025-05-22 17:02_

---

_Closed by @konstin on 2025-05-22 17:02_

---
