```yaml
number: 3337
title: "Remove `Into::into`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/remove-into-into
created_at: 2024-05-02T09:54:35Z
updated_at: 2024-05-02T10:26:43Z
url: https://github.com/astral-sh/uv/pull/3337
synced_at: 2026-01-10T14:37:54Z
```

# Remove `Into::into`

---

_Pull request opened by @konstin on 2024-05-02 09:54_

Motivated by https://github.com/astral-sh/uv/pull/3263#discussion_r1585896159

I don't think we can lint againt this since we do want to allow `.into()`, just not the bare `Into::into` call.

---

_Label `internal` added by @konstin on 2024-05-02 09:54_

---

_Merged by @konstin on 2024-05-02 10:26_

---

_Closed by @konstin on 2024-05-02 10:26_

---

_Branch deleted on 2024-05-02 10:26_

---
