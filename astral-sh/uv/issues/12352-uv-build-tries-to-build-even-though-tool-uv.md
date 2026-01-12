```yaml
number: 12352
title: "`uv build` tries to build even though `tool.uv.package` is False"
type: issue
state: closed
author: gregglind
labels:
  - bug
  - error messages
assignees: []
created_at: 2025-03-20T22:36:18Z
updated_at: 2025-04-02T13:33:29Z
url: https://github.com/astral-sh/uv/issues/12352
synced_at: 2026-01-12T16:01:01Z
```

# `uv build` tries to build even though `tool.uv.package` is False

---

_@gregglind_

### Summary

### Observed:

1.  `pyproject` with tool.uv.package = false
2.  `uv build` will attempt to build

> Building source distribution...
error: Multiple top-level packages discovered in a flat-layout: [....

### Expected

Exit success with a warning that it can't build packages if tool.uv.package = false.

https://docs.astral.sh/uv/concepts/projects/config/#project-packaging,

> Setting tool.uv.package = false will force a project package not to be built and installed into the project environment. uv will ignore a declared build system when interacting with the project

### Use Case

1.  Roots of monorepos support *some* uv actions (`sync` `add`) but not others (`build`).  

### Related:

https://github.com/astral-sh/uv/issues/10204



### Platform

mac os arm64

### Version

uv 0.6.8 (Homebrew 2025-03-18)

### Python version

Python 3.13.0

---

_Label `bug` added by @gregglind on 2025-03-20 22:36_

---

_Comment by @zanieb on 2025-03-20 22:38_

`tool.uv.package` is intended to toggle implicit builds, like during `uv sync` and `uv run` — `uv build` is an explicit attempt to build the project and will ignore this flag as the command otherwise would do nothing. We could add a warning or improve the error message, but this is "working as intended".

---

_Label `error messages` added by @zanieb on 2025-03-20 22:38_

---

_Comment by @zanieb on 2025-03-20 22:38_

cc @konstin 

---

_Comment by @pizkaz on 2025-03-31 15:01_

We just bumped into this as well using [uv2nix](https://github.com/pyproject-nix/uv2nix) to manage dependencies for a "non-package" python project (or rather: environment).

For anyone running into the same problem: Adding the following section to `pyproject.toml` did the job for us:
```toml
[tool.setuptools]
py-modules = []
```

---

_Comment by @gregglind on 2025-03-31 15:37_

Solution (alternative):

- My way of tackling it was to use a yaml checker for that field before the process that calls `uv build`.  

---

_Comment by @gregglind on 2025-03-31 15:41_

Re: improvements to the error message.

I learned something from this discussion:  the concepts of **implicit** and **explicit** builds or installs.  

I was mostly led astray by the docs mentioned in the initial report.  Perhaps, add 

"Note:  the setting only affects implicit builds.  Explicitly calling`uv build` will attempt to build the package."


---

_Comment by @zanieb on 2025-04-01 20:50_

Added in https://github.com/astral-sh/uv/pull/12608

---

_Closed by @zanieb on 2025-04-02 13:33_

---

_Closed by @zanieb on 2025-04-02 13:33_

---
