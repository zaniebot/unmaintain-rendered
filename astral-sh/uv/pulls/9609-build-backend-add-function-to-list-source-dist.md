```yaml
number: 9609
title: "Build backend: Add function to list source dist files only"
type: pull_request
state: closed
author: konstin
labels:
  - preview
assignees: []
base: main
head: konsti/build-backend-list
created_at: 2024-12-03T15:22:40Z
updated_at: 2024-12-03T15:31:38Z
url: https://github.com/astral-sh/uv/pull/9609
synced_at: 2026-01-10T12:00:00Z
```

# Build backend: Add function to list source dist files only

---

_Pull request opened by @konstin on 2024-12-03 15:22_

Add the backend implementation for `uv build --list` without wiring it up to the CLI. We're using the directory writer to capture which file would be added to the archive, without actually adding them. This does not yet add the CLI part.

---

_Label `preview` added by @konstin on 2024-12-03 15:22_

---

_Review requested from @BurntSushi by @konstin on 2024-12-03 15:22_

---

_Comment by @BurntSushi on 2024-12-03 15:30_

I'm having deja vu with #9602 on this one.

---

_Comment by @konstin on 2024-12-03 15:31_

My stack got out of sync :/

---

_Closed by @konstin on 2024-12-03 15:31_

---
