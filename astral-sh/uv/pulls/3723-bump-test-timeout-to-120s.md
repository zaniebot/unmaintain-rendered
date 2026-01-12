```yaml
number: 3723
title: Bump test timeout to 120s
type: pull_request
state: closed
author: zanieb
labels:
  - internal
  - testing
assignees: []
base: main
head: zb/timeout
created_at: 2024-05-21T21:50:29Z
updated_at: 2024-05-21T21:56:51Z
url: https://github.com/astral-sh/uv/pull/3723
synced_at: 2026-01-12T16:05:49Z
```

# Bump test timeout to 120s

---

_@zanieb_

90s is too low in our test suite :sob:


---

_Label `internal` added by @zanieb on 2024-05-21 21:50_

---

_Label `testing` added by @zanieb on 2024-05-21 21:50_

---

_Comment by @ibraheemdev on 2024-05-21 21:52_

Even after https://github.com/astral-sh/uv/pull/3714?

---

_Comment by @zanieb on 2024-05-21 21:53_

Yeah I saw it flake on `find_links_offline_match` at https://github.com/astral-sh/uv/actions/runs/9181553313/job/25248540367?pr=3677

---

_Comment by @ibraheemdev on 2024-05-21 21:55_

That seems like a legitimate deadlock? Usually takes just over a second https://github.com/astral-sh/uv/actions/runs/9179591991/job/25242063661#step:8:753.

---

_Comment by @zanieb on 2024-05-21 21:56_

_sigh_ I guess I'm bit by the deadlock haha

---

_Comment by @zanieb on 2024-05-21 21:56_

Yeah I was just looking at that thinking it shouldn't be slow.

---

_Closed by @zanieb on 2024-05-21 21:56_

---
