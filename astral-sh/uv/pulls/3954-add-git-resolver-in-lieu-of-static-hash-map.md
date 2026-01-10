```yaml
number: 3954
title: Add Git resolver in lieu of static hash map
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/git-lock
created_at: 2024-06-01T02:31:24Z
updated_at: 2024-06-03T08:06:41Z
url: https://github.com/astral-sh/uv/pull/3954
synced_at: 2026-01-10T13:59:34Z
```

# Add Git resolver in lieu of static hash map

---

_Pull request opened by @charliermarsh on 2024-06-01 02:31_

## Summary

This PR removes the static resolver map:

```rust
static RESOLVED_GIT_REFS: Lazy<Mutex<FxHashMap<RepositoryReference, GitSha>>> =
    Lazy::new(Mutex::default);
```

With a `GitResolver` struct that we now pass around on the `BuildContext`. There should be no behavior changes here; it's purely an internal refactor with an eye towards making it cleaner for us to "pre-populate" the list of resolved SHAs.


---

_Marked ready for review by @charliermarsh on 2024-06-01 02:31_

---

_Label `internal` added by @charliermarsh on 2024-06-01 02:31_

---

_Merged by @charliermarsh on 2024-06-01 02:44_

---

_Closed by @charliermarsh on 2024-06-01 02:44_

---

_Branch deleted on 2024-06-01 02:44_

---

_Comment by @konstin on 2024-06-03 08:06_

:tada: 

---
