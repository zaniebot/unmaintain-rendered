```yaml
number: 14820
title: Uv build default
type: issue
state: closed
author: HoseynAAmiri
labels:
  - question
assignees: []
created_at: 2025-07-22T16:29:29Z
updated_at: 2025-07-22T19:10:09Z
url: https://github.com/astral-sh/uv/issues/14820
synced_at: 2026-01-12T16:01:57Z
```

# Uv build default

---

_@HoseynAAmiri_

### Question

How can I make new project or library build default to hatchling instead of uv_build? It seems like the new version of uv (v8.1) uses uv_build now instead of hatchling (v7.1).

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @HoseynAAmiri on 2025-07-22 16:29_

---

_Comment by @zanieb on 2025-07-22 16:33_

I'd recommend using `--build-backend hatchling`. There's not a persistent setting,

---

_Closed by @zanieb on 2025-07-22 19:10_

---
