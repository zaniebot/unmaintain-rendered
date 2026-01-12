```yaml
number: 234
title: Fix musl compilation
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: fix-glib-detection
created_at: 2023-10-30T16:15:48Z
updated_at: 2023-10-30T17:10:18Z
url: https://github.com/astral-sh/uv/pull/234
synced_at: 2026-01-12T16:03:49Z
```

# Fix musl compilation

---

_@konstin_

musl (which we already use in ruff) allows statically linked binaries on linux. This PR switches to rustls and vendors and fixes the glibc detection. Using static musl builds makes it easier to avoid glibc errors in docker and we'll need it later for alpine users anyway.

An alternative is using vendored openssl.

---

_Comment by @konstin on 2023-10-30 16:17_

The code itself isn't too interesting, mainly looking for feedback on openssl vs. rustls

---

_@charliermarsh approved on 2023-10-30 16:37_

---

_Merged by @konstin on 2023-10-30 17:10_

---

_Closed by @konstin on 2023-10-30 17:10_

---

_Branch deleted on 2023-10-30 17:10_

---
