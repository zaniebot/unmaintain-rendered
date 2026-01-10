```yaml
number: 10830
title: Separate musl and libc linux builds in CI
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: zb/libc-musl
created_at: 2025-01-21T22:07:38Z
updated_at: 2025-01-23T21:19:28Z
url: https://github.com/astral-sh/uv/pull/10830
synced_at: 2026-01-10T11:45:13Z
```

# Separate musl and libc linux builds in CI

---

_Pull request opened by @zanieb on 2025-01-21 22:07_

We have a lot of jobs downstream of the `build-binary-linux` job, but the job is significantly slower than the other binary builds because we need to configure musl. Instead, we split this into two jobs (as it was before https://github.com/astral-sh/uv/pull/2309#discussion_r1520101330) to speed things up.

The libc job takes ~1m and its _downstream_ jobs finish before the musl build does. The musl job takes ~5m.

---

_Label `testing` added by @zanieb on 2025-01-21 22:07_

---

_Label `internal` added by @zanieb on 2025-01-21 22:07_

---

_Marked ready for review by @zanieb on 2025-01-21 22:34_

---

_@konstin approved on 2025-01-23 20:23_

I'm surprised musl takes so long to set up

---

_Merged by @zanieb on 2025-01-23 21:19_

---

_Closed by @zanieb on 2025-01-23 21:19_

---

_Branch deleted on 2025-01-23 21:19_

---
