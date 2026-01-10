```yaml
number: 16687
title: Skip python_install_emulated_macos except on ARM64 macos with rosetta
type: pull_request
state: merged
author: zsol
labels:
  - internal
assignees: []
merged: true
base: main
head: zsol/jj-rqszyytlrnrl
created_at: 2025-11-11T17:55:27Z
updated_at: 2025-11-12T13:48:18Z
url: https://github.com/astral-sh/uv/pull/16687
synced_at: 2026-01-10T06:28:12Z
```

# Skip python_install_emulated_macos except on ARM64 macos with rosetta

---

_Pull request opened by @zsol on 2025-11-11 17:55_

## Summary

This test isn't useful on non-arm64 macs, and it outright fails if rosetta isn't installed.

## Test Plan

Run it on my rosetta-stripped macbook


---

_Review requested from @konstin by @zsol on 2025-11-11 17:55_

---

_Review requested from @zanieb by @zsol on 2025-11-11 17:55_

---

_Marked ready for review by @zsol on 2025-11-11 18:48_

---

_Review request for @konstin removed by @konstin on 2025-11-11 19:12_

---

_Label `internal` added by @konstin on 2025-11-11 19:13_

---

_Comment by @konstin on 2025-11-11 19:13_

Can't confirm as non-mac user, but LGTM otherwise.

---

_Merged by @zanieb on 2025-11-12 13:48_

---

_Closed by @zanieb on 2025-11-12 13:48_

---

_Branch deleted on 2025-11-12 13:48_

---
