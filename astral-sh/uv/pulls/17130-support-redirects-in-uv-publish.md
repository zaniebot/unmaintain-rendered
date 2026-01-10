```yaml
number: 17130
title: "Support redirects in `uv publish`"
type: pull_request
state: merged
author: dijor0310
labels:
  - enhancement
assignees: []
merged: true
base: main
head: diyor/add-redirect-uv-publish
created_at: 2025-12-14T23:38:06Z
updated_at: 2025-12-18T00:31:55Z
url: https://github.com/astral-sh/uv/pull/17130
synced_at: 2026-01-10T05:49:14Z
```

# Support redirects in `uv publish`

---

_Pull request opened by @dijor0310 on 2025-12-14 23:38_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Follow redirects for `uv publish`. Fixes #17126.

## Test Plan

<!-- How was it tested? -->
Added a unit test to test the custom redirect logic.

---

_Renamed from "add redirect logic to `uv publish`" to "Support redirects in `uv publish`" by @konstin on 2025-12-15 15:28_

---

_Label `enhancement` added by @konstin on 2025-12-15 15:29_

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:136 on 2025-12-15 15:31_

In this error message, isn't the number of retry always the maximum number of redirects? i.e., can we say something like "Too many redirects, only {n_past_redirections} redirects are allowed"?

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:493 on 2025-12-15 15:32_

There is a word like a "where" missing

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:515 on 2025-12-15 15:34_

```suggestion
                        debug!("Redirecting the request to: {}", current_registry);
```

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:513 on 2025-12-15 15:37_

We can't unwrap here, we need to handle those errors gracefully. In uv production code, we need to avoid panics whenever possible; In this case, it's external input, so we have to assume it's in some way malformed.

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:1769 on 2025-12-15 15:38_

nit: We don't need to have an inner block for the assignment, it can be `let group = UploadDistribution`.

---

_@konstin reviewed on 2025-12-15 15:41_

Looks really good already!

We need one more addition for security: https://github.com/astral-sh/uv/issues/17126#issuecomment-3655931704

---

_@dijor0310 reviewed on 2025-12-15 22:18_

---

_Review comment by @dijor0310 on `crates/uv-publish/src/lib.rs`:136 on 2025-12-15 22:18_

You're right, fixed :)

---

_Review comment by @dijor0310 on `crates/uv-publish/src/lib.rs`:493 on 2025-12-15 22:18_

Done!

---

_@dijor0310 reviewed on 2025-12-15 22:18_

---

_@dijor0310 reviewed on 2025-12-15 22:18_

---

_Review comment by @dijor0310 on `crates/uv-publish/src/lib.rs`:515 on 2025-12-15 22:18_

Done!

---

_@dijor0310 reviewed on 2025-12-15 22:19_

---

_Review comment by @dijor0310 on `crates/uv-publish/src/lib.rs`:513 on 2025-12-15 22:19_

Changed, please take a look

---

_Review comment by @dijor0310 on `crates/uv-publish/src/lib.rs`:1769 on 2025-12-15 22:20_

Done!

---

_@dijor0310 reviewed on 2025-12-15 22:20_

---

_Comment by @dijor0310 on 2025-12-15 22:24_

@konstin, added a check that the new URL is in the same realm:
```rust
...
if Realm::from(&current_registry) == Realm::from(registry) {
    debug!("Redirecting the request to: {}", current_registry);
    n_past_redirections += 1;
    continue;
}
...
```

---

_Comment by @konstin on 2025-12-16 08:39_

Thanks for fixing!

I switched to explicit error types and added a test for infinite redirects.

---

_@konstin approved on 2025-12-16 08:40_

---

_Comment by @konstin on 2025-12-16 08:47_

I also added a test for the realm switch, since we consider that a security feature.

---

_Merged by @konstin on 2025-12-16 09:04_

---

_Closed by @konstin on 2025-12-16 09:04_

---

_Comment by @dijor0310 on 2025-12-16 10:18_

Thanks for merging, @konstin!

---
