```yaml
number: 15202
title: Add hint for venv in source distribution error
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/venv-in-source-dist-hint
created_at: 2025-08-10T13:42:40Z
updated_at: 2025-08-19T10:56:03Z
url: https://github.com/astral-sh/uv/pull/15202
synced_at: 2026-01-12T16:11:37Z
```

# Add hint for venv in source distribution error

---

_@konstin_

Venvs should not be in source distributions, and on Unix, we now reject them for having a link outside the source directory. This PR adds a hint for that since users were confused (#15096).

In the process, we're differentiating IO errors for format error for uncompression generally.

Fixes #15096

---

_Label `error messages` added by @konstin on 2025-08-10 13:42_

---

_@konstin reviewed on 2025-08-10 13:43_

---

_Review comment by @konstin on `crates/uv-extract/src/error.rs`:9 on 2025-08-10 13:43_

The `during async read` allows us to tell apart the code path.

---

_@konstin reviewed on 2025-08-10 17:31_

---

_Review comment by @konstin on `crates/uv/src/commands/build_frontend.rs`:398 on 2025-08-10 17:31_

The tokio-tar crate has an effectively stringly-types, very nested, everything-is-an-io::Error interface.

---

_Renamed from "Hint about error from venv in source distribution" to "Add hint for venv in source distribution error" by @konstin on 2025-08-18 16:48_

---

_Review requested from @zanieb by @konstin on 2025-08-18 16:48_

---

_@zanieb reviewed on 2025-08-18 17:19_

---

_Review comment by @zanieb on `crates/uv-extract/src/error.rs`:9 on 2025-08-18 17:19_

To tell it apart from which case? I would rather not leak that it's async to a user.

---

_@zanieb approved on 2025-08-18 17:21_

---

_@konstin reviewed on 2025-08-18 18:35_

---

_Review comment by @konstin on `crates/uv-extract/src/error.rs`:9 on 2025-08-18 18:35_

Telling apart `zip` errors vs. `async-zip` errors from user reports. I've made it more sneaky now by using two slightly different strings that effectively say the same but lead us to the correct location when grepping through the code.

---

_Merged by @konstin on 2025-08-18 22:07_

---

_Closed by @konstin on 2025-08-18 22:07_

---

_Branch deleted on 2025-08-18 22:07_

---

_Comment by @musicinmybrain on 2025-08-19 10:40_

Looking at https://github.com/astral-sh/tokio-tar/commit/f1488188f1d1b54a73eb0c42a8b8f4b9ee87d688, are you planning to release `astral-tokio-tar` 0.5.3 and change `astral-tokio-tar = { git = "https://github.com/astral-sh/tokio-tar", rev = "f1488188f1d1b54a73eb0c42a8b8f4b9ee87d688" }` back to `astral-tokio-tar = { version = "0.5.3" }`?

---

_Comment by @konstin on 2025-08-19 10:56_

Yes, we plan doing a new release in https://github.com/astral-sh/tokio-tar soon.

---
