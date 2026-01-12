```yaml
number: 407
title: Injecting a python version when building source distributions
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-11-13T09:49:10Z
updated_at: 2023-12-04T10:27:38Z
url: https://github.com/astral-sh/uv/issues/407
synced_at: 2026-01-12T15:58:23Z
```

# Injecting a python version when building source distributions

---

_@konstin_

When resolving packages, we allow setting a python version that is not the one in the current venv when resolving, e.g. `target/debug/puffin pip-compile --python-version py39`. We will select everything as if we were on that python version. This however doesn't really work anymore when building source distribution because a PEP 517 (and setup.py too) need a `sys.executable` that will necessarily be the local version.

Possible options:
 * Build the source distribution with the real python version, pick the wheel with the local python version tag (even if it mismatches the faked environment), assume that wheels built for different python versions have the same metadata (this is a somewhat necessary assumption anyway, without it a sane reusable resolution becomes all but impossible). We would keep two environments around, the main fake one and the secondary real one.
 * Provision the fake python from python-build-standalone for builds, making it a real python.
 * Ban source distribution builds when `--python-version` is set.

---

_Comment by @charliermarsh on 2023-11-13 14:34_

I think I want Option (1).

---

_Label `bug` added by @charliermarsh on 2023-11-13 14:34_

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-11-13 14:34_

---

_Comment by @charliermarsh on 2023-11-13 14:34_

But I can't tell if what's implemented in #398 is Option (1).

---

_Comment by @konstin on 2023-11-13 15:34_

I think it's broken on both main and #398 atm, this is a separate task to implement

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-29 15:11_

---

_Closed by @konstin on 2023-12-04 10:27_

---
