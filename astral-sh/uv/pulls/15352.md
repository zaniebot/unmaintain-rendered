```yaml
number: 15352
title: Promote trixie and alpine 3.22 as default tags
type: pull_request
state: merged
author: samypr100
labels:
  - breaking
assignees: []
merged: true
base: main
head: change-default-docker
created_at: 2025-08-18T15:00:22Z
updated_at: 2025-10-07T23:10:38Z
url: https://github.com/astral-sh/uv/pull/15352
synced_at: 2026-01-10T06:36:15Z
```

# Promote trixie and alpine 3.22 as default tags

---

_Pull request opened by @samypr100 on 2025-08-18 15:00_

## Summary

Semi related to https://github.com/astral-sh/uv/issues/15270, except there's no removal.

Makes Alpine 3.22 and debian trixie the default tags instead of removing bookworm and alpine 3.21 to minimize churn.

~~This PR is pending https://github.com/astral-sh/uv/pull/15351 merged first.~~ Merged

## Test Plan

No functional changes made besides changing tag pointers.

---

_Label `breaking` added by @samypr100 on 2025-08-18 15:00_

---

_Added to milestone `v0.9.0` by @samypr100 on 2025-08-18 15:00_

---

_Marked ready for review by @samypr100 on 2025-08-19 00:05_

---

_Comment by @samypr100 on 2025-09-19 20:32_

Note, the OIDC changes are unrelated to this PR but likely due to the OIDC setup related to my fork and depot.

---

_Renamed from "chore: make trixie and alpine 3.22 the default tags" to "Promote trixie and alpine 3.22 as default tags" by @samypr100 on 2025-09-23 22:16_

---

_Comment by @samypr100 on 2025-10-07 21:06_

Failures seem to be due to rate limits

---

_@samypr100 reviewed on 2025-10-07 21:09_

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:186 on 2025-10-07 21:09_

Thanks @geofft for noticing the repeated `debian-slim`, `debian` shared tags, not sure why that occurred.

---

_Merged by @zanieb on 2025-10-07 22:26_

---

_Closed by @zanieb on 2025-10-07 22:26_

---

_Branch deleted on 2025-10-07 23:10_

---
