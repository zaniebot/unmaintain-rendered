---
number: 15262
title: Docker tag for 0.8.10 missing from extra image bases
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-08-13T21:09:06Z
updated_at: 2025-08-13T21:36:03Z
url: https://github.com/astral-sh/uv/issues/15262
synced_at: 2026-01-10T01:25:54Z
---

# Docker tag for 0.8.10 missing from extra image bases

---

_Issue opened by @zanieb on 2025-08-13 21:09_

e.g.

```
❯ docker pull ghcr.io/astral-sh/uv:0.8.9-bookworm
0.8.9-bookworm: Pulling from astral-sh/uv
6fbab1970a5a: Already exists 
e4b341315eac: Already exists 
1de51daaef20: Already exists 
2c8f8efa4105: Already exists 
c588e8cd1786: Downloading   10.4MB/18.35MB
^Ccontext canceled

❯ docker pull ghcr.io/astral-sh/uv:0.8.10-bookworm

What's next:
    View a summary of image vulnerabilities and recommendations → docker scout quickview ghcr.io/astral-sh/uv:0.8.10-bookworm
Error response from daemon: manifest unknown
```

---

_Label `bug` added by @zanieb on 2025-08-13 21:09_

---

_Comment by @zanieb on 2025-08-13 21:11_

See https://github.com/astral-sh/uv/actions/runs/16897040537/job/47868769738

```
{
    "target": {
      "docker-metadata-action": {
        "tags": [
          "ghcr.io/astral-sh/uv:0.8.9-bookworm-slim",
          "ghcr.io/astral-sh/uv:0.8-bookworm-slim",
          "ghcr.io/astral-sh/uv:0.8.9-debian-slim",
          "ghcr.io/astral-sh/uv:0.8-debian-slim",
          "ghcr.io/astral-sh/uv:bookworm-slim",
          "ghcr.io/astral-sh/uv:debian-slim",
          "docker.io/astral/uv:0.8.9-bookworm-slim",
          "docker.io/astral/uv:0.8-bookworm-slim",
          "docker.io/astral/uv:0.8.9-debian-slim",
          "docker.io/astral/uv:0.8-debian-slim",
          "docker.io/astral/uv:bookworm-slim",
          "docker.io/astral/uv:debian-slim"
        ],
        "args": {
          "DOCKER_META_IMAGES": "ghcr.io/astral-sh/uv,docker.io/astral/uv",
          "DOCKER_META_VERSION": "0.8.9-bookworm-slim"
        }
      }
    }
  }
```

vs https://github.com/astral-sh/uv/actions/runs/16948083120/job/48035818473

```
  {
    "target": {
      "docker-metadata-action": {
        "tags": [
          "ghcr.io/astral-sh/uv:bookworm-slim",
          "ghcr.io/astral-sh/uv:debian-slim",
          "docker.io/astral/uv:bookworm-slim",
          "docker.io/astral/uv:debian-slim"
        ],
        "args": {
          "DOCKER_META_IMAGES": "ghcr.io/astral-sh/uv,docker.io/astral/uv",
          "DOCKER_META_VERSION": "bookworm-slim"
        }
      }
    }
  }
```

---

_Comment by @zanieb on 2025-08-13 21:12_

Suspicious of https://github.com/astral-sh/uv/pull/15245

---

_Referenced in [astral-sh/uv#15263](../../astral-sh/uv/pulls/15263.md) on 2025-08-13 21:16_

---

_Closed by @zanieb on 2025-08-13 21:36_

---
