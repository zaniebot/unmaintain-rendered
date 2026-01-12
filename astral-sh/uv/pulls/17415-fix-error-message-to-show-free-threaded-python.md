```yaml
number: 17415
title: Fix error message to show free-threaded Python indicator
type: pull_request
state: open
author: nooscraft
labels: []
assignees: []
base: main
head: bugfix/17406
created_at: 2026-01-12T12:10:16Z
updated_at: 2026-01-12T12:11:29Z
url: https://github.com/astral-sh/uv/pull/17415
synced_at: 2026-01-12T13:00:12Z
```

# Fix error message to show free-threaded Python indicator

---

_Pull request opened by @nooscraft on 2026-01-12 12:10_

Include 't' suffix in wheel compatibility error messages when using free-threaded Python.
Add explanatory note about GIL incompatibility.
Fix detection to work regardless of version matching.

Fixes the issue 17406

---
