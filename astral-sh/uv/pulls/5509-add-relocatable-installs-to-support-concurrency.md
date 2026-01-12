```yaml
number: 5509
title: Add relocatable installs to support concurrency-safe cached environments
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/safe
created_at: 2024-07-27T17:05:29Z
updated_at: 2024-07-29T00:32:12Z
url: https://github.com/astral-sh/uv/pull/5509
synced_at: 2026-01-12T16:06:51Z
```

# Add relocatable installs to support concurrency-safe cached environments

---

_@charliermarsh_

## Summary

The idea here is similar to what we do for wheels: we create the `CachedEnvironment` in the `archive-v0` bucket, then symlink it to its content-addressed location. This ensures that we can always recreate these environments without concern for whether anyone else is accessing them.

Part of the challenge here is that we want the virtual environments to be relocatable, because we're now building them in one location but persisting them in another. This requires that we write relative (rather than absolute) paths to scripts and entrypoints. The main risk with relocatable virtual environments is that the scripts and entrypoints _themselves_ are not relocatable, because they use a relative shebang. But that's fine for cached environments, which are never intended to leave the cache.

Closes https://github.com/astral-sh/uv/issues/5503.


---

_Label `bug` added by @charliermarsh on 2024-07-27 17:05_

---

_Label `enhancement` added by @charliermarsh on 2024-07-27 17:05_

---

_Label `preview` added by @charliermarsh on 2024-07-27 17:05_

---

_Review requested from @konstin by @charliermarsh on 2024-07-27 17:51_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-27 18:41_

---

_Merged by @charliermarsh on 2024-07-29 00:32_

---

_Closed by @charliermarsh on 2024-07-29 00:32_

---

_Branch deleted on 2024-07-29 00:32_

---
