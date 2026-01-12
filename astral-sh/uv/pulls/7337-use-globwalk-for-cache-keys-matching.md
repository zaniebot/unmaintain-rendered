```yaml
number: 7337
title: "Use `globwalk` for `cache-keys` matching"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/globwalk
created_at: 2024-09-12T18:22:00Z
updated_at: 2024-09-12T19:06:07Z
url: https://github.com/astral-sh/uv/pull/7337
synced_at: 2026-01-12T16:07:47Z
```

# Use `globwalk` for `cache-keys` matching

---

_@charliermarsh_

## Summary

This should be more efficient as we can do a single traversal.

Closes https://github.com/astral-sh/uv/issues/7321.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-12 18:22_

---

_Marked ready for review by @charliermarsh on 2024-09-12 18:22_

---

_Label `performance` added by @charliermarsh on 2024-09-12 18:22_

---

_Review comment by @BurntSushi on `crates/uv-cache-info/src/cache_info.rs`:85 on 2024-09-12 18:38_

Nice.

---

_@BurntSushi approved on 2024-09-12 18:39_

LGTM. Thank you for doing this!

---

_Merged by @charliermarsh on 2024-09-12 19:06_

---

_Closed by @charliermarsh on 2024-09-12 19:06_

---

_Branch deleted on 2024-09-12 19:06_

---
