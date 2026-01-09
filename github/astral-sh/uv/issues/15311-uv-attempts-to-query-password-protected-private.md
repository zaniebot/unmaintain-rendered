---
number: 15311
title: uv attempts to query password-protected private PyPI index even if only optional dependencies require the index
type: issue
state: closed
author: ntamas
labels:
  - bug
assignees: []
created_at: 2025-08-15T14:22:03Z
updated_at: 2025-08-15T19:36:07Z
url: https://github.com/astral-sh/uv/issues/15311
synced_at: 2026-01-07T13:12:19-06:00
---

# uv attempts to query password-protected private PyPI index even if only optional dependencies require the index

---

_Issue opened by @ntamas on 2025-08-15 14:22_

### Summary

Our use-case is as follows:

* We have a Python project where almost all the dependencies are available from the public PyPI index
* Some _optional_ dependencies exist only in a _private_, password-protected PyPI index
* The private index is marked as `explicit = true`, and all optional dependencies that are to be fetched from this private index have a corresponding entry in `[tool.uv.sources]`

Our goal is to allow people who check out the project from `git` to be able to run `uv sync` and install all the _non-optional_ dependencies even if they do not have access to the private PyPI repository (which contains _optional_) dependencies only. However, we have found that `uv sync` attempts to talk to the private repository even if we do not ask for any of the extras, and this fails if the user does not have the credentials for the private PyPI index.

Minimal repro: run `uv init` in an empty directory and add the following `pyproject.toml` file:

```toml
[project]
name = "uv-dep-repro"
version = "0.1.0"
description = ""
requires-python = ">=3.13"
dependencies = [
    "trio >= 0.30.0"
]

[project.optional-dependencies]
some_extra = ["some-package-from-private-index>=0.1.0"]

[tool.uv.sources]
some-package-from-private-index = { index = "collmot" }

[[tool.uv.index]]
name = "collmot"
url = "https://pypi.collmot.com/"
explicit = true
```

Output of `uv sync --verbose`:

```
DEBUG uv 0.8.10 (7e817bafd 2025-08-13)
DEBUG Found project root: `/home/tamas/git/uv-dep-repro`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/home/tamas/git/uv-dep-repro`
DEBUG Reading Python requests from version file at `/home/tamas/git/uv-dep-repro/.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.13`
DEBUG Released lock at `/tmp/uv-6cfccf177770ce1c.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: uv-dep-repro @ file:///home/tamas/git/uv-dep-repro
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.5
DEBUG Solving with target Python version: >=3.13
DEBUG Adding direct dependency: uv-dep-repro[some-extra]*
DEBUG Searching for a compatible version of uv-dep-repro @ file:///home/tamas/git/uv-dep-repro (*)
DEBUG Adding direct dependency: uv-dep-repro==0.1.0
DEBUG Adding direct dependency: uv-dep-repro[some-extra]==0.1.0
DEBUG Searching for a compatible version of uv-dep-repro @ file:///home/tamas/git/uv-dep-repro (==0.1.0)
DEBUG Adding direct dependency: trio>=0.30.0
DEBUG Searching for a compatible version of uv-dep-repro @ file:///home/tamas/git/uv-dep-repro (==0.1.0)
DEBUG Adding direct dependency: some-package-from-private-index>=0.1.0
DEBUG Found fresh response for: https://pypi.org/simple/trio/
DEBUG No cache entry for: https://pypi.org/simple/some-package-from-private-index/
DEBUG Searching for a compatible version of trio (>=0.30.0)
DEBUG Selecting: trio==0.30.0 [compatible] (trio-0.30.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/69/8e/3f6dfda475ecd940e786defe6df6c500734e686c9cd0a0f8ef6821e9b2f2/trio-0.30.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for trio==0.30.0: attrs>=23.2.0
DEBUG Adding transitive dependency for trio==0.30.0: cffi{implementation_name != 'pypy' and os_name == 'nt'}>=1.14
DEBUG Adding transitive dependency for trio==0.30.0: idna*
DEBUG Adding transitive dependency for trio==0.30.0: outcome*
DEBUG Adding transitive dependency for trio==0.30.0: sniffio>=1.3.0
DEBUG Adding transitive dependency for trio==0.30.0: sortedcontainers*
DEBUG Found fresh response for: https://pypi.org/simple/attrs/
DEBUG Found fresh response for: https://pypi.org/simple/idna/
DEBUG Found fresh response for: https://pypi.org/simple/outcome/
DEBUG Found fresh response for: https://pypi.org/simple/sortedcontainers/
DEBUG Found fresh response for: https://pypi.org/simple/sniffio/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/77/06/bb80f5f86020c4551da315d78b3ab75e8228f89f0162f2c3a819e407941a/attrs-25.3.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/55/8b/5ab7257531a5d830fc8000c476e63c935488d74609b50f9384a643ec0a62/outcome-1.3.0.post0-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/32/46/9cb0e58b2deb7f82b84065f37f3bffeb12413f947f9388e4cac22c4621ce/sortedcontainers-2.4.0-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/76/c6/c88e154df9c4e1a2a66ccf0005a88dfb2650c1dffb6f5ce603dfbd452ce3/idna-3.10-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/cffi/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata
DEBUG No netrc file found
DEBUG Searching for a compatible version of some-package-from-private-index (>=0.1.0)
DEBUG No compatible version found for: some-package-from-private-index
DEBUG Recording unit propagation conflict of some-package-from-private-index from incompatibility of (uv-dep-repro[some-extra])
DEBUG Searching for a compatible version of uv-dep-repro @ file:///home/tamas/git/uv-dep-repro (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: uv-dep-repro[some-extra]
  × No solution found when resolving dependencies:
  ╰─▶ Because some-package-from-private-index was not found in the package registry and uv-dep-repro[some-extra] depends on some-package-from-private-index>=0.1.0, we can conclude
      that uv-dep-repro[some-extra]'s requirements are unsatisfiable.
      And because your project requires uv-dep-repro[some-extra], we can conclude that your project's requirements are unsatisfiable.
DEBUG Released lock at `/home/tamas/git/uv-dep-repro/.venv/.lock`
```

My expectation would be that `uv` should recognize that we were not asking for `some_extra` so it should not even attempt to query the private repository - or even if it does, it should recognize that none of the dependencies are to be fetched from this repo and it should not fail the `uv sync` command.

uv version: 0.8.10
Python version: 3.13
Operating system: Linux

OS and Python version probably does not matter as we have similar reports from people using other OS or Python versions.

### Platform

Linux 6.8.0-71-generic x86_64 GNU/Linux

### Version

uv 0.8.10 (7e817bfad 2025-08-13)

### Python version

Python 3.13.5

---

_Label `bug` added by @ntamas on 2025-08-15 14:22_

---

_Comment by @charliermarsh on 2025-08-15 15:25_

I think this is expected. `uv sync` creates a universal lockfile, which requires resolving all dependencies, even they won't be installed by the "current command". If you need to install in an environment that won't have full access, we'd typically recommend resolving ahead-of-time, checking in the lockfile, then running `uv sync`.

---

_Comment by @DWhettam on 2025-08-15 15:30_

I've just encountered this as well, and I'm wondering if is there a good solution in the case where you have access to the private index without either:
1 - providing username+pass everytime you run uv sync
2 - storing secrets in plain text as env variables

Is there a solution with e.g. keyring that can be used? I've seen some mention of keyring in the docs but it looks like it only works with specific indexes (please correct me if I'm wrong on that). In my case I'm using gitlab package repository as my private index, and couldn't get keyring to work



---

_Comment by @zanieb on 2025-08-15 15:47_

You can use keyring with arbitrary indexes, there just needs to be a username attached to the index URL or you need to use `authenticate = always`. See https://docs.astral.sh/uv/concepts/indexes/#using-credential-providers

---

_Comment by @DWhettam on 2025-08-15 16:04_

oh great, thanks! I'm trying that now - is there a way to associate the specific keyring secret with the index? Do I need to name it the same thing as the index? I get a 401 error currently. I've tried various combinations of:
`uv run keyring set uv gitlab`
with different naming options.

I have in my pyproject.toml
```
[[tool.uv.index]]
name = "gitlab"
url = "https://gitlab.com/api/v4/groups/<group_id>/-/packages/pypi/simple/"
explicit = false
authenticate = "always"

```

`uv add my_package` fails with this setup, but works with an env variable set. 

---

_Comment by @zanieb on 2025-08-15 16:10_

Would you mind opening a new issue? This is a separate topic than the current issue is focused on.

---

_Comment by @ntamas on 2025-08-15 17:45_

> I think this is expected. `uv sync` creates a universal lockfile, which requires resolving all dependencies, even they won't be installed by the "current command". If you need to install in an environment that won't have full access, we'd typically recommend resolving ahead-of-time, checking in the lockfile, then running `uv sync`.

This makes sense, and also this is what I see in my simplified example (i.e. committing the lockfile on a machine with credentials and then checking out the project on another machine without credentials seems to work). Something else must be at play because I still see failures with our "real" repository that triggered the issue (and the lockfile is up-to-date, from a machine that has full access to all repos). I'll try to identify what the difference is between our real repo and the minimal repro and post an update here.

---

_Comment by @ntamas on 2025-08-15 19:36_

Okay, I think that this issue can be closed. Upon closer investigation, it seems like the solution of @charliermarsh works: the lockfile needs all dependencies (even the optional ones), but once the lockfile is committed, users who check out the repository with the lockfile can now run `uv sync` without issues even if they do not have access to the private repositories. Any changes to `pyproject.toml` that necessitates the regeneration of the lockfile would fail for them, but I guess we can live with it for the time being.

Thanks for the speedy responses!

---

_Closed by @ntamas on 2025-08-15 19:36_

---

_Referenced in [astral-sh/uv#15344](../../astral-sh/uv/issues/15344.md) on 2025-08-18 08:47_

---
