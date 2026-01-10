```yaml
number: 5744
title: Remove build step
type: pull_request
state: closed
author: blueraft
labels: []
assignees: []
draft: true
base: main
head: ci-partition
created_at: 2024-08-03T07:26:31Z
updated_at: 2024-08-07T20:20:30Z
url: https://github.com/astral-sh/uv/pull/5744
synced_at: 2026-01-10T13:31:54Z
```

# Remove build step

---

_Pull request opened by @blueraft on 2024-08-03 07:26_

Not reusing the archive build improves test times through partitioning, though it does increase the billable time.

@zanieb wdyt? 

Linux:
- 2m vs 4m on main

macOS:
- 2m40s vs 5m20s on main

windows:
- 5m vs 9m30s on main



---

_Converted to draft by @blueraft on 2024-08-03 07:26_

---

_Closed by @blueraft on 2024-08-03 07:28_

---

_Reopened by @blueraft on 2024-08-03 07:41_

---

_Assigned to @zanieb by @zanieb on 2024-08-05 13:41_

---

_Comment by @zanieb on 2024-08-06 20:22_

Kind of tempting, though the partitions are a pain for auto-merge / branch checks.

---

_Comment by @zanieb on 2024-08-06 20:22_

(Thanks for giving this a try, don't know why I assumed it'd be slower)

---

_Comment by @blueraft on 2024-08-06 21:13_

> though the partitions are a pain for auto-merge / branch checks.

Wonder if you can use the job id for matching this 🤔

---

_Comment by @zanieb on 2024-08-07 13:26_

I don't think so, GitHub branch protections are by job name -.-

---

_Comment by @blueraft on 2024-08-07 19:29_

Bummer. The larger runners did the trick though so I'll close this PR. 😅

---

_Closed by @blueraft on 2024-08-07 19:29_

---

_Branch deleted on 2024-08-07 20:20_

---
