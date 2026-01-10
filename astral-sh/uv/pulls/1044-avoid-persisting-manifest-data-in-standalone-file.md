```yaml
number: 1044
title: Avoid persisting manifest data in standalone file
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/manifest
created_at: 2024-01-22T20:16:08Z
updated_at: 2024-01-23T19:46:50Z
url: https://github.com/astral-sh/uv/pull/1044
synced_at: 2026-01-10T15:39:03Z
```

# Avoid persisting manifest data in standalone file

---

_Pull request opened by @charliermarsh on 2024-01-22 20:16_

## Summary

This PR gets rid of the manifest that we store for source distributions. Historically, that manifest included the source distribution metadata, plus a list of built wheels.

The problem with the manifest is that it duplicates state, since we now have to look at both the manifest and the filesystem to understand the cache state. Instead, I think we should treat the cache as the source of truth, and get rid of the duplicated state in the manifest.

Now, we store the manifest (which is merely used to check for cache freshness -- in future PRs, I will repurpose it though, so I left it around), then the distribution metadata as its own file, then any distributions in the same directory. When we want to see if there are any valid distributions, we `readdir` on the directory. This is also much more consistent with how the install plan works.


---

_Marked ready for review by @charliermarsh on 2024-01-22 20:19_

---

_Review requested from @konstin by @charliermarsh on 2024-01-22 20:19_

---

_@konstin approved on 2024-01-23 14:47_

The `CacheBucket` docs need updating

---

_Merged by @charliermarsh on 2024-01-23 19:46_

---

_Closed by @charliermarsh on 2024-01-23 19:46_

---

_Branch deleted on 2024-01-23 19:46_

---
