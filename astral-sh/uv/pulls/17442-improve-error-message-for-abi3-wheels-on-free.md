```yaml
number: 17442
title: Improve error message for abi3 wheels on free-threaded Python
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: claude/abi-wheel-compatibility-35SSt
created_at: 2026-01-13T18:01:13Z
updated_at: 2026-01-13T21:19:32Z
url: https://github.com/astral-sh/uv/pull/17442
synced_at: 2026-01-13T21:36:42Z
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
