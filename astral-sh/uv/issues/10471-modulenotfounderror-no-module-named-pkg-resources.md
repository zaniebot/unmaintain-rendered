```yaml
number: 10471
title: "`ModuleNotFoundError: No module named 'pkg_resources'` when adding google-cloud-storage"
type: issue
state: closed
author: marc-at-brightnight
labels:
  - question
assignees: []
created_at: 2025-01-10T14:45:55Z
updated_at: 2025-01-11T13:53:14Z
url: https://github.com/astral-sh/uv/issues/10471
synced_at: 2026-01-12T16:00:14Z
```

# `ModuleNotFoundError: No module named 'pkg_resources'` when adding google-cloud-storage

---

_@marc-at-brightnight_

We are trying to switch from poetry to uv. We are getting some import errors after migrating dependencies, specifically for google-cloud-storage.

`ModuleNotFoundError: No module named 'pkg_resources'`

With poetry, running the command below works. With uv, it does not.
```
❯ ls venv/lib/python3.11/site-packages/ | grep pkg_resources  # poetry
pkg_resources

❯ ls .venv/lib/python3.11/site-packages/ | grep pkg_resources # uv
```

I recreated this issue with a small repo: https://github.com/marc-at-brightnight/uv_test

Would appreciate any help!


---

_Comment by @konstin on 2025-01-10 14:54_

`pkg_resources` is provided by `setuptools`. `setuptools` is part of the "seed" packages up until Python 3.11 (Python 3.12+ does install pip as seed package with `python -m venv`). Poetry installs those by default, while uv doesn't. In uv, you can get the seed packages with `uv venv --seed` (or by installing `uv pip install setuptools`).

fwiw `pkg_resources` is deprecated, so it is recommended packages migrate to the std `importlib` instead (https://setuptools.pypa.io/en/latest/pkg_resources.html). 

---

_Comment by @zanieb on 2025-01-10 14:54_

The Python 3.12 changelog entry

> [gh-95299](https://github.com/python/cpython/issues/95299): Do not pre-install setuptools in virtual environments created with [venv](https://docs.python.org/3/library/venv.html#module-venv). This means that distutils, setuptools, pkg_resources, and easy_install will no longer available by default; to access these run pip install setuptools in the [activated](https://docs.python.org/3/library/venv.html#venv-explanation) virtual environment.

---

_Label `question` added by @zanieb on 2025-01-10 14:54_

---

_Comment by @marc-at-brightnight on 2025-01-10 18:39_

Thanks for the clarification, this is good to know. I found upgrading `google-cloud-storage` to v2 fixed the issue.

---

_Closed by @marc-at-brightnight on 2025-01-10 18:39_

---

_Comment by @edmorley on 2025-01-11 13:52_

> Poetry installs those by default, while uv doesn't

As of Poetry 2.0.0 (released last week), Poetry no longer installs setuptools in the venvs it creates either:
https://github.com/python-poetry/poetry/pull/9331

(So all of the Python 3.12+ stdlib `venv` module, uv and Poetry are now consistent in that regard. If a package needs to use `pkg_resources` it must add an explicit dependency on it, or ideally move away from `pkg_resources`.)

---
