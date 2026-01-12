```yaml
number: 5034
title: Fix Fedora system test
type: pull_request
state: merged
author: j178
labels:
  - testing
assignees: []
merged: true
base: main
head: fedora
created_at: 2024-07-13T06:13:58Z
updated_at: 2024-07-14T05:59:26Z
url: https://github.com/astral-sh/uv/pull/5034
synced_at: 2026-01-12T16:06:35Z
```

# Fix Fedora system test

---

_@j178_

Seems like the latest [`fedora:41`](https://hub.docker.com/layers/library/fedora/rawhide/images/sha256-c037a87094660ceda037ee319b17f59559241d2a3580d1d0f414e72b0a8f3827?context=explore) (pushed at Jul 12, 2024 at 22:05 UTC) docker image has removed the `python3` package, causing our current [CI failure](https://github.com/astral-sh/uv/actions/runs/9917708846/job/27401528178).


---

_@zanieb approved on 2024-07-13 15:24_

Thank you!

---

_Label `testing` added by @zanieb on 2024-07-13 15:25_

---

_Merged by @zanieb on 2024-07-13 15:25_

---

_Closed by @zanieb on 2024-07-13 15:25_

---

_Branch deleted on 2024-07-14 05:59_

---
