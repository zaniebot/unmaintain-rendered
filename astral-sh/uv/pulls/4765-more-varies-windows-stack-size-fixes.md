```yaml
number: 4765
title: More varies windows stack size fixes
type: pull_request
state: closed
author: konstin
labels:
  - internal
  - windows
assignees: []
draft: true
base: ibraheem/uv-tree
head: konsti/fix-windows-stack-size-again
created_at: 2024-07-03T12:14:31Z
updated_at: 2024-07-03T18:52:43Z
url: https://github.com/astral-sh/uv/pull/4765
synced_at: 2026-01-10T13:48:28Z
```

# More varies windows stack size fixes

---

_Pull request opened by @konstin on 2024-07-03 12:14_

Fix windows stack sizes in debug mode for https://github.com/astral-sh/uv/pull/4708

In this case, box clap arguments so that clap parsing can be done in the default 1MB stack on windows.

Draft so i get the windows CI failure.

---

_Label `internal` added by @konstin on 2024-07-03 12:14_

---

_Label `windows` added by @konstin on 2024-07-03 12:14_

---

_Closed by @konstin on 2024-07-03 18:52_

---

_Branch deleted on 2024-07-03 18:52_

---
