```yaml
number: 3444
title: "Use manylinux: auto for aarch64 builds"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/glibc
created_at: 2024-05-07T23:58:18Z
updated_at: 2024-05-09T03:59:53Z
url: https://github.com/astral-sh/uv/pull/3444
synced_at: 2026-01-12T16:05:39Z
```

# Use manylinux: auto for aarch64 builds

---

_@charliermarsh_

See: https://github.com/astral-sh/uv/issues/3439

---

_Comment by @charliermarsh on 2024-05-07 23:58_

Not sure this will work. Let's try.

---

_@messense reviewed on 2024-05-09 03:25_

---

_Review comment by @messense on `.github/workflows/build-binaries.yml`:305 on 2024-05-09 03:25_

Try this workaround:

```yaml
- uses: PyO3/maturin-aciton@v1
  env:
    # Workaround ring 0.17 build issue
    CFLAGS_aarch64_unknown_linux_gnu: "-D__ARM_ARCH=8"
  with:
    manylinux: auto
```

---

_@charliermarsh reviewed on 2024-05-09 03:56_

---

_Review comment by @charliermarsh on `.github/workflows/build-binaries.yml`:305 on 2024-05-09 03:56_

Wow I think that worked? Thanks! Are there any limitations here? Will the wheel work as expected?

---

_Merged by @charliermarsh on 2024-05-09 03:58_

---

_Closed by @charliermarsh on 2024-05-09 03:58_

---

_Branch deleted on 2024-05-09 03:58_

---

_@messense reviewed on 2024-05-09 03:59_

---

_Review comment by @messense on `.github/workflows/build-binaries.yml`:305 on 2024-05-09 03:59_

It should work as expected, I think the obvious limitation is that it might break in the future if ring/boringssl actually requires a newer GCC version.

---
