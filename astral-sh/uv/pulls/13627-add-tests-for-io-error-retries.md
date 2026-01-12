```yaml
number: 13627
title: Add tests for IO Error retries
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - network
assignees: []
merged: true
base: main
head: konsti/advanced-retry-tests
created_at: 2025-05-23T20:57:25Z
updated_at: 2025-06-13T09:57:47Z
url: https://github.com/astral-sh/uv/pull/13627
synced_at: 2026-01-12T16:10:46Z
```

# Add tests for IO Error retries

---

_@konstin_

Often, HTTP requests don't fail due to server errors, but from spurious network errors such as connection resets. reqwest surfaces these as `io::Error`, and we have to handle their retrying separately.

Companion PR: https://github.com/LukeMathWalker/wiremock-rs/pull/159

---

_Label `internal` added by @konstin on 2025-05-23 20:57_

---

_Label `network` added by @konstin on 2025-05-23 20:57_

---

_Assigned to @jtfmumm by @zanieb on 2025-05-28 14:27_

---

_Review requested from @jtfmumm by @konstin on 2025-05-30 10:18_

---

_@konstin reviewed on 2025-06-06 17:41_

---

_Review comment by @konstin on `crates/uv/tests/it/network.rs`:259 on 2025-06-06 17:41_

Note to self: This needs to become an `io_error_server`

---

_Comment by @jtfmumm on 2025-06-09 19:44_

`respond_with_err` is very useful. Thanks for adding that!

---

_Review comment by @jtfmumm on `Cargo.toml`:192 on 2025-06-09 19:48_

Are you waiting to merge until this is accepted upstream? 

---

_Review comment by @jtfmumm on `crates/uv/tests/it/network.rs`:16 on 2025-06-09 19:49_

Since we only use the URI in tests, can we just return that for now?

---

_@jtfmumm reviewed on 2025-06-09 19:50_

---

_@jtfmumm reviewed on 2025-06-09 19:51_

---

_Review comment by @jtfmumm on `Cargo.toml`:192 on 2025-06-09 19:51_

If not, can you add a comment explaining the `respond_with_err` change we're using?

---

_@jtfmumm approved on 2025-06-09 19:53_

---

_@konstin reviewed on 2025-06-10 10:02_

---

_Review comment by @konstin on `Cargo.toml`:192 on 2025-06-10 10:02_

Since upstream is not responding, I'll fork into the astral org and tag the commit instead.

---

_Review comment by @konstin on `crates/uv/tests/it/network.rs`:16 on 2025-06-10 10:03_

The mock server is closed on drop, so we need to keep the reference to keep it alive

---

_@konstin reviewed on 2025-06-10 10:03_

---

_@konstin reviewed on 2025-06-13 09:47_

---

_Review comment by @konstin on `Cargo.toml`:192 on 2025-06-13 09:47_

Updated to out fork

---

_Merged by @konstin on 2025-06-13 09:57_

---

_Closed by @konstin on 2025-06-13 09:57_

---

_Branch deleted on 2025-06-13 09:57_

---
