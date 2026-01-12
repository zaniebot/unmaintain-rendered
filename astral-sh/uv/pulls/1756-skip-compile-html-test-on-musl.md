```yaml
number: 1756
title: Skip compile_html test on musl
type: pull_request
state: merged
author: konstin
labels:
  - musl
assignees: []
merged: true
base: main
head: konsti/compile-html-no-musl
created_at: 2024-02-20T14:53:58Z
updated_at: 2024-02-20T16:36:04Z
url: https://github.com/astral-sh/uv/pull/1756
synced_at: 2026-01-12T16:04:43Z
```

# Skip compile_html test on musl

---

_@konstin_

The torch index has no musllinux wheel, so we need to skip the test on alpine.


---

_Label `musl` added by @konstin on 2024-02-20 14:53_

---

_@charliermarsh reviewed on 2024-02-20 14:54_

---

_Review comment by @charliermarsh on `.gitignore`:7 on 2024-02-20 14:54_

Why?

---

_@konstin reviewed on 2024-02-20 14:55_

---

_Review comment by @konstin on `.gitignore`:7 on 2024-02-20 14:55_

This helps when setting `CARGO_TARGET_DIR="target-alpine"` when both the host and the docker think they are the default target

---

_@charliermarsh reviewed on 2024-02-20 15:01_

---

_Review comment by @charliermarsh on `.gitignore`:7 on 2024-02-20 15:01_

Feels like it maybe belongs in a personal `.gitignore`. Can we at least change to `target-alpine/`? Otherwise I worry this will cause a confusing behavior in the future when we forget about it.

---

_@charliermarsh approved on 2024-02-20 15:01_

---

_Merged by @konstin on 2024-02-20 16:36_

---

_Closed by @konstin on 2024-02-20 16:36_

---

_Branch deleted on 2024-02-20 16:36_

---
