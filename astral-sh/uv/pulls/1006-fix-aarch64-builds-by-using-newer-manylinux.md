```yaml
number: 1006
title: Fix aarch64 builds by using newer manylinux version
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
base: main
head: konsti/manylinux-2_28
created_at: 2024-01-19T10:21:06Z
updated_at: 2024-01-22T12:18:18Z
url: https://github.com/astral-sh/uv/pull/1006
synced_at: 2026-01-10T15:39:03Z
```

# Fix aarch64 builds by using newer manylinux version

---

_Pull request opened by @konstin on 2024-01-19 10:21_

Fixes #992

---

_Renamed from "manylinux-2_28" to "Fix aarch64 builds by using newer manylinux version" by @konstin on 2024-01-19 10:33_

---

_Review requested from @charliermarsh by @konstin on 2024-01-19 10:33_

---

_Marked ready for review by @konstin on 2024-01-19 10:33_

---

_Comment by @charliermarsh on 2024-01-19 13:53_

I think I tried this? Hopefully I messed it up :) Let's give it a shot, see if CI succeeds here.

---

_Comment by @charliermarsh on 2024-01-19 13:56_

@konstin - I think there's a new failure here for `libz-ng`, but I can look into that.

---

_Comment by @charliermarsh on 2024-01-19 20:03_

Replaced by https://github.com/astral-sh/puffin/pull/1012.

---

_Closed by @charliermarsh on 2024-01-19 20:03_

---

_Branch deleted on 2024-01-22 12:18_

---
