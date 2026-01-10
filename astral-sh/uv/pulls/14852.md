```yaml
number: 14852
title: Publish riscv64 wheels to PyPI
type: pull_request
state: merged
author: zanieb
labels:
  - do-not-merge
assignees: []
merged: true
base: main
head: zb/riscv
created_at: 2025-07-23T18:13:10Z
updated_at: 2025-07-23T21:52:38Z
url: https://github.com/astral-sh/uv/pull/14852
synced_at: 2026-01-10T06:53:02Z
```

# Publish riscv64 wheels to PyPI

---

_Pull request opened by @zanieb on 2025-07-23 18:13_

This reverts commit 49b450109b825ec8fdec7600c2a8f763496f70b7 from https://github.com/astral-sh/uv/pull/14009 following https://github.com/pypi/warehouse/pull/18390


---

_Comment by @zanieb on 2025-07-23 20:11_

Are our wheels tagged `manylinux_2_39_riscv64`?

---

_Comment by @zanieb on 2025-07-23 20:12_

https://github.com/astral-sh/uv/pull/14006 suggests we're using `manylinux_2_31`

---

_Comment by @zanieb on 2025-07-23 20:12_

cc @Xeonacid

---

_Renamed from "Revert "filter out riscv64 wheels before publishing to pypi (#14009)"" to "Publish riscv64 wheels to PyPI" by @zanieb on 2025-07-23 20:13_

---

_@charliermarsh approved on 2025-07-23 21:21_

---

_Label `do-not-merge` added by @zanieb on 2025-07-23 21:26_

---

_Comment by @zanieb on 2025-07-23 21:26_

I'm not sure how to change our target version off the top of my head, cc @konstin 

---

_Comment by @konstin on 2025-07-23 21:33_

maturin should tag them based on the underlying build host image, to switch the glibc version, the main toggle is to switch to a different image.

Where are you seeing the wheel tag?

---

_Comment by @zanieb on 2025-07-23 21:35_

You can see it in https://github.com/astral-sh/uv/actions/runs/16454309827/job/46507380876

---

_Comment by @zanieb on 2025-07-23 21:37_

We use `manylinux: auto`; see https://github.com/PyO3/maturin-action/issues/365

---

_Comment by @zanieb on 2025-07-23 21:42_

Sorry I totally misread the patch over in warehouse, that's not a limitation.

---

_Merged by @zanieb on 2025-07-23 21:52_

---

_Closed by @zanieb on 2025-07-23 21:52_

---

_Branch deleted on 2025-07-23 21:52_

---
