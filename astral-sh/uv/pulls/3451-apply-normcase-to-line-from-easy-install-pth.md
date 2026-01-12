```yaml
number: 3451
title: Apply normcase to line from easy-install.pth
type: pull_request
state: merged
author: hauntsaninja
labels: []
assignees: []
merged: true
base: main
head: norm-case
created_at: 2024-05-08T06:21:23Z
updated_at: 2024-05-08T09:40:40Z
url: https://github.com/astral-sh/uv/pull/3451
synced_at: 2026-01-12T16:05:39Z
```

# Apply normcase to line from easy-install.pth

---

_@hauntsaninja_

Thanks for the suggestion from https://github.com/astral-sh/uv/pull/3415#discussion_r1591772942

Also it looks like you improved `egg-link` parsing in https://github.com/astral-sh/uv/pull/3415/commits/e23c91f52e7df1a5230fc3cb7ca6d3d4ca0b24a2 so copying the changes over to the other parse site (happy to move this to a helper too, if so lmk where to put it)

---

_@hauntsaninja reviewed on 2024-05-08 06:22_

---

_Review comment by @hauntsaninja on `crates/install-wheel-rs/src/uninstall.rs`:239 on 2024-05-08 06:22_

Just going off of the non winapi version of https://github.com/python/cpython/blob/a7711a2a4e5cf16b34fc284085da724a8c2c06dd/Lib/ntpath.py#L78

---

_Converted to draft by @hauntsaninja on 2024-05-08 06:26_

---

_Comment by @hauntsaninja on 2024-05-08 06:27_

(draft to make sure tests fail on windows before I fix things)

---

_Marked ready for review by @hauntsaninja on 2024-05-08 06:38_

---

_@konstin approved on 2024-05-08 08:40_

Thanks

---

_Merged by @konstin on 2024-05-08 08:40_

---

_Closed by @konstin on 2024-05-08 08:40_

---

_Branch deleted on 2024-05-08 09:40_

---
