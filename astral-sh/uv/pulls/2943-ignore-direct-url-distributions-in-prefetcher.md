```yaml
number: 2943
title: Ignore direct URL distributions in prefetcher
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/prefetch
created_at: 2024-04-09T18:57:40Z
updated_at: 2024-04-09T19:11:09Z
url: https://github.com/astral-sh/uv/pull/2943
synced_at: 2026-01-12T16:05:19Z
```

# Ignore direct URL distributions in prefetcher

---

_@charliermarsh_

## Summary

The prefetcher tallies the number of times we tried a given package, and then once we hit a threshold, grabs the version map, assuming it's already been fetched. For direct URL distributions, though, we don't have a version map! And there's no need to prefetch.

Closes https://github.com/astral-sh/uv/issues/2941.

---

_Marked ready for review by @charliermarsh on 2024-04-09 18:58_

---

_Label `bug` added by @charliermarsh on 2024-04-09 18:58_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-09 18:58_

---

_Review requested from @konstin by @charliermarsh on 2024-04-09 18:58_

---

_@zanieb reviewed on 2024-04-09 18:59_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/batch_prefetch.rs`:55 on 2024-04-09 18:59_

Why `None` for the extras field?

---

_@charliermarsh reviewed on 2024-04-09 19:02_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/batch_prefetch.rs`:55 on 2024-04-09 19:02_

"No URL, no extra"

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/batch_prefetch.rs`:55 on 2024-04-09 19:02_

Oh sorry

---

_@charliermarsh reviewed on 2024-04-09 19:02_

---

_@charliermarsh reviewed on 2024-04-09 19:03_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/batch_prefetch.rs`:55 on 2024-04-09 19:03_

If we include an extra, we'll also include the non-extra variant in the tree. So it seems fine to confine this to those non-extra variants, rather than duplicating the prefetch.

---

_@zanieb reviewed on 2024-04-09 19:09_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/batch_prefetch.rs`:55 on 2024-04-09 19:09_

okee

---

_Merged by @zanieb on 2024-04-09 19:09_

---

_Closed by @zanieb on 2024-04-09 19:09_

---

_Branch deleted on 2024-04-09 19:09_

---

_Comment by @zanieb on 2024-04-09 19:11_

Confirming in https://github.com/astral-sh/uv/pull/2942

---
