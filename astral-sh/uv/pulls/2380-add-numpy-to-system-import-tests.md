```yaml
number: 2380
title: Add numpy to system import tests
type: pull_request
state: merged
author: konstin
labels:
  - testing
assignees: []
merged: true
base: main
head: konsti/add-numpy-test
created_at: 2024-03-12T09:28:36Z
updated_at: 2024-03-18T13:09:33Z
url: https://github.com/astral-sh/uv/pull/2380
synced_at: 2026-01-10T14:49:08Z
```

# Add numpy to system import tests

---

_Pull request opened by @konstin on 2024-03-12 09:28_

Installing and importing numpy tests for two cases:

* The python architecture and the package architecture don't match (https://github.com/astral-sh/uv/issues/2326)
* The libc of python and that of the package don't match on linux (musllinux vs manylinux, picking a compatible manylinux version)

All pylint deps are py3-none-any, so they don't catch those cases.


---

_Label `testing` added by @konstin on 2024-03-12 09:28_

---

_Comment by @charliermarsh on 2024-03-12 14:17_

Not sure what the Pyenv failure is, but the Windows-Python 3.13 failure looks like we need a larger stack size (these are debug builds).

---

_Comment by @konstin on 2024-03-12 17:03_

Added stack size overrides to the windows tests

---

_Comment by @konstin on 2024-03-12 17:53_

The two CI failures look like bugs in uv.

---

_@zanieb reviewed on 2024-03-12 20:03_

---

_Review comment by @zanieb on `scripts/check_system_python.py`:144 on 2024-03-12 20:03_

This is missing the `--break-system-packages` flag which can be included with 

```suggestion
            [uv, "pip", "install", "numpy", "--system"] + allow_externally_managed,
```

This is causing your failure on alpine.

---

_@charliermarsh reviewed on 2024-03-12 20:14_

---

_Review comment by @charliermarsh on `scripts/check_system_python.py`:5 on 2024-03-12 20:14_

The """ should be on the next line.

---

_Comment by @charliermarsh on 2024-03-13 18:26_

@konstin - For the Windows failure, can you check if pip is able to install NumPy there? Is it clear that it's a uv issue, and not a system environment issue?

---

_@charliermarsh approved on 2024-03-18 13:09_

---

_Merged by @charliermarsh on 2024-03-18 13:09_

---

_Closed by @charliermarsh on 2024-03-18 13:09_

---

_Branch deleted on 2024-03-18 13:09_

---
