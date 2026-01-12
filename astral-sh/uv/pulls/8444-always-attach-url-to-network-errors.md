```yaml
number: 8444
title: Always attach URL to network errors
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - network
assignees: []
merged: true
base: main
head: konsti/reqwest-explicit-error
created_at: 2024-10-22T10:48:47Z
updated_at: 2024-10-25T09:10:19Z
url: https://github.com/astral-sh/uv/pull/8444
synced_at: 2026-01-12T16:08:19Z
```

# Always attach URL to network errors

---

_@konstin_

Due to the enduring problems with #8144 and related issues, I've opted for a more systemic approach and switched all reqwest errors to not use the implicit-try-fallthrough `#[from]`, but an explicit `#[source]` with a URL attached (except for when there is only one or no URL). This guarantees that we get the URL that failed, and helps identifying the responsible code path.



---

_Label `bug` added by @konstin on 2024-10-22 10:48_

---

_Label `network` added by @konstin on 2024-10-22 10:48_

---

_Review requested from @charliermarsh by @konstin on 2024-10-22 10:48_

---

_Review requested from @zanieb by @konstin on 2024-10-22 10:48_

---

_Comment by @zanieb on 2024-10-22 11:42_

It's a bit of a bummer that in all the snapshot changes here we just end up repeating the URL. 

---

_Comment by @konstin on 2024-10-22 11:47_

I'd prefer it if reqwest would always include the URL or otherwise have a switch to turn the optional URL off from duplication in error display, but lacking that it's better to repeat the URL than to end up with errors like https://github.com/astral-sh/uv/issues/8144#issuecomment-2426912260 that we can't trace anymore.

---

_Comment by @charliermarsh on 2024-10-22 13:10_

Could we only attach for certain error kinds?

---

_@charliermarsh reviewed on 2024-10-24 02:21_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_compile.rs`:9368 on 2024-10-24 02:21_

I think we should probably use the colon and backticks for this.

---

_@charliermarsh approved on 2024-10-24 02:21_

---

_Comment by @konstin on 2024-10-25 07:57_

For just the reqwest error messages and given the opaque reqwest error type, it doesn't make sense to optimize this too much, so we accept the slight regression in the error trace for being able to fix bugs like #8144 and ensuring that we show the user the error URL.

---

_Merged by @konstin on 2024-10-25 09:10_

---

_Closed by @konstin on 2024-10-25 09:10_

---

_Branch deleted on 2024-10-25 09:10_

---
