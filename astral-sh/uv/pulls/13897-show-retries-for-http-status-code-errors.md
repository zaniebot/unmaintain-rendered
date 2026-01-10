```yaml
number: 13897
title: Show retries for HTTP status code errors
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - error messages
assignees: []
merged: true
base: main
head: konsti/fix-retry-reporting
created_at: 2025-06-07T10:53:02Z
updated_at: 2025-06-16T10:14:01Z
url: https://github.com/astral-sh/uv/pull/13897
synced_at: 2026-01-10T11:10:42Z
```

# Show retries for HTTP status code errors

---

_Pull request opened by @konstin on 2025-06-07 10:53_

Using a companion change in the middleware (https://github.com/TrueLayer/reqwest-middleware/pull/235, forked&tagged pending review), we can check and show retries for HTTP status core errors, to consistently report retries again.

We fix two cases:
* Show retries for status code errors for cache client requests
* Show retries for status code errors for Python download requests

Not handled:
* Show previous retries when a distribution download fails mid-streaming
* Perform retries when a distribution download fails mid-streaming
* Show previous retries when a Python download fails mid-streaming
* Perform retries when a Python download fails mid-streaming

---

_Review requested from @jtfmumm by @konstin on 2025-06-07 10:53_

---

_Label `error messages` added by @konstin on 2025-06-16 08:26_

---

_Label `enhancement` added by @konstin on 2025-06-16 08:26_

---

_Marked ready for review by @konstin on 2025-06-16 08:28_

---

_Review comment by @jtfmumm on `crates/uv-client/src/cached_client.rs`:123 on 2025-06-16 09:16_

Can this `impl` line be removed now?

---

_Review comment by @jtfmumm on `crates/uv-client/src/cached_client.rs`:649 on 2025-06-16 09:23_

This same logic appears below. Would it be worth breaking this match out into a `CachedClientError` method?

---

_Review comment by @jtfmumm on `crates/uv-client/src/registry_client.rs`:618 on 2025-06-16 09:25_

Can you use the `error` method?

---

_Review comment by @jtfmumm on `crates/uv-distribution/src/distribution_database.rs`:815 on 2025-06-16 09:27_

Is there a reason you use `err: source` here and in the following but not in the cases above and below?

---

_@jtfmumm reviewed on 2025-06-16 09:28_

---

_Review comment by @konstin on `crates/uv-client/src/cached_client.rs`:123 on 2025-06-16 09:44_

It's used as the non-consuming conversion `is_extended_transient_error`, while the `From`s consume `Self`. Could be a deref or a `From` with `&Self` too, not sure what the most intuitive here is.

---

_@konstin reviewed on 2025-06-16 09:44_

---

_@jtfmumm reviewed on 2025-06-16 09:51_

---

_Review comment by @jtfmumm on `crates/uv-client/src/cached_client.rs`:123 on 2025-06-16 09:51_

What I meant was that it looks like `retries()` and `error()` can be under one instance of that line:

```rust
impl<CallbackError: std::error::Error + 'static> CachedClientError<CallbackError> {
    fn retries(&self) -> Option<u32> {
        match self {
            CachedClientError::Client { retries, .. } => *retries,
            CachedClientError::Callback { retries, .. } => *retries,
        }
    }

    fn error(&self) -> &dyn std::error::Error {
        match self {
            CachedClientError::Client { err, .. } => err,
            CachedClientError::Callback { err, .. } => err,
        }
    }
}
```

---

_Review comment by @konstin on `crates/uv-distribution/src/distribution_database.rs`:815 on 2025-06-16 09:52_

Good catch

---

_@konstin reviewed on 2025-06-16 09:52_

---

_Review comment by @konstin on `crates/uv-client/src/cached_client.rs`:123 on 2025-06-16 09:53_

oh, yes sure that's better!

---

_@konstin reviewed on 2025-06-16 09:53_

---

_@jtfmumm approved on 2025-06-16 10:05_

---

_Merged by @konstin on 2025-06-16 10:14_

---

_Closed by @konstin on 2025-06-16 10:14_

---

_Branch deleted on 2025-06-16 10:14_

---
