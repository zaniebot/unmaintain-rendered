```yaml
number: 6678
title: Add semver-compatible docker tag prefixes
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - releases
assignees: []
created_at: 2024-08-27T08:33:00Z
updated_at: 2024-08-28T13:31:54Z
url: https://github.com/astral-sh/uv/issues/6678
synced_at: 2026-01-12T15:59:06Z
```

# Add semver-compatible docker tag prefixes

---

_@konstin_

Currently, we only tag latest and specific versions in [ghcr.io/astral-sh/uv](https://github.com/astral-sh/uv/pkgs/container/uv). We should also tag and update e.g. `0.3`, so you can do `COPY --from=ghcr.io/astral-sh/uv:0.3 /uv /bin/uv` and also get the latest compatible 0.3 release.

---

_Label `enhancement` added by @konstin on 2024-08-27 08:33_

---

_Label `release` added by @konstin on 2024-08-27 08:33_

---

_Comment by @zanieb on 2024-08-27 11:37_

Definitely. 

Example at https://github.com/PrefectHQ/prefect/blob/0b747dc7b4c9f94d5e8ffe7e6ab7a045de7451e0/.github/workflows/docker-images.yaml#L110-L116

---

_Closed by @zanieb on 2024-08-28 13:31_

---
