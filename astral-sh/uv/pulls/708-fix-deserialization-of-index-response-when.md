```yaml
number: 708
title: "Fix deserialization of index response when `requires_python` field is missing"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/fix-req-missing
created_at: 2023-12-19T19:07:14Z
updated_at: 2023-12-20T10:53:49Z
url: https://github.com/astral-sh/uv/pull/708
synced_at: 2026-01-12T16:04:08Z
```

# Fix deserialization of index response when `requires_python` field is missing

---

_@zanieb_

Closes https://github.com/astral-sh/puffin/issues/707

---

_@charliermarsh approved on 2023-12-20 02:42_

---

_@konstin approved on 2023-12-20 10:53_

Good catch i didn't realize `deserialize_with` breaks the none default

---

_Merged by @konstin on 2023-12-20 10:53_

---

_Closed by @konstin on 2023-12-20 10:53_

---

_Branch deleted on 2023-12-20 10:53_

---
