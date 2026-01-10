```yaml
number: 2518
title: Fix wheel builds and uploads for musl ARM
type: pull_request
state: merged
author: charliermarsh
labels:
  - releases
assignees: []
merged: true
base: main
head: charlie/arm
created_at: 2024-03-18T18:26:48Z
updated_at: 2024-03-18T18:47:21Z
url: https://github.com/astral-sh/uv/pull/2518
synced_at: 2026-01-10T14:49:08Z
```

# Fix wheel builds and uploads for musl ARM

---

_Pull request opened by @charliermarsh on 2024-03-18 18:26_

If you look at https://pypi.org/project/uv/0.1.22/#files...

- We didn't upload the ARMv6 wheel (I thought I had removed the `# Skip for `arm`, which is not supported by PyPI.`), it must've gotten re-added in a rebase or something.
- We lost the musllinux builds for ARM. I think this is because I built them as manylinux.

---

_Review requested from @konstin by @charliermarsh on 2024-03-18 18:26_

---

_Label `release` added by @charliermarsh on 2024-03-18 18:26_

---

_@zanieb approved on 2024-03-18 18:35_

---

_Merged by @charliermarsh on 2024-03-18 18:47_

---

_Closed by @charliermarsh on 2024-03-18 18:47_

---

_Branch deleted on 2024-03-18 18:47_

---
