```yaml
number: 17442
title: Improve error message for abi3 wheels on free-threaded Python
type: pull_request
state: open
author: zanieb
labels:
  - bug
  - error messages
assignees: []
base: main
head: claude/abi-wheel-compatibility-35SSt
created_at: 2026-01-13T18:01:13Z
updated_at: 2026-01-14T19:15:01Z
url: https://github.com/astral-sh/uv/pull/17442
synced_at: 2026-01-14T19:46:16Z
```

# Improve error message for abi3 wheels on free-threaded Python

---

_@zanieb_

Closes #17406 

Unlike https://github.com/astral-sh/uv/pull/17415, this returns a dedicated error variant instead of adding a downstream special case to handling of `Python` tag incompatibilities.


---

_@zanieb reviewed on 2026-01-13 18:01_

---

_Review comment by @zanieb on `crates/uv-platform-tags/src/tags.rs`:314 on 2026-01-13 18:01_

This seems sort of questionable.

---

_@zanieb reviewed on 2026-01-13 23:22_

---

_Review comment by @zanieb on `crates/uv-installer/src/satisfies.rs`:478 on 2026-01-13 23:22_

I think this should probably be "which is incompatible"?

---

_@zanieb reviewed on 2026-01-13 23:23_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/prioritized_distribution.rs`:230 on 2026-01-13 23:23_

I think we should probably omit this latter part? It's included in the subsequent message.

Maybe "a free-threaded compatible ABI tag"?

---

_Label `bug` added by @zanieb on 2026-01-13 23:28_

---

_Label `error messages` added by @zanieb on 2026-01-13 23:28_

---

_Marked ready for review by @zanieb on 2026-01-14 14:54_

---

_Converted to draft by @zanieb on 2026-01-14 14:56_

---

_Marked ready for review by @zanieb on 2026-01-14 19:15_

---
