```yaml
number: 15274
title: Persist cache info when re-installing cached wheels
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cache-info
created_at: 2025-08-14T11:48:28Z
updated_at: 2025-08-14T12:05:42Z
url: https://github.com/astral-sh/uv/pull/15274
synced_at: 2026-01-12T16:11:40Z
```

# Persist cache info when re-installing cached wheels

---

_@charliermarsh_

## Summary

I noticed that these paths aren't returning the cache information, so if you install through these paths, we actually don't write `uv_cache.json` at all. I'm not sure how a user would actually end up here, because assuming there are no bugs, we don't really ever use this path? The install plan indexes the cached wheels and marks the wheel as installed, which means it's typically a mistake if we're asking the `DistributionDatabase` for a wheel that's already available in the cache... But I did verify that if I _skip_ the install plan's cache lookup, we write a wheel without `uv_cache.json`, so this is definitely more correct.


---

_Label `bug` added by @charliermarsh on 2025-08-14 11:48_

---

_Marked ready for review by @charliermarsh on 2025-08-14 11:48_

---

_@charliermarsh reviewed on 2025-08-14 11:50_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/source/built_wheel_metadata.rs`:60 on 2025-08-14 11:50_

Instead of using these `with_` methods, I split up the struct to make the fields required (which makes this mistake much harder to do).

---

_Merged by @charliermarsh on 2025-08-14 12:05_

---

_Closed by @charliermarsh on 2025-08-14 12:05_

---

_Branch deleted on 2025-08-14 12:05_

---
