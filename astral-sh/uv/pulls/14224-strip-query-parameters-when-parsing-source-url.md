```yaml
number: 14224
title: Strip query parameters when parsing source URL
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dep-url
created_at: 2025-06-23T18:37:41Z
updated_at: 2025-06-23T18:52:09Z
url: https://github.com/astral-sh/uv/pull/14224
synced_at: 2026-01-12T16:11:06Z
```

# Strip query parameters when parsing source URL

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/14217.


---

_Label `bug` added by @charliermarsh on 2025-06-23 18:38_

---

_@charliermarsh reviewed on 2025-06-23 18:42_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:2454 on 2025-06-23 18:42_

This is the fix -- using `url.base_str()` instead of `url.as_ref()`. The rest of these changes are improvements to the error message.

---

_Merged by @charliermarsh on 2025-06-23 18:52_

---

_Closed by @charliermarsh on 2025-06-23 18:52_

---

_Branch deleted on 2025-06-23 18:52_

---
