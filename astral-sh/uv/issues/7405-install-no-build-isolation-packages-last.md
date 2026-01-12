```yaml
number: 7405
title: Install no-build-isolation-packages last
type: issue
state: open
author: bluss
labels:
  - enhancement
assignees: []
created_at: 2024-09-15T12:10:31Z
updated_at: 2024-12-03T15:29:29Z
url: https://github.com/astral-sh/uv/issues/7405
synced_at: 2026-01-12T15:59:13Z
```

# Install no-build-isolation-packages last

---

_@bluss_

Feature request/suggestion: If there is a package marked no-build-isolation-package, install it after everything else. This makes uv sync more likely to work without any extra actions (extra uv pip install invocations).

The no-build-isolation-package could be the current workspace. For example, in `uv sync` install all other deps, dev-deps and workspace members before installing any package marked no-build-isolation-package.  (Advanced version: read build-system.requires and ensure at least these requirements are installed, if they exist, before any packages depend on them.)

---

_Comment by @zanieb on 2024-09-15 14:41_

Don't we need to build the packages to determine their metadata to perform resolution? I don't think we can necessarily install the build requirements first without performing two resolutions which sounds like a non-starter. Reading the build requirements first makes sense, though it may be hard to implement. I think @charliermarsh wants to do some work around locking build requirements and defining a graph of build requirements so maybe we'd solve this via further declarations instead?

---

_Label `enhancement` added by @zanieb on 2024-09-15 14:41_

---

_Comment by @bluss on 2024-09-15 15:24_

In the case I was working on (I don't know if it's a good idea, but a project using maturin normally as a build dep, now as dev-dep), it can install using these steps:

```
rm -r .venv uv.lock
uv sync --no-install-project -v --no-cache -U
uv sync -v --no-build-isolation-package project-name
```

And the log prints the following for the first `uv sync`:

```
Found static `pyproject.toml` for: project-name
```

I think that shows that in this case - static metadata, building the project is not required to know the deps, and uv can already do it. Would be great if the required ordering was automatic (maybe?). In this case it's "not the project", then the full sync, but in other cases it might be all deps except one, then the last one.

---

_Comment by @bluss on 2024-10-01 10:23_

@charliermarsh do you know if this is possible? If it doesn't solve the problem then it's of course not necessary. But it could help resolve some simple situations, where you want uv to first install maturin, setuptools, whatever, then the no-build-isolation-package.

---

_Comment by @charliermarsh on 2024-10-01 14:39_

I think it's possible.

---

_Comment by @zanieb on 2024-12-03 15:28_

@WonsukYangTrlbs please do not reply to issues with status requests. If there's no commentary, there's probably no progress. See #9452 for guidelines on interacting with the issue tracker.

---
