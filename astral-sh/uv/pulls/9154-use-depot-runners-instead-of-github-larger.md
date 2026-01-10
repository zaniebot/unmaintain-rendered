```yaml
number: 9154
title: Use Depot runners instead of GitHub larger runners for Unix
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/debot
created_at: 2024-11-15T18:29:51Z
updated_at: 2024-11-15T23:07:55Z
url: https://github.com/astral-sh/uv/pull/9154
synced_at: 2026-01-10T12:00:00Z
```

# Use Depot runners instead of GitHub larger runners for Unix

---

_Pull request opened by @zanieb on 2024-11-15 18:29_

_No description provided._

---

_Label `internal` added by @zanieb on 2024-11-15 18:29_

---

_Marked ready for review by @zanieb on 2024-11-15 22:07_

---

_Comment by @zanieb on 2024-11-15 22:08_

I don't see any reason not to give this a try for a bit. A brief analysis shows these are faster:

```
Test job (total)

macOS: 6m 21s -> 5m 17s 
Linux: 4m 12s -> 2m 23s

Cache restore

macOS: 22s -> 31s
Linux: 25s -> 14s

Test run

macOS: 5m 25s -> 4m 30s
Linux: 3m 22s -> 1m 52s
```

And will save us about 20% cost (not accounting for the speed-up)

---

_@charliermarsh approved on 2024-11-15 23:02_

---

_Merged by @zanieb on 2024-11-15 23:07_

---

_Closed by @zanieb on 2024-11-15 23:07_

---

_Branch deleted on 2024-11-15 23:07_

---
