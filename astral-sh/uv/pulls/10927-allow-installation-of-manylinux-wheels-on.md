```yaml
number: 10927
title: Allow installation of manylinux wheels on loongarch64
type: pull_request
state: merged
author: wojiushixiaobai
labels:
  - enhancement
assignees: []
merged: true
base: main
head: patch_loong64
created_at: 2025-01-24T05:15:35Z
updated_at: 2025-01-24T13:36:04Z
url: https://github.com/astral-sh/uv/pull/10927
synced_at: 2026-01-10T11:45:19Z
```

# Allow installation of manylinux wheels on loongarch64

---

_Pull request opened by @wojiushixiaobai on 2025-01-24 05:15_

# Summary
auditwheel is capable of generating loongarch64 wheels for manylinux_2_38 and above. Here we modify uv-platform-tags so that those wheels can be installed using uv.

- https://github.com/pypa/auditwheel/pull/522

# Test Plan
```yaml
      - name: Setup QEMU
        run: docker run --rm --privileged ghcr.io/loong64/qemu-user-static --reset -p yes

      - name: Build wheels
        uses: loong64/cibuildwheel@main
        env:
          CIBW_MANYLINUX_LOONGARCH64_IMAGE: manylinux_2_38
          CIBW_ARCHS: loongarch64
```

- https://github.com/loong64/pypi
- https://gitlab.com/loong64/pypi/-/packages/

---

_@konstin reviewed on 2025-01-24 13:35_

thanks!

---

_Label `enhancement` added by @konstin on 2025-01-24 13:36_

---

_Merged by @konstin on 2025-01-24 13:36_

---

_Closed by @konstin on 2025-01-24 13:36_

---
