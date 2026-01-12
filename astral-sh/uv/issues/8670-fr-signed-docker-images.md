```yaml
number: 8670
title: "FR: signed docker images"
type: issue
state: closed
author: mjpieters
labels:
  - help wanted
  - releases
assignees: []
created_at: 2024-10-29T17:09:50Z
updated_at: 2025-01-31T15:00:24Z
url: https://github.com/astral-sh/uv/issues/8670
synced_at: 2026-01-12T15:59:31Z
```

# FR: signed docker images

---

_@mjpieters_

We'd appreciate it greatly if the docker images produced for uv were signed.

Github has provided a [blog post with a sample Github Actions step](https://github.blog/security/supply-chain-security/safeguard-container-signing-capability-actions/), as well as a [sample workflow](https://github.com/actions/starter-workflows/blob/1394e47812550adc70e4af1b714a8000ceee6c00/ci/docker-publish.yml#L85-L98) that uses key-less signing with `cosign`, to accomplish this.

---

_Renamed from "FR: signed docker containers" to "FR: signed docker images" by @mjpieters on 2024-10-29 17:10_

---

_Label `help wanted` added by @zanieb on 2024-10-29 17:18_

---

_Label `releases` added by @zanieb on 2024-10-29 17:18_

---

_Closed by @zanieb on 2025-01-31 15:00_

---
