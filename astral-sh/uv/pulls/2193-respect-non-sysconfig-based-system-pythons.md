```yaml
number: 2193
title: "Respect non-`sysconfig`-based system Pythons"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/sysconfig-paths
created_at: 2024-03-05T02:04:06Z
updated_at: 2024-03-05T21:23:36Z
url: https://github.com/astral-sh/uv/pull/2193
synced_at: 2026-01-12T16:04:54Z
```

# Respect non-`sysconfig`-based system Pythons

---

_@charliermarsh_

## Summary

`pip` uses `sysconfig` for Python 3.10 and later by default; however, it falls back to `distutils` for earlier Python versions, and distros can actually tell `pip` to continue falling back to `distutils` via the `_PIP_USE_SYSCONFIG` variable.

By _always_ using `sysconfig`, we're doing the wrong then when installing into some system Pythons, e.g., on Debian prior to Python 3.10.

This PR modifies our logic to mirror `pip` exactly, which is what's been recommended to me as the right thing to do.

Closes https://github.com/astral-sh/uv/issues/2113.

## Test Plan

Most notably, the new Debian tests pass here (which fail on main: https://github.com/astral-sh/uv/pull/2144).

I also added Pyston as a second stress-test.

---

_Renamed from "Charlie/sysconfig paths" to "Update `get_interpreter_info.py` to respect non-sysconfig-based system Pythons" by @charliermarsh on 2024-03-05 02:04_

---

_Marked ready for review by @charliermarsh on 2024-03-05 02:08_

---

_Renamed from "Update `get_interpreter_info.py` to respect non-sysconfig-based system Pythons" to "Respect non-`sysconfig`-based system Pythons" by @charliermarsh on 2024-03-05 02:08_

---

_Label `bug` added by @charliermarsh on 2024-03-05 02:08_

---

_Label `compatibility` added by @charliermarsh on 2024-03-05 02:08_

---

_Review requested from @konstin by @charliermarsh on 2024-03-05 02:22_

---

_@konstin approved on 2024-03-05 09:51_

---

_Merged by @charliermarsh on 2024-03-05 21:23_

---

_Closed by @charliermarsh on 2024-03-05 21:23_

---

_Branch deleted on 2024-03-05 21:23_

---
