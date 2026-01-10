```yaml
number: 13585
title: Add basic network error tests
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - error messages
  - network
assignees: []
merged: true
base: main
head: konsti/basic-retry-tests
created_at: 2025-05-21T21:15:17Z
updated_at: 2025-06-10T10:00:07Z
url: https://github.com/astral-sh/uv/pull/13585
synced_at: 2026-01-10T11:10:41Z
```

# Add basic network error tests

---

_Pull request opened by @konstin on 2025-05-21 21:15_

Add basic tests for error messages on retryable network errors.

This test mod is intended to grow to ensure that we handle retryable errors correctly and that we show the appropriate error message if we failed after retrying.

The starter tests show some common cases we've seen download errors in: simple and find links indexes, file downloads and Python installs.

For `io::Error` fault injection to test the reqwest `Err` path besides the HTTP status code `Ok` path, see https://github.com/LukeMathWalker/wiremock-rs/issues/149.


---

_Label `internal` added by @konstin on 2025-05-21 21:15_

---

_Label `error messages` added by @konstin on 2025-05-21 21:15_

---

_Label `network` added by @konstin on 2025-05-21 21:15_

---

_Assigned to @jtfmumm by @zanieb on 2025-05-28 15:12_

---

_Review requested from @jtfmumm by @konstin on 2025-05-30 10:18_

---

_@jtfmumm reviewed on 2025-06-09 19:54_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/network.rs`:16 on 2025-06-09 19:54_

It looks like you can break this out into a separate function for use across these tests and just return the URI.

---

_Review comment by @jtfmumm on `crates/uv/tests/it/network.rs`:16 on 2025-06-09 19:55_

You've already broken this out in the follow up PR so it may not be necessary here if you've touched all these cases.

---

_@jtfmumm reviewed on 2025-06-09 19:55_

---

_@jtfmumm approved on 2025-06-09 19:56_

---

_Merged by @konstin on 2025-06-10 10:00_

---

_Closed by @konstin on 2025-06-10 10:00_

---

_Branch deleted on 2025-06-10 10:00_

---
