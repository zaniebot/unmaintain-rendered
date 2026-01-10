```yaml
number: 1137
title: "bootstrap: add 'libxcrypt-compat' to pacman install command"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/add-arch-bootstrap
created_at: 2024-01-26T20:08:23Z
updated_at: 2024-01-26T21:56:42Z
url: https://github.com/astral-sh/uv/pull/1137
synced_at: 2026-01-10T15:39:03Z
```

# bootstrap: add 'libxcrypt-compat' to pacman install command

---

_Pull request opened by @BurntSushi on 2024-01-26 20:08_

This is apparently necessary to permit Python 3.8.12 to run. Namely, it
needs to link to libcrypt.so.1, and without libxcrypt-compat, that
linking step fails.


---

_@zanieb approved on 2024-01-26 20:08_

Thanks!

---

_Label `internal` added by @zanieb on 2024-01-26 20:08_

---

_Merged by @BurntSushi on 2024-01-26 21:56_

---

_Closed by @BurntSushi on 2024-01-26 21:56_

---

_Branch deleted on 2024-01-26 21:56_

---
