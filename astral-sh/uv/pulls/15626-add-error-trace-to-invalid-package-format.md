```yaml
number: 15626
title: Add error trace to invalid package format
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/show-error-trace-for-broken-package-layout
created_at: 2025-09-02T11:47:54Z
updated_at: 2025-09-02T13:22:43Z
url: https://github.com/astral-sh/uv/pull/15626
synced_at: 2026-01-12T16:11:51Z
```

# Add error trace to invalid package format

---

_@konstin_

In https://github.com/astral-sh/uv/issues/11636, we're getting reports for installation flakes that report an invalid package format for what appears to be a network problem. Since we're cutting the error reporting to the first error message in the chain, we're not reporting the actual network error underneath it.

This PR displays the whole error chain for invalid package format errors, so we can debug and eventually catch-and-retry https://github.com/astral-sh/uv/issues/11636.


---

_Review requested from @zanieb by @konstin on 2025-09-02 11:47_

---

_Label `error messages` added by @konstin on 2025-09-02 11:47_

---

_@konstin reviewed on 2025-09-02 11:48_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/availability.rs`:127 on 2025-09-02 11:48_

Name and location of the type is up for bikeshedding. Maybe we want a generic version in uv-warnings?

---

_@konstin reviewed on 2025-09-02 11:49_

---

_Review comment by @konstin on `crates/uv/tests/it/pip_sync.rs`:2627 on 2025-09-02 11:49_

There's a chance here that user's mistake this hint for an error message (since we're using the familiar error format), not sure if we can make it clearer that the resolution error trace above is the actual error.

---

_@zanieb approved on 2025-09-02 13:11_

---

_Merged by @konstin on 2025-09-02 13:22_

---

_Closed by @konstin on 2025-09-02 13:22_

---

_Branch deleted on 2025-09-02 13:22_

---
