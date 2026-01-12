```yaml
number: 15737
title: "Use the `windows` crate facade consistently"
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - windows
assignees: []
merged: true
base: main
head: konsti/windows-only-windows
created_at: 2025-09-08T16:05:33Z
updated_at: 2025-09-09T15:17:46Z
url: https://github.com/astral-sh/uv/pull/15737
synced_at: 2026-01-12T16:11:54Z
```

# Use the `windows` crate facade consistently

---

_@konstin_

The initial motivation for this change was that we were using both the `windows`, the `window_sys` and the `windows_core` crate in various places. These crates have slightly unconventional versioning scheme where there is a large workspace with the same version in general, but only some crates get breaking releases when a new breaking release happens, the others stay on the previous breaking version. The `windows` crate is a shim for all three of them, with a single version. This simplifies handling the versions.

Using `windows` over `windows_sys` has the advantage of a higher level error interface, we now get a `Result` for all windows API calls instead of C-style int-returns and get-last-error calls. This makes the uv-keyring crate more resilient.

We keep using the `windows_registry` crate, which provides a higher level interface to windows registry access.

---

_Review requested from @geofft by @konstin on 2025-09-08 16:05_

---

_Review requested from @zanieb by @konstin on 2025-09-08 16:05_

---

_Label `internal` added by @konstin on 2025-09-08 16:05_

---

_Label `windows` added by @konstin on 2025-09-08 16:05_

---

_@konstin reviewed on 2025-09-08 16:06_

---

_Review comment by @konstin on `crates/uv-keyring/src/windows.rs`:499 on 2025-09-08 16:06_

I didn't see any backwards conversion from `windows::core::Error` to `WIN32_ERROR`.

---

_@geofft reviewed on 2025-09-08 16:26_

crates/uv/src/windows_exception.rs LGTM, rest seems believable but I only skimmed. (The one thing we had recently about calling `GetLastError` on the same thread as the error is, I assume, fine, if the wrapper calls it for us.)

Is there any impact on binary size?

---

_Review comment by @konstin on `crates/uv-keyring/src/windows.rs`:497 on 2025-09-08 17:38_

We can also use the built-in `windows::core::Error` display here. It has better coverage, I didn't do so far because I avoided behavior changes.

---

_@konstin reviewed on 2025-09-08 17:39_

---

_@zanieb approved on 2025-09-08 17:39_

I wonder if we should document this somewhere

---

_Merged by @konstin on 2025-09-09 15:07_

---

_Closed by @konstin on 2025-09-09 15:07_

---

_Branch deleted on 2025-09-09 15:07_

---

_Comment by @konstin on 2025-09-09 15:17_

The binary size difference is unnoticeable: 20695782 bytes -> 20695408 bytes.

---
