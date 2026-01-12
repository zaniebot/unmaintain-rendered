```yaml
number: 16357
title: fix small typos
type: pull_request
state: merged
author: s-rigaud
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-typos
created_at: 2025-10-18T22:07:53Z
updated_at: 2025-10-21T12:12:00Z
url: https://github.com/astral-sh/uv/pull/16357
synced_at: 2026-01-12T16:12:13Z
```

# fix small typos

---

_@s-rigaud_

## Summary

I tried to fix minor typos in the project

---

_@zanieb approved on 2025-10-18 22:32_

Thanks!

Though note the `ecosystem/` files are vendored from those projects for testing.

---

_Label `documentation` added by @zanieb on 2025-10-18 22:33_

---

_Comment by @konstin on 2025-10-20 11:54_

Can you revert the `ecosystem/` changes? To keep the tests realistic and avoid confusion over differences with the real project, we want to keep e.g. `PKG-INFO` exactly as they are upstream.

---

_@konstin approved on 2025-10-21 12:00_

---

_Merged by @konstin on 2025-10-21 12:12_

---

_Closed by @konstin on 2025-10-21 12:12_

---
