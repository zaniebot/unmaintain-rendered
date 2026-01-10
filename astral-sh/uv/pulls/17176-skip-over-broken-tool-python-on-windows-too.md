```yaml
number: 17176
title: Skip over broken tool Python on Windows too
type: pull_request
state: open
author: konstin
labels:
  - windows
assignees: []
draft: true
base: main
head: konsti/windows-broken-tool-retry
created_at: 2025-12-18T14:31:17Z
updated_at: 2025-12-18T14:31:17Z
url: https://github.com/astral-sh/uv/pull/17176
synced_at: 2026-01-10T05:49:14Z
```

# Skip over broken tool Python on Windows too

---

_Pull request opened by @konstin on 2025-12-18 14:31_

Fixes #16252

The fix works on my machine.

TODO:
* Can we do something more reliable than string parsing (at least it works on a German machine, but still). Where is the error returned in the trampoline?
* Integration test, ideally without actually installing and uninstalling Python


---

_Label `windows` added by @konstin on 2025-12-18 14:31_

---
