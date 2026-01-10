```yaml
number: 10298
title: Rename CI runners and use Windows 2025 preview on large runners
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/runners
created_at: 2025-01-04T22:18:38Z
updated_at: 2025-01-05T20:29:39Z
url: https://github.com/astral-sh/uv/pull/10298
synced_at: 2026-01-10T11:44:41Z
```

# Rename CI runners and use Windows 2025 preview on large runners

---

_Pull request opened by @zanieb on 2025-01-04 22:18_

I'm renaming our runners to be more explicit about their size, architecture, and version.

Switching to Windows 2025 over 2022 in some of our jobs in the hope that it's faster.

---

_Label `internal` added by @zanieb on 2025-01-04 22:18_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:220 on 2025-01-04 22:20_

The macOS larger runners are not configurable like the rest, for some reason.

---

_@zanieb reviewed on 2025-01-04 22:20_

---

_@zanieb reviewed on 2025-01-04 22:31_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:175 on 2025-01-04 22:31_

The new `github-...` labels are roughly structure to match the depot ones.

I preferred to be explicit about x86_64

---

_Marked ready for review by @zanieb on 2025-01-04 23:02_

---

_@zanieb reviewed on 2025-01-05 06:59_

---

_Review comment by @zanieb on `.github/workflows/build-binaries.yml`:159 on 2025-01-05 06:59_

The last number here is the CPU core count

---

_@charliermarsh approved on 2025-01-05 20:06_

---

_Merged by @zanieb on 2025-01-05 20:29_

---

_Closed by @zanieb on 2025-01-05 20:29_

---

_Branch deleted on 2025-01-05 20:29_

---
