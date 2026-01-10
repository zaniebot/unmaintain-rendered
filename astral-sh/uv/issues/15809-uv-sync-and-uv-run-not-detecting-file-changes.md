---
number: 15809
title: "`uv sync` and `uv run` not detecting file changes when using `scikit_build_core.build` backend"
type: issue
state: open
author: alechouse97
labels:
  - question
assignees: []
created_at: 2025-09-12T12:45:46Z
updated_at: 2025-09-12T15:12:55Z
url: https://github.com/astral-sh/uv/issues/15809
synced_at: 2026-01-10T01:26:00Z
---

# `uv sync` and `uv run` not detecting file changes when using `scikit_build_core.build` backend

---

_Issue opened by @alechouse97 on 2025-09-12 12:45_

### Summary

We have an all python code that we are compiling to cython modules (all `.so` instead of `.py`) upon delivery to customers/users. I was able to get this part up an running quickly using the `scikit_build_core.build` backend. However, I have run into two issues that I have been unable to solve when it comes to working on the project:

1. After making a source code change, running `uv sync` does not recompile the package, so these changes are never reflected. 
2. When running a file using `uv run`, the package is only recompiled if the file you are running has changed, but not any other modules in the package.

Ideally, when using the `scikit_build_core.build` backend, uv should detect source code file changes and recompile the package.

Or, and this would probably be preferred in my case since the package itself is python only, have a flag or option for `uv sync` or `run` or in the `pyproject.toml` where the code not compiled, and just installed as raw python. Then we only compile with cython when running `uv build`.


### Platform

RHEL 9 x86_64

### Version

uv 0.8.17

### Python version

Python 3.13.6

---

_Label `bug` added by @alechouse97 on 2025-09-12 12:45_

---

_Label `bug` removed by @konstin on 2025-09-12 13:08_

---

_Label `enhancement` added by @konstin on 2025-09-12 13:08_

---

_Comment by @zanieb on 2025-09-12 13:09_

It sounds like you want to configure https://docs.astral.sh/uv/reference/settings/#cache-keys ?

---

_Comment by @konstin on 2025-09-12 13:09_

uv can't detect what files are being used by scikit-build-learn, so we're limited in how we can detect when and what to recompile. We're improving the situation in https://github.com/astral-sh/uv/pull/15705 by setting better cache key defaults that rebuild when source files change.

---

_Comment by @alechouse97 on 2025-09-12 14:09_

@zanieb Yep that got it!

This might be a stretch, but is there a way to treat the project with the normal `uv_build` backend approach when using `uv run` and `uv sync`, but then perform the cython compilation just when running `uv build`? We are basically looking for a way to just compile the code when building it into a wheel, but keep it pure python during normal development.

---

_Label `enhancement` removed by @charliermarsh on 2025-09-12 14:57_

---

_Label `question` added by @charliermarsh on 2025-09-12 14:57_

---

_Comment by @konstin on 2025-09-12 15:12_

Do you mean like https://github.com/astral-sh/uv/issues/15801?

---
