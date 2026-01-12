```yaml
number: 113
title: In resolver, choose package with fewest matching versions
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2023-10-17T18:54:19Z
updated_at: 2023-11-09T01:28:13Z
url: https://github.com/astral-sh/uv/issues/113
synced_at: 2026-01-12T15:58:21Z
```

# In resolver, choose package with fewest matching versions

---

_@charliermarsh_

_No description provided._

---

_Comment by @charliermarsh on 2023-10-20 05:26_

Doing this efficiently is not trivial though and it does require iterating over _all_ available versions for every package (often, we can skip most of them).

Perhaps some heuristic whereby we only look at the number of versions if it's... pretty small?

---

_Label `performance` added by @charliermarsh on 2023-10-24 19:23_

---

_Added to milestone `Future` by @charliermarsh on 2023-10-25 04:32_

---

_Comment by @charliermarsh on 2023-10-29 21:12_

I realized we can't actually do this because we don't have all metadata available for all packages at once. We'd have to wait for all packages to be fetched. Unclear if that's worth it 🤔 It might be?

---

_Comment by @charliermarsh on 2023-10-29 21:27_

(We can't answer this until we have a more extensive benchmarking setup, IMO.)

---

_Closed by @charliermarsh on 2023-11-09 01:28_

---
