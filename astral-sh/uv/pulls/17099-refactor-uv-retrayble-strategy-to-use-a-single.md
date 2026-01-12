```yaml
number: 17099
title: Refactor uv retrayble strategy to use a single code path
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/better-retry-handling-3
created_at: 2025-12-12T11:33:42Z
updated_at: 2025-12-18T10:10:51Z
url: https://github.com/astral-sh/uv/pull/17099
synced_at: 2026-01-12T16:12:36Z
```

# Refactor uv retrayble strategy to use a single code path

---

_@konstin_

Refactoring that allows uv's retryable strategy to return `Some(Retryable::Fatal)`, also helpful for https://github.com/astral-sh/uv/pull/16245

---

_Label `internal` added by @konstin on 2025-12-12 11:33_

---

_Converted to draft by @konstin on 2025-12-12 12:43_

---

_@zanieb reviewed on 2025-12-12 13:18_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:1057 on 2025-12-12 13:18_

Is this documentation for this function up-to-date still?

---

_@zanieb reviewed on 2025-12-12 13:18_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:1027 on 2025-12-12 13:18_

Can we rename this to `retryable_on_request_failure` for consistency with the `reqwest_retry` naming?

---

_@zanieb reviewed on 2025-12-12 13:19_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:1026 on 2025-12-12 13:19_

This this comment just be on the `uv_retryable_strategy` doc comment? I think it makes more sense there?

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:1057 on 2025-12-15 13:31_

I've updated it

---

_@konstin reviewed on 2025-12-15 13:31_

---

_Marked ready for review by @konstin on 2025-12-15 13:31_

---

_Review requested from @zanieb by @konstin on 2025-12-16 09:48_

---

_@EliteTK approved on 2025-12-16 14:29_

Looked at this in the context of the other changes and in the context of what I've learned about this code yesterday/today. Looks good from my end.

---

_Merged by @konstin on 2025-12-18 10:10_

---

_Closed by @konstin on 2025-12-18 10:10_

---

_Branch deleted on 2025-12-18 10:10_

---
