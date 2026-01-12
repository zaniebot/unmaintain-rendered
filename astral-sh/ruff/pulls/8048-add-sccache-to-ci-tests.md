```yaml
number: 8048
title: Add sccache to CI tests
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zanie/test-sccache
created_at: 2023-10-18T15:29:19Z
updated_at: 2023-10-24T15:23:01Z
url: https://github.com/astral-sh/ruff/pull/8048
synced_at: 2026-01-12T15:55:25Z
```

# Add sccache to CI tests

---

_@zanieb_

_No description provided._

---

_Comment by @MichaReiser on 2023-10-18 23:16_

Where does sccache store the cache artifacts?

---

_Comment by @zanieb on 2023-10-18 23:48_

I think it reuses the GitHub Actions cache. It doesn't appear to be faster here though.

---

_Comment by @MichaReiser on 2023-10-19 00:08_

<img width="989" alt="Screenshot 2023-10-19 at 09 08 18" src="https://github.com/astral-sh/ruff/assets/1203881/ce5d2269-7ada-4e43-8a05-f57f7aa12a15">

The caches are much smaller but it generates a ton of them


---

_@MichaReiser reviewed on 2023-10-19 00:10_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:104 on 2023-10-19 00:10_

Should we remove `rust-cache` for the test to avoid double caching (or the two conflicting)?

---

_@zanieb reviewed on 2023-10-19 13:51_

---

_Review comment by @zanieb on `.github/workflows/ci.yaml`:104 on 2023-10-19 13:51_

The pull request we were referencing (that showed a speedup) had both setup

---

_Comment by @zanieb on 2023-10-19 13:52_

If it's smaller overall that'd probably be good, I don't think things got slower..

---

_Closed by @zanieb on 2023-10-24 15:23_

---
