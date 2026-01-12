```yaml
number: 16317
title: Calling bazel from setup.py
type: issue
state: closed
author: froody
labels:
  - question
assignees: []
created_at: 2025-10-15T15:58:27Z
updated_at: 2025-11-03T02:43:13Z
url: https://github.com/astral-sh/uv/issues/16317
synced_at: 2026-01-12T16:02:28Z
```

# Calling bazel from setup.py

---

_@froody_

### Question

I maintain a codebase that uses bazel (and rules_python) to manage running and testing a mix of Python and C++. I’d like to migrate to using 
pyproject.toml to be the source of truth for python deps, so that devs can do `uv run` and invoke pytest directly. The problem is I still need to invoke bazel to build some C++ Python extension modules, which is easy enough to do in setup.py, but in factoring out the bazel-specific bits into a reusable module, I find uv will often copy the sources into a temp directory in order to build, which makes it hard/impossible to get back to the original source tree in order to invoke bazel. I don’t want to have setup.py cloning my large repo, I want to use the existing checkout as there will often be local changes, but how do I safely/reliably pass file system paths needed for bazel through pyproject.toml/setup.py?

### Platform

macOS and Ubuntu 

### Version

_No response_

---

_Label `question` added by @froody on 2025-10-15 15:58_

---

_Comment by @konstin on 2025-10-20 12:29_

There's two ways to build a wheel for a package: Build the from the source tree (your repository) directly, or build a source distribution first, unpack that source distribution in a temporary directory and build the wheel from that source distribution. This is independent of whether a `pyproject.toml` exists, it depends on which PEP 517 mechanism the build tool (in a package manager) uses. You can do a direct build with uv with `uv build --wheel`.

---

_Closed by @charliermarsh on 2025-11-03 02:43_

---
