```yaml
number: 874
title: Only write to rust cache in CI from main branch
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/cache-main
created_at: 2024-01-10T20:51:28Z
updated_at: 2024-01-10T21:04:28Z
url: https://github.com/astral-sh/uv/pull/874
synced_at: 2026-01-10T15:39:02Z
```

# Only write to rust cache in CI from main branch

---

_Pull request opened by @zanieb on 2024-01-10 20:51_

Each cache entry is ~1 GB of our allotted 10 GB for the repository which is quite a bit. We're probably losing cache entries all the time since we add an entry per commit per pull request.

Saving the cache takes ~3 minutes ([example](https://github.com/astral-sh/puffin/actions/runs/7479909295/job/20358124969)), it's probably just slowing down CI. It's ~25% of our test runtime and ~50% of our clippy runtime.

---

_Review requested from @charliermarsh by @zanieb on 2024-01-10 21:00_

---

_@charliermarsh approved on 2024-01-10 21:02_

---

_Comment by @zanieb on 2024-01-10 21:04_

The effect of this may be complicated in cases where the cache does not need to be updated (i.e. the key is unchanged). Easy to revert if we're not seeing consistent improvements though.

---

_Merged by @zanieb on 2024-01-10 21:04_

---

_Closed by @zanieb on 2024-01-10 21:04_

---

_Branch deleted on 2024-01-10 21:04_

---
