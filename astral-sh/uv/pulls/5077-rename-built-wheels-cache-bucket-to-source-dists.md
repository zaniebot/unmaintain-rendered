```yaml
number: 5077
title: "Rename \"built-wheels\" cache bucket to \"source-dists\""
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - cache
assignees: []
merged: true
base: main
head: zb/sdist-cache-bucket
created_at: 2024-07-15T17:56:02Z
updated_at: 2024-07-15T19:41:05Z
url: https://github.com/astral-sh/uv/pull/5077
synced_at: 2026-01-10T13:42:52Z
```

# Rename "built-wheels" cache bucket to "source-dists"

---

_Pull request opened by @zanieb on 2024-07-15 17:56_

This name should lead to less confusion. Unfortunately this is a "breaking cache change" so everyone's cache will be invalidated. I'm not sure if we should support a rename-on-upgrade.

edit: We can make the breaking change next time we bump the version

---

_Review comment by @zanieb on `crates/uv-cache/src/lib.rs`:541 on 2024-07-15 17:57_

There is more in this docstring that should change but I do not feel like I have the context on the changes to the cache design to do so.

---

_@zanieb reviewed on 2024-07-15 17:57_

---

_Review requested from @charliermarsh by @zanieb on 2024-07-15 18:30_

---

_Label `cache` added by @zanieb on 2024-07-15 18:31_

---

_Comment by @charliermarsh on 2024-07-15 19:26_

I think this is reasonable but I don't think it merits bumping the cache version. Can we look for an opportunity to do this next time bump the cache?

---

_Marked ready for review by @zanieb on 2024-07-15 19:29_

---

_Comment by @zanieb on 2024-07-15 19:29_

I dropped the user-facing name change

---

_Label `internal` added by @zanieb on 2024-07-15 19:29_

---

_@charliermarsh approved on 2024-07-15 19:40_

---

_Comment by @charliermarsh on 2024-07-15 19:40_

Thanks!

---

_Merged by @zanieb on 2024-07-15 19:41_

---

_Closed by @zanieb on 2024-07-15 19:41_

---

_Branch deleted on 2024-07-15 19:41_

---
