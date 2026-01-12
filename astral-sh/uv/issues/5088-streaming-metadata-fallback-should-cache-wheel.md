```yaml
number: 5088
title: Streaming metadata fallback should cache wheel
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2024-07-16T01:32:57Z
updated_at: 2024-07-16T13:21:48Z
url: https://github.com/astral-sh/uv/issues/5088
synced_at: 2026-01-12T15:58:53Z
```

# Streaming metadata fallback should cache wheel

---

_@charliermarsh_

See: `wheel_metadata_no_pep658`. If we can't do a range request, we stream the wheel searching for the `METADATA` file. Because we stream, we stop as soon as we read the `METADATA` file... but that also means we don't cache the wheel, which seems like the wrong tradeoff (for large wheels).

---

_Label `enhancement` added by @charliermarsh on 2024-07-16 01:33_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-16 02:24_

---

_Closed by @charliermarsh on 2024-07-16 13:21_

---

_Closed by @charliermarsh on 2024-07-16 13:21_

---
