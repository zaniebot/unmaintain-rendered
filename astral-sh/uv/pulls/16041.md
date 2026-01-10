```yaml
number: 16041
title: Document transparent x86_64 emulation on aarch64
type: pull_request
state: merged
author: konstin
labels:
  - documentation
assignees: []
merged: true
base: main
head: konsti/Transparent-x86_64-emulation-on-aarch64
created_at: 2025-09-26T13:25:36Z
updated_at: 2025-10-02T16:31:40Z
url: https://github.com/astral-sh/uv/pull/16041
synced_at: 2026-01-10T06:36:15Z
```

# Document transparent x86_64 emulation on aarch64

---

_Pull request opened by @konstin on 2025-09-26 13:25_

_No description provided._

---

_Review requested from @geofft by @konstin on 2025-09-26 13:25_

---

_Review requested from @Gankra by @konstin on 2025-09-26 13:25_

---

_Label `documentation` added by @konstin on 2025-09-26 13:25_

---

_@konstin reviewed on 2025-09-26 13:28_

---

_Review comment by @konstin on `docs/concepts/python-versions.md`:476 on 2025-09-26 13:28_

These is my assumption based on previous issues. There are exceptions (https://learn.microsoft.com/en-us/windows/arm/arm64x-pe), but it seems to be advisable to have the user stay either all on x86_64 or all on aarch64 for their venv.

---

_Review requested from @zsol by @konstin on 2025-09-26 13:28_

---

_@Gankra approved on 2025-10-02 15:05_

(pedantic context which may or may not be relevant here, but I am always compelled to mention)

WoA and Rosetta 2 differ in that aiui WoA is installed-by-default and guaranteed to work, while Rosetta 2 is "only" auto-installed if you launch a *gui* app that needs it (and thereafter will *also* permanently work on non-gui binaries on the system). Apple has also aiui signaled an intent to end support for Rosetta 2.

---

_Merged by @konstin on 2025-10-02 16:31_

---

_Closed by @konstin on 2025-10-02 16:31_

---

_Branch deleted on 2025-10-02 16:31_

---
