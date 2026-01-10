```yaml
number: 9127
title: Remove end-of-line whitespace in snapshots
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/trim-end-of-line-whitespace
created_at: 2024-11-14T18:05:15Z
updated_at: 2024-11-14T18:16:21Z
url: https://github.com/astral-sh/uv/pull/9127
synced_at: 2026-01-10T12:00:00Z
```

# Remove end-of-line whitespace in snapshots

---

_Pull request opened by @konstin on 2024-11-14 18:05_

I've configured my IDE to remove trailing end-of-line whitespace, and these snapshots were causing trouble.

---

_Label `internal` added by @konstin on 2024-11-14 18:05_

---

_Renamed from "Remove end-of-line whitespace in tests" to "Remove end-of-line whitespace in snapshots" by @konstin on 2024-11-14 18:05_

---

_@BurntSushi approved on 2024-11-14 18:09_

Same happens to me (my editor also trims trailing whitespace). I noticed the `sklearn` one a few days ago and carefully plucked it out of my commit.

---

_Merged by @konstin on 2024-11-14 18:16_

---

_Closed by @konstin on 2024-11-14 18:16_

---

_Branch deleted on 2024-11-14 18:16_

---
