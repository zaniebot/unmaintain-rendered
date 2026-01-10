```yaml
number: 7867
title: "Allow `py3x-none` tags in newer than Python 3.x"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/cp3x
created_at: 2024-10-02T10:47:59Z
updated_at: 2024-10-03T17:02:16Z
url: https://github.com/astral-sh/uv/pull/7867
synced_at: 2026-01-10T12:53:58Z
```

# Allow `py3x-none` tags in newer than Python 3.x

---

_Pull request opened by @konstin on 2024-10-02 10:47_

Unlike `cp36-...`, which requires exactly CPython 3.6, `py36-none` is compatible with all versions starting at Python 3.6.

Note that `py3x-none` should not be used. Instead, use `py3-none` with `requires-python`. 

Fixes #7800

---

_Label `bug` added by @konstin on 2024-10-02 10:47_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:362 on 2024-10-02 11:17_

Does this entire part make sense? In the example above, I thought cp36 was a lower-bound, so it _would_ match`==3.10.*`.

---

_@charliermarsh reviewed on 2024-10-02 11:17_

---

_@konstin reviewed on 2024-10-02 14:12_

---

_Review comment by @konstin on `crates/uv-resolver/src/requires_python.rs`:362 on 2024-10-02 14:12_

`cpxx-<...>-<...>` as a python tag only really makes sense in combination with a matching ABI tag, `cp311-none` doesn't really make sense: why are you requiring CPython if you're not requiring its ABI? For pragmatic reasons, we're following pip (on python 3.12), `cp` is an exact python tag unless we use `abi3`, then it's a minimum version, while `py` is a lower bound:

```
$ pip debug --verbose | grep manylinux_2_39_x86_64
WARNING: This command is only meant for debugging. Do not use this with automation for parsing and getting these details, since the output and options of this command may change without notice.
  cp312-cp312-manylinux_2_39_x86_64
  cp312-abi3-manylinux_2_39_x86_64
  cp312-none-manylinux_2_39_x86_64
  cp311-abi3-manylinux_2_39_x86_64
  cp310-abi3-manylinux_2_39_x86_64
  cp39-abi3-manylinux_2_39_x86_64
  cp38-abi3-manylinux_2_39_x86_64
  cp37-abi3-manylinux_2_39_x86_64
  cp36-abi3-manylinux_2_39_x86_64
  cp35-abi3-manylinux_2_39_x86_64
  cp34-abi3-manylinux_2_39_x86_64
  cp33-abi3-manylinux_2_39_x86_64
  cp32-abi3-manylinux_2_39_x86_64
  py312-none-manylinux_2_39_x86_64
  py3-none-manylinux_2_39_x86_64
  py311-none-manylinux_2_39_x86_64
  py310-none-manylinux_2_39_x86_64
  py39-none-manylinux_2_39_x86_64
  py38-none-manylinux_2_39_x86_64
  py37-none-manylinux_2_39_x86_64
  py36-none-manylinux_2_39_x86_64
  py35-none-manylinux_2_39_x86_64
  py34-none-manylinux_2_39_x86_64
  py33-none-manylinux_2_39_x86_64
  py32-none-manylinux_2_39_x86_64
  py31-none-manylinux_2_39_x86_64
  py30-none-manylinux_2_39_x86_64
```

---

_@charliermarsh reviewed on 2024-10-02 23:00_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:367 on 2024-10-02 23:00_

What if the version is greater than our own upper-bound on Python? E.g., `py310` but the user has `== 3.8.*`.

---

_@charliermarsh reviewed on 2024-10-03 17:01_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:367 on 2024-10-03 17:01_

I guess we don't handle this at all here yet (upper bounds).

---

_Merged by @charliermarsh on 2024-10-03 17:02_

---

_Closed by @charliermarsh on 2024-10-03 17:02_

---

_Branch deleted on 2024-10-03 17:02_

---
