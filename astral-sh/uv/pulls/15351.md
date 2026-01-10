```yaml
number: 15351
title: "chore: add trixie base images and alpine 3.22 support"
type: pull_request
state: merged
author: samypr100
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-08-18T14:30:45Z
updated_at: 2025-08-19T00:02:27Z
url: https://github.com/astral-sh/uv/pull/15351
synced_at: 2026-01-10T06:44:33Z
```

# chore: add trixie base images and alpine 3.22 support

---

_Pull request opened by @samypr100 on 2025-08-18 14:30_

## Summary

Follow up to https://github.com/astral-sh/uv/pull/15269#issuecomment-3194000772

Enables the following additional images to be published
* buildpack-deps:trixie
* debian:trixie-slim
* alpine:3.22

## Test Plan

Existing Tests. The newly added images were checked manually.


---

_Renamed from "chore: add trixie base and alpine 3.22" to "chore: add trixie base images and alpine 3.22 support" by @samypr100 on 2025-08-18 14:30_

---

_Marked ready for review by @samypr100 on 2025-08-18 14:33_

---

_Review requested from @zanieb by @konstin on 2025-08-18 14:46_

---

_Comment by @samypr100 on 2025-08-18 15:08_

> Run depot/build-push-action@9785b135c3c76c33db102e45be96a25ab55cd507
> Depot version
> Attempting to acquire open-source pull request OIDC token
> Waiting for OIDC auth challenge 62d75eee-779d-4c82-8eda-9bdbc2da3ad7
> ...
> Unable to exchange open-source pull request OIDC token for temporary Depot token: Error: OIDC auth challenge 62d75eee-779d-4c82-8eda-9bdbc2da3ad7 timed out

Is this a known depot bug/issue?

---

_@zanieb approved on 2025-08-18 15:45_

---

_Comment by @zanieb on 2025-08-18 15:45_

Yeah that's a known issue. It should succeed on retry.

---

_Comment by @zanieb on 2025-08-18 22:32_

It isn't succeeding on retry. I'll follow-up with Depot.

---

_Merged by @zanieb on 2025-08-18 22:55_

---

_Closed by @zanieb on 2025-08-18 22:55_

---

_Branch deleted on 2025-08-19 00:02_

---
