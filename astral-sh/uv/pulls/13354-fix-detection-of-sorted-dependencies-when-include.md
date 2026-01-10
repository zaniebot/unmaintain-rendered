```yaml
number: 13354
title: Fix detection of sorted dependencies when include-group is used
type: pull_request
state: merged
author: RazerM
labels: []
assignees: []
merged: true
base: main
head: feature/uv-add-sorted-with-include-group
created_at: 2025-05-08T19:44:01Z
updated_at: 2025-05-10T18:00:28Z
url: https://github.com/astral-sh/uv/pull/13354
synced_at: 2026-01-10T11:10:41Z
```

# Fix detection of sorted dependencies when include-group is used

---

_Pull request opened by @RazerM on 2025-05-08 19:44_

This follows on from #13334 to fix another case.

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

If a dependency group contained any `{ include-group = "..." }` entries, the sort detection would bail out. The root cause of the problem was gating the sort detection behind `deps.iter().all(Value::is_str)`.

A public code search reveals that keeping include-groups at the top is by far the most common, but keeping them at the bottom isn't uncommon. In both of these cases, uv will now preserve the convention that is in use.

Unless I've missed it, I don't think uv supports `uv add`ing an include-group, and so that wasn't tested here.

## Test Plan

cargo test


---

_Assigned to @charliermarsh by @konstin on 2025-05-08 19:53_

---

_Review requested from @charliermarsh by @konstin on 2025-05-08 19:53_

---

_Review requested from @charliermarsh by @konstin on 2025-05-08 19:53_

---

_Comment by @zanieb on 2025-05-08 20:33_

(The failures are https://status.depot.dev)

---

_@charliermarsh approved on 2025-05-10 16:47_

This looks reasonable to me, thanks.

---

_Merged by @charliermarsh on 2025-05-10 18:00_

---

_Closed by @charliermarsh on 2025-05-10 18:00_

---
