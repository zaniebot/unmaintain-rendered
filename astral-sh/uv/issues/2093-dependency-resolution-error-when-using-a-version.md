---
number: 2093
title: Dependency resolution error when using a version not yet on PyPI
type: issue
state: closed
author: Andrew-S-Rosen
labels: []
assignees: []
created_at: 2024-02-29T18:29:44Z
updated_at: 2024-04-01T21:31:52Z
url: https://github.com/astral-sh/uv/issues/2093
synced_at: 2026-01-10T01:23:12Z
---

# Dependency resolution error when using a version not yet on PyPI

---

_Issue opened by @Andrew-S-Rosen on 2024-02-29 18:29_

I have a Python package (`quacc`) that has a required dependency (`ase`). The most recent version of `ase` on PyPI is 3.22.1, but the `master` branch of `ase` is >3.22.1. Since the `quacc` package relies on features added since the last `ase` release, in the `dependencies` list in `pyproject.toml` of my `quacc` package, I have `ase>3.22.1` to ensure that users won't have a silent failure when installing the package if they have an out-of-date dependency. However, `uv` does not like this at all (even though `pip` is perfectly content).

Reproduction with uv 0.1.13 on Ubuntu in a clean miniconda environment:

```bash
uv pip install "ase @ git+https://gitlab.com/ase/ase.git" # installs >3.22.1
uv pip install "quacc @ git+https://github.com/Quantum-Accelerators/quacc.git" # or `uv pip install quacc`

```

Traceback:
```
 Updated https://github.com/Quantum-Accelerators/quacc.git (8c55f09)                     × No solution found when resolving dependencies:
  ╰─▶ Because only ase<=3.22.1 is available and quacc==0.6.8 depends on ase>3.22.1,
      we can conclude that quacc==0.6.8 cannot be used.
      And because only quacc==0.6.8 is available and you require quacc, we can
      conclude that the requirements are unsatisfiable.
```

While it is true that only `ase<=3.22.1` is available on PyPI, the current Python environment has `ase>3.22.1`, and that should have priority. `uv` should not try to be installing `ase` since it's already in my environment.

---

_Comment by @charliermarsh on 2024-02-29 18:59_

I think this roughly the same as https://github.com/astral-sh/uv/issues/1661, which is: we don't use the `site-packages` directory of already-installed packages as a package _source_ when resolving.

---

_Closed by @charliermarsh on 2024-02-29 18:59_

---

_Comment by @Andrew-S-Rosen on 2024-02-29 19:04_

Thanks! You're right. That's the same issue, I believe. 

---

_Referenced in [astral-sh/uv#2596](../../astral-sh/uv/pulls/2596.md) on 2024-03-21 21:49_

---

_Comment by @zanieb on 2024-04-01 21:31_

Hi! This should be addressed in the latest release ([0.1.27](https://github.com/astral-sh/uv/releases/tag/0.1.27)) via #2596.

Let us know if you have any problems.

---
