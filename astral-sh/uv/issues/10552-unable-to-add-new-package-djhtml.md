```yaml
number: 10552
title: "Unable to add new package `djhtml`"
type: issue
state: closed
author: wgordon17
labels:
  - needs-mre
assignees: []
created_at: 2025-01-13T02:06:18Z
updated_at: 2025-01-13T02:39:16Z
url: https://github.com/astral-sh/uv/issues/10552
synced_at: 2026-01-12T16:00:15Z
```

# Unable to add new package `djhtml`

---

_@wgordon17_

```
$ uv version
uv 0.5.18 (27d1bad55 2025-01-11)

$ mkdir testing
$ cd testing
$ uv init
Initialized project `testing`

$ uv add "djhmtl" --verbose
DEBUG uv 0.5.18 (27d1bad55 2025-01-11)
DEBUG Found project root: `/Users/home/testing`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `/Users/home/testing/.python-version`
DEBUG Using Python request `3.11` from version file at `.python-version`
DEBUG The virtual environment's Python version satisfies `3.11`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: testing @ file:///Users/home/testing
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.11.10
DEBUG Solving with target Python version: >=3.11
DEBUG Adding direct dependency: testing*
DEBUG Searching for a compatible version of testing @ file:///Users/home/testing (*)
DEBUG Adding direct dependency: djhmtl*
DEBUG No cache entry for: https://pypi.org/simple/djhmtl/
DEBUG Checking netrc for credentials for https://pypi.org/simple/djhmtl/
DEBUG Searching for a compatible version of djhmtl (*)
DEBUG No compatible version found for: djhmtl
DEBUG Recording unit propagation conflict of djhmtl from incompatibility of (testing)
DEBUG Searching for a compatible version of testing @ file:///Users/home/testing (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: testing
DEBUG Reverting changes to `pyproject.toml`
DEBUG Removing `uv.lock`
  × No solution found when resolving dependencies:
  ╰─▶ Because djhmtl was not found in the package registry and your project depends on djhmtl, we can conclude that your project's requirements are
      unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.

$ uv add "djhmtl" --verbose --frozen
DEBUG uv 0.5.18 (27d1bad55 2025-01-11)
DEBUG Found project root: `/Users/home/testing`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `/Users/home/testing/.python-version`
DEBUG Using Python request `3.11` from version file at `.python-version`
DEBUG The virtual environment's Python version satisfies `3.11`
```

~~Using the `--frozen` as suggested does actually work, and it downloads/installs the package as expected, but it will then refuse to lock~~ (I forgot that I had tested with `pip install djhtml`, which _does_ work, and that was why the package was available locally. `uv add djhtml --frozen` does nothing but update `pyproject.toml`)

---

_Comment by @charliermarsh on 2025-01-13 02:12_

How are you configuring your index? 

---

_Comment by @wgordon17 on 2025-01-13 02:27_

I'm not...this is just a totally blank project, I've done `0` customizations to it ¯\\\_(ツ)_/¯ 

---

_Comment by @charliermarsh on 2025-01-13 02:35_

That package doesn't exist on PyPI: https://pypi.org/project/djhmtl/. So you must have a custom index configured with pip that's locating it?

---

_Label `needs-mre` added by @charliermarsh on 2025-01-13 02:35_

---

_Comment by @wgordon17 on 2025-01-13 02:39_

Ohhhh...bloody hell! 🤦‍♂️ Just a _really_ stupid typo on my part! I even spelled it right as `djhtml` everywhere else 🙄

---

_Closed by @wgordon17 on 2025-01-13 02:39_

---
