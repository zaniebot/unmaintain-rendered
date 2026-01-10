```yaml
number: 2458
title: Add system install test for Rosetta on macOS
type: pull_request
state: closed
author: zanieb
labels:
  - testing
assignees: []
draft: true
base: main
head: zb/rosetta
created_at: 2024-03-14T14:36:06Z
updated_at: 2024-03-20T20:38:03Z
url: https://github.com/astral-sh/uv/pull/2458
synced_at: 2026-01-10T14:49:08Z
```

# Add system install test for Rosetta on macOS

---

_Pull request opened by @zanieb on 2024-03-14 14:36_

_No description provided._

---

_Label `testing` added by @zanieb on 2024-03-14 14:36_

---

_Comment by @zanieb on 2024-03-15 14:58_

@charliermarsh Could you take a look at this failure? I'm not sure if this is worth including but I want to make sure there's not a bug.

---

_@samypr100 reviewed on 2024-03-19 01:19_

---

_Review comment by @samypr100 on `.github/workflows/ci.yml`:394 on 2024-03-19 01:19_

In the brew install logs I see the bottle is being downloaded for sonoma, meaning it's the pre-built version of 3.8 for sonoma which is likely only arm64.  You might need to add `HOMEBREW_BUILD_FROM_SOURCE=1` or `--build-from-source` to brew install, although that will make it painfully slow.

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:394 on 2024-03-19 02:30_

Interesting thanks. It's confusing because this installed correctly in some previous runs but is failing now 🤷‍♀️ I'm not sure this coverage is worth it in general because installing Rosetta Brew is an order of magnitude slower than all the other tests.

---

_@zanieb reviewed on 2024-03-19 02:30_

---

_Closed by @zanieb on 2024-03-20 20:38_

---
