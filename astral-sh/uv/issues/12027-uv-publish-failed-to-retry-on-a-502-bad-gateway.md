---
number: 12027
title: "`uv publish` failed to retry on a 502 bad gateway error"
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-03-06T22:52:21Z
updated_at: 2025-03-10T11:38:09Z
url: https://github.com/astral-sh/uv/issues/12027
synced_at: 2026-01-10T01:25:14Z
---

# `uv publish` failed to retry on a 502 bad gateway error

---

_Issue opened by @zanieb on 2025-03-06 22:52_

See https://github.com/astral-sh/uv/actions/runs/13707864294/job/38338658887

I'm not sure why this is the case? We special case retries during publish though, so I would not be surprised if we were handling something incorrectly?

https://github.com/astral-sh/uv/blob/4611690745c2358aaec407bcd620ac1255520255/crates/uv-publish/src/lib.rs#L398-L411

5XX is supposedly handled in the `DefaultRetryableStrategy` from `reqwest-retry` 

```rust
/// Default request success retry strategy.
///
/// Will only retry if:
/// * The status was 5XX (server error)
/// * The status was 408 (request timeout) or 429 (too many requests)
///
/// Note that success here means that the request finished without interruption, not that it was logically OK.
pub fn default_on_request_success(success: &reqwest::Response) -> Option<Retryable> {
    let status = success.status();
    if status.is_server_error() {
        Some(Retryable::Transient)
    } else if status.is_client_error()
        && status != StatusCode::REQUEST_TIMEOUT
        && status != StatusCode::TOO_MANY_REQUESTS
    {
        Some(Retryable::Fatal)
    } else if status.is_success() {
        None
    } else if status == StatusCode::REQUEST_TIMEOUT || status == StatusCode::TOO_MANY_REQUESTS {
        Some(Retryable::Transient)
    } else {
        Some(Retryable::Fatal)
    }
}
```

and

```rust
    /// Check if status is within 500-599.
    #[inline]
    pub fn is_server_error(&self) -> bool {
        600 > self.0.get() && self.0.get() >= 500
    }
```

---

_Label `bug` added by @zanieb on 2025-03-06 22:52_

---

_Comment by @zanieb on 2025-03-06 23:17_

This log is present

https://github.com/astral-sh/uv/blob/748582ee6f3ff1007692f131fa4b59d7c100ec66/crates/uv-client/src/base_client.rs#L441

---

_Comment by @zanieb on 2025-03-06 23:22_

But it looks like we set the retries to zero?

https://github.com/astral-sh/uv/blob/3b83b48fd27fc224c5fd534f88a2c0fb28ffdd06/crates/uv/src/commands/publish.rs#L57

which is propagated at

https://github.com/astral-sh/uv/blob/4611690745c2358aaec407bcd620ac1255520255/crates/uv-publish/src/lib.rs#L382

https://github.com/astral-sh/uv/blob/748582ee6f3ff1007692f131fa4b59d7c100ec66/crates/uv-client/src/base_client.rs#L414-L417

and checked at 

https://github.com/astral-sh/uv/blob/4611690745c2358aaec407bcd620ac1255520255/crates/uv-publish/src/lib.rs#L400

```rust
impl RetryPolicy for ExponentialBackoff {
    fn should_retry(&self, _request_start_time: SystemTime, n_past_retries: u32) -> RetryDecision {
        if self.too_many_attempts(n_past_retries) {
            RetryDecision::DoNotRetry
        } else {
            ...
```

```rust
    fn too_many_attempts(&self, n_past_retries: u32) -> bool {
        self.max_n_retries
            .is_some_and(|max_n| max_n <= n_past_retries)
    }
```

---

_Comment by @zanieb on 2025-03-06 23:28_

Possible this regressed in

- https://github.com/astral-sh/uv/pull/9213
- https://github.com/astral-sh/uv/pull/9276



---

_Comment by @charliermarsh on 2025-03-07 01:31_

It might be as simple as removing `.retries(0)`, but I don't fully understand the comment above it. @konstin can you take this one please?

---

_Assigned to @konstin by @charliermarsh on 2025-03-07 01:31_

---

_Comment by @konstin on 2025-03-07 12:55_

We need to keep the `retries(0)` as the auth middleware is incompatible with upload bodies (https://github.com/seanmonstar/reqwest/issues/2416), we need to construct the retry policy ourselves.

---

_Referenced in [astral-sh/uv#12041](../../astral-sh/uv/pulls/12041.md) on 2025-03-07 12:56_

---

_Closed by @konstin on 2025-03-10 11:38_

---

_Closed by @konstin on 2025-03-10 11:38_

---
