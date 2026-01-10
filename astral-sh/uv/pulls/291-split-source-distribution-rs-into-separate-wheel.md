```yaml
number: 291
title: "Split `source_distribution.rs` into separate wheel and sdist fetchers"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/split
created_at: 2023-11-02T15:46:07Z
updated_at: 2023-11-02T16:04:53Z
url: https://github.com/astral-sh/uv/pull/291
synced_at: 2026-01-10T15:50:28Z
```

# Split `source_distribution.rs` into separate wheel and sdist fetchers

---

_Pull request opened by @charliermarsh on 2023-11-02 15:46_

_No description provided._

---

_Marked ready for review by @charliermarsh on 2023-11-02 15:46_

---

_Review comment by @konstin on `crates/puffin-resolver/src/distribution/built_distribution.rs`:21 on 2023-11-02 15:49_

I'd call this `WheelFetcher`, it doesn't support other built distributions

---

_Review comment by @konstin on `crates/puffin-resolver/src/distribution/built_distribution.rs`:25 on 2023-11-02 15:50_

I'd only pass the cache here, it's not nice that it's just a path but we don't need the whole build context

---

_@konstin approved on 2023-11-02 15:57_

---

_Merged by @charliermarsh on 2023-11-02 16:04_

---

_Closed by @charliermarsh on 2023-11-02 16:04_

---

_Branch deleted on 2023-11-02 16:04_

---
