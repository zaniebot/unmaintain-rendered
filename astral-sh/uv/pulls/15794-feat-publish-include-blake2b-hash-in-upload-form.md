```yaml
number: 15794
title: "feat(publish): include blake2b hash in upload form"
type: pull_request
state: merged
author: woodruffw
labels: []
assignees: []
merged: true
base: main
head: ww/publish-blake2
created_at: 2025-09-11T16:36:56Z
updated_at: 2025-09-11T20:17:08Z
url: https://github.com/astral-sh/uv/pull/15794
synced_at: 2026-01-12T16:11:56Z
```

# feat(publish): include blake2b hash in upload form

---

_@woodruffw_

## Summary

This adds the `blake2_256_digest` form item on uploads, since some third-party indices apparently expect it. This is slightly different from the (rejected) idea of including an `md5_digest` since Blake2b is not broken like MD5 is.

To do this, I've tweaked the `hash_file` helper to return multiple hashes instead of just one. This makes getting the hashes-by-type out of the function slightly less pretty in terms of what Rust can assert about safe indexing.

Closes #15781.

## Test Plan

This should be covered under existing upload tests, since we include the blake2 hash unconditionally.

---

_Review requested from @charliermarsh by @woodruffw on 2025-09-11 16:36_

---

_Review requested from @konstin by @woodruffw on 2025-09-11 16:36_

---

_Assigned to @woodruffw by @woodruffw on 2025-09-11 16:36_

---

_Review comment by @woodruffw on `crates/uv-publish/src/lib.rs`:720 on 2025-09-11 16:37_

Flagging: I thought iter + find here would be slightly cleaner than indexing with `[0]` and `[1]`, since we may not want to assume the return order is the same as the request order in the `hash_file` parameters.

---

_@woodruffw reviewed on 2025-09-11 16:37_

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:720 on 2025-09-11 18:55_

We can assume the order, but this is fine too

---

_@konstin reviewed on 2025-09-11 18:55_

---

_@konstin approved on 2025-09-11 19:01_

---

_Merged by @woodruffw on 2025-09-11 20:17_

---

_Closed by @woodruffw on 2025-09-11 20:17_

---

_Branch deleted on 2025-09-11 20:17_

---
