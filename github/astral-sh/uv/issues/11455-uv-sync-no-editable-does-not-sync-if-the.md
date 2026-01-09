---
number: 11455
title: "uv sync --no-editable does not sync if the `pyproject.toml` did not chage"
type: issue
state: open
author: gaborbernat
labels:
  - enhancement
assignees: []
created_at: 2025-02-12T17:08:29Z
updated_at: 2025-07-03T23:11:08Z
url: https://github.com/astral-sh/uv/issues/11455
synced_at: 2026-01-07T13:12:18-06:00
---

# uv sync --no-editable does not sync if the `pyproject.toml` did not chage

---

_Issue opened by @gaborbernat on 2025-02-12 17:08_

### Summary

As a user my expectation is that uv sync will sync me dependencies. 🤔 And while caching immutable (mostly) published packages i makes sense, caching a local package is debatable. 🤔 Do I need to set `**/*.py` as a cache key for uv to rebuild my package? 🤔 So I can test the latest version of the package and not an old outdated variant.

### Example

If so, at leaast would be nice to be able to force reinstall without needing to know the package name 😄 Something like: `--reinstall-project` 😂 today one can do `--reinstall-package {package-name}` but first you need to know (and type out) the package name. 


---

_Label `enhancement` added by @gaborbernat on 2025-02-12 17:08_

---

_Referenced in [tox-dev/tox-uv#176](../../tox-dev/tox-uv/pulls/176.md) on 2025-02-13 00:06_

---

_Comment by @konstin on 2025-02-13 13:53_

Can you use an editable install of the package? This should avoid the need to reinstall local packages altogether.

---

_Comment by @gaborbernat on 2025-02-13 14:33_

Editable installs can differ from the final wheel in many ways. You want to test what your end users use. E.g. using editable installs will not test missing package data files. 

---

_Comment by @clssn on 2025-02-27 18:38_

I ran into the same problem, also requiring `--no-editable`. I'd like to avoid configuration of cache-keys for all the projects in our monorepo (doesn't scale since it is easily forgotten when adding a new project - would require a dedicated pre-commit hook etc.).

We're now working around the issue with using `--reinstall` but that's needlessly causing overhead by needlessly reinstalling the regular packages as well.

If changing the default caching behavior for editables in combination with `--no-editable` isn't acceptable, what about an `--reinstall-editables` option? 

---

_Comment by @zhuoqun-chen on 2025-07-03 23:09_

@konstin I think the proposed `--reinstall-editables` flag by @clssn is also reasonable especially in a `monorepo` with many workspace members, and where the `.venv` is located at the monorepo root folder. In my use case `uv sync --all-packages` will create `__editable__**.py` for local library dependencies (using `setuptools` backend). And this makes `vscode intellisense` can't have correct package import hints as described in https://github.com/astral-sh/uv/issues/3898. Using `uv sync --all-packages --no-editable` will make vscode happy, but even if the source code of the workspace member changes, doing it again will not change anything inside `.venv` at all.

---

_Referenced in [astral-sh/uv#15380](../../astral-sh/uv/issues/15380.md) on 2025-08-21 09:43_

---
