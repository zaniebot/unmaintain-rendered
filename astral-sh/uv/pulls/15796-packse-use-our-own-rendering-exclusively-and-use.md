```yaml
number: 15796
title: "packse: Use our own rendering exclusively, and use pylock.toml"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/pylock-toml-for-packse
created_at: 2025-09-11T18:48:43Z
updated_at: 2025-09-16T13:25:13Z
url: https://github.com/astral-sh/uv/pull/15796
synced_at: 2026-01-12T16:11:56Z
```

# packse: Use our own rendering exclusively, and use pylock.toml

---

_@konstin_

This PR contains two changes: The companion PR to https://github.com/astral-sh/packse/pull/277, which moderately simplifies the uv side, and switching to pylock.toml for packse as dogfooding. These changes can be applied independent from each other.

Since all files, including the vendored build dependencies, are now on GitHub Pages under the same root, we only need a packse index root URL.


---

_Label `internal` added by @konstin on 2025-09-11 18:48_

---

_Marked ready for review by @konstin on 2025-09-16 10:14_

---

_Review requested from @zanieb by @konstin on 2025-09-16 10:14_

---

_@zanieb approved on 2025-09-16 13:23_

---

_Comment by @zanieb on 2025-09-16 13:24_

The #277 link is wrong

---

_Comment by @konstin on 2025-09-16 13:25_

right id, wrong repo - thanks!

---

_Merged by @konstin on 2025-09-16 13:25_

---

_Closed by @konstin on 2025-09-16 13:25_

---

_Branch deleted on 2025-09-16 13:25_

---
