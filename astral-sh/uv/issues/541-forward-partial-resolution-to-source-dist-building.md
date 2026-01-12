```yaml
number: 541
title: Forward partial resolution to source dist building
type: issue
state: closed
author: konstin
labels: []
assignees: []
created_at: 2023-12-04T11:04:14Z
updated_at: 2024-01-16T05:37:16Z
url: https://github.com/astral-sh/uv/issues/541
synced_at: 2026-01-12T15:58:23Z
```

# Forward partial resolution to source dist building

---

_@konstin_

Currently, when we encounter a source dist in the main resolution, we do an entirely new resolution for build requirements of the source dist. This means that sometimes we have slightly different versions in the main resolution (https://github.com/astral-sh/puffin/issues/537). Instead, we should pass the versions of the current partial resolution as preferred versions in the source dist build.

This is half a performance feature (using the same cached versions as in the main resolution) and half a correctness feature, e.g. when the result of a source dist build depends on the installed versions (we can't really handle this correctly, but we handle it slightly less bad that way by increasing our chances on hitting the right version), but also being more what the user expects.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-09 04:09_

---

_Comment by @charliermarsh on 2024-01-09 04:09_

I'll give this a shot.

---

_Closed by @charliermarsh on 2024-01-16 05:37_

---
