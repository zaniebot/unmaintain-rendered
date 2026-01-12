```yaml
number: 11601
title: Include environment variables in interpreter info caching
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/cache-python-interpreter-by-env-vars
created_at: 2025-02-18T15:43:22Z
updated_at: 2025-02-19T10:10:20Z
url: https://github.com/astral-sh/uv/pull/11601
synced_at: 2026-01-12T16:09:54Z
```

# Include environment variables in interpreter info caching

---

_@konstin_

We want to use `sys.path` for package discovery (#2500, #9849). For that, we need to know the correct value of `sys.path`. `sys.path` is a runtime-changeable value, which gets influenced from a lot of different sources: Environment variables, CLI arguments, `.pth` files with scripting, `sys.path.append()` at runtime, a distributor patching Python, etc. We cannot capture them all accurately, especially since it's possible to change `sys.path` mid-execution. Instead, we do a best effort attempt at matching the user's expectation.

The assumption is that package installation generally happens in venv site-packages, system/user site-packages (including pypy shipping packages with std), and `PYTHONPATH`. Specifically, we reuse `PYTHONPATH` as dedicated way for users to tell uv to include specific directories in package discovery.

A common way to influence `sys.path` that is not using venvs is setting `PYTHONPATH`. To support this we're capturing `PYTHONPATH` as part of the cache invalidation, i.e. we refresh the interpreter metadata if it changed. For completeness, we're also capturing other environment variables documented as influencing `sys.path` or other fields in the interpreter info.

This PR does not include reading registry values for `sys.path` additions on Windows as documented in https://docs.python.org/3.11/using/windows.html#finding-modules. It notably also does not include parsing of python CLI arguments, we only consider their environment variable versions for package installation and listing. We could try parsing CLI flags in `uv run python`, but we'd still miss them when Python is launched indirectly through a script, and it's more consistent to only consider uv's own arguments and environment variables, similar to uv's behavior in other places.

---

_Label `enhancement` added by @konstin on 2025-02-18 15:43_

---

_@zanieb approved on 2025-02-18 21:26_

---

_@zanieb reviewed on 2025-02-18 21:27_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:748 on 2025-02-18 21:27_

Technically, we should add all these to the static `EnvVars` declarations so we can document what we read these for. Could you do that please?

---

_@konstin reviewed on 2025-02-19 09:28_

---

_Review comment by @konstin on `crates/uv-python/src/interpreter.rs`:748 on 2025-02-19 09:28_

oops, good catch!

---

_Merged by @konstin on 2025-02-19 10:10_

---

_Closed by @konstin on 2025-02-19 10:10_

---

_Branch deleted on 2025-02-19 10:10_

---
