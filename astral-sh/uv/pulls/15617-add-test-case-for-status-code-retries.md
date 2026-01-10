```yaml
number: 15617
title: Add test case for status code retries
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/retry-status
created_at: 2025-09-01T10:25:20Z
updated_at: 2025-09-02T13:41:56Z
url: https://github.com/astral-sh/uv/pull/15617
synced_at: 2026-01-10T06:44:33Z
```

# Add test case for status code retries

---

_Pull request opened by @konstin on 2025-09-01 10:25_

When migrating from the `reqwest_retry` crate, we want to ensure that the status codes we retry stay the same. This also helps us to intentionally migrate to a different list later, by enumerating the list of status codes that are retried.


---

_Label `internal` added by @konstin on 2025-09-01 10:25_

---

_@konstin reviewed on 2025-09-01 10:26_

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:967 on 2025-09-01 10:26_

Logic fix for cases where the error is not nested in a thiserror type.

---

_@zanieb approved on 2025-09-02 13:16_

---

_Merged by @konstin on 2025-09-02 13:41_

---

_Closed by @konstin on 2025-09-02 13:41_

---

_Branch deleted on 2025-09-02 13:41_

---
