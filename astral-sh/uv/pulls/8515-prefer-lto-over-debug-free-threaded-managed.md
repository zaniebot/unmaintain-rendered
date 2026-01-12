```yaml
number: 8515
title: "Prefer `lto` over `debug` free-threaded managed Python builds"
type: pull_request
state: merged
author: j178
labels:
  - bug
assignees: []
merged: true
base: main
head: lto
created_at: 2024-10-24T06:42:15Z
updated_at: 2024-10-24T12:51:21Z
url: https://github.com/astral-sh/uv/pull/8515
synced_at: 2026-01-12T16:08:21Z
```

# Prefer `lto` over `debug` free-threaded managed Python builds

---

_@j178_

Fix this typo:
https://github.com/astral-sh/uv/blob/98523e2014e9a5c69706623344026d76296e178f/crates/uv-python/fetch-download-metadata.py#L399

and removed an empty line in the `downloads.inc.mustache" template.

---

_@zanieb approved on 2024-10-24 12:33_

Ah thank you

---

_Label `bug` added by @zanieb on 2024-10-24 12:33_

---

_Merged by @zanieb on 2024-10-24 12:33_

---

_Closed by @zanieb on 2024-10-24 12:33_

---

_Renamed from "Fix a typo in `fetch-download-metadata.py`" to "Prefer `lto` over `debug` free-threaded managed Python builds" by @zanieb on 2024-10-24 12:33_

---

_Branch deleted on 2024-10-24 12:51_

---
