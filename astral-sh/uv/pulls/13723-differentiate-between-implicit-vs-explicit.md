```yaml
number: 13723
title: Differentiate between implicit vs explicit architecture requests
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/arch-request
created_at: 2025-05-29T21:27:44Z
updated_at: 2025-05-29T22:04:58Z
url: https://github.com/astral-sh/uv/pull/13723
synced_at: 2026-01-10T11:10:42Z
```

# Differentiate between implicit vs explicit architecture requests

---

_Pull request opened by @zanieb on 2025-05-29 21:27_

In https://github.com/astral-sh/uv/pull/13721#issuecomment-2920530601 I presumed that all the installation problems in https://github.com/astral-sh/uv/pull/13722 were solved by https://github.com/astral-sh/uv/pull/13709 but they were not because we don't differentiate between implicit and explicit architecture requests so a request for `aarch64` is considered satisfied by an existing `x86-64` installation even if the user explicitly requested that architecture.

Now, we track if it was explicit or implicit, requiring an exact match in the former case, and a `supports` in the latter.

We considered doing this for other items in the request, like the operating system but we don't have a `supports()` concept there. It might make sense for libc in the future.

---

_@Gankra approved on 2025-05-29 21:36_

We probably shouldn't do the Deref impl, we want everything to grapple with this distinction and force us to audit it.

---

_Renamed from "Track implicit vs explicit architecture requests" to "Differentiate between implicit vs explicit architecture requests" by @zanieb on 2025-05-29 21:42_

---

_Marked ready for review by @zanieb on 2025-05-29 21:43_

---

_Comment by @zanieb on 2025-05-29 21:43_

(There should be no user facing change, unblocks https://github.com/astral-sh/uv/pull/13722)

---

_@zanieb reviewed on 2025-05-29 21:45_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:241 on 2025-05-29 21:45_

Dead code

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:146 on 2025-05-29 21:45_

Key changes

The first line is new behavior, requiring exact equality.

The second retains the desirable existing behavior.

---

_@zanieb reviewed on 2025-05-29 21:45_

---

_Merged by @zanieb on 2025-05-29 22:04_

---

_Closed by @zanieb on 2025-05-29 22:04_

---

_Branch deleted on 2025-05-29 22:04_

---
