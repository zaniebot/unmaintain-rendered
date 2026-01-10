```yaml
number: 17118
title: Avoid allowing package-versions with no wheels in supported environments
type: pull_request
state: open
author: charliermarsh
labels:
  - bug
assignees: []
draft: true
base: main
head: charlie/plat
created_at: 2025-12-13T02:58:50Z
updated_at: 2025-12-14T21:55:56Z
url: https://github.com/astral-sh/uv/pull/17118
synced_at: 2026-01-10T05:49:14Z
```

# Avoid allowing package-versions with no wheels in supported environments

---

_Pull request opened by @charliermarsh on 2025-12-13 02:58_

## Summary

Closes https://github.com/astral-sh/uv/issues/17067.

Closes https://github.com/astral-sh/uv/issues/17060.


---

_Review requested from @konstin by @charliermarsh on 2025-12-13 02:58_

---

_Label `bug` added by @charliermarsh on 2025-12-13 02:58_

---

_Comment by @charliermarsh on 2025-12-13 03:43_

A bunch of test failures here but they might be correct, since they all represent us producing lockfiles with at least one empty entry (typically for something like `torch`).

---

_Comment by @charliermarsh on 2025-12-13 03:52_

(I think it's a bit too strict actually, playing with it now.)

---

_Comment by @konstin on 2025-12-14 21:55_

A less strict formulation: Determine which maker in `tool.uv.environments` the current `env` is a sub-environment of (the marker of `env` is contained in the marker in `tool.uv.environments`), and then check the package against that marker. This ensures that the `env` narrowing from additional forking doesn't fail a resolution that would otherwise pass. This may be functionally equivalent to making `tool.uv.environments` being treated as required-environments.

There's also a problem I'm not sure we can solve: We only know whether a package will have no distributions in the lockfile after we've done the entire resolution: Consider a package that has only a macOS arm64 wheels, no source dist. The first fork we process is macOS x86_64. When we process the package, it has no distributions. The final fork we process is macOS arm64. Depending on whether we see a dependency on the package in that fork, it may or may not have no distributions in the lockfile. In the scenario, the user may or may not have set `tool.uv.environments` to `sys_platform == "darwin"`, this problem should affect either case. It should also work without forking, by only considering two conditional dependencies with macOS arm64 and macOS x86_64 markers respectively.

---
