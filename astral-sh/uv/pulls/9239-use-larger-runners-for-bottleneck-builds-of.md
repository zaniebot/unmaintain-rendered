```yaml
number: 9239
title: Use larger runners for bottleneck builds of release artifacts
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/release-runner
created_at: 2024-11-19T19:38:42Z
updated_at: 2024-11-19T20:36:01Z
url: https://github.com/astral-sh/uv/pull/9239
synced_at: 2026-01-10T12:00:00Z
```

# Use larger runners for bottleneck builds of release artifacts

---

_Pull request opened by @zanieb on 2024-11-19 19:38_

Uses a different runner for builds that take >15m.

Most of the builds finish in ~10 minutes.

---

_Label `internal` added by @zanieb on 2024-11-19 19:38_

---

_Marked ready for review by @zanieb on 2024-11-19 19:59_

---

_Comment by @zanieb on 2024-11-19 19:59_

The Windows jobs still look like a bottleneck, but I'm not sure I want to switch to the extra large runners.

---

_@charliermarsh approved on 2024-11-19 20:13_

---

_Comment by @charliermarsh on 2024-11-19 20:13_

Should we use _smaller_ runners for the fast builds or something? Probably a bad idea.

---

_Comment by @zanieb on 2024-11-19 20:23_

I don't think we can, we're using the GitHub defaults which are small and free.

If anything, I'd want to use the fast runners for everything and get the release time down so we don't need to monitor it as long — it's not worth the investment right now though.

---

_Merged by @zanieb on 2024-11-19 20:35_

---

_Closed by @zanieb on 2024-11-19 20:35_

---

_Branch deleted on 2024-11-19 20:36_

---
