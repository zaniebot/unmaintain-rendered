```yaml
number: 17105
title: Use the same retry logic across uv
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/better-retry-handling-5
created_at: 2025-12-12T17:44:33Z
updated_at: 2025-12-18T12:44:38Z
url: https://github.com/astral-sh/uv/pull/17105
synced_at: 2026-01-12T16:12:37Z
```

# Use the same retry logic across uv

---

_@konstin_

We were using slightly different retry code in multiple places, this PR unifies it.

Also fixes retry undercounting in publish if the retry middleware was involved.


---

_Marked ready for review by @konstin on 2025-12-15 14:00_

---

_Review requested from @EliteTK by @konstin on 2025-12-15 17:45_

---

_Label `internal` added by @konstin on 2025-12-16 09:54_

---

_Review comment by @EliteTK on `crates/uv-client/src/base_client.rs`:1142 on 2025-12-16 10:35_

nitpick: The type itself isn't deciding.

```suggestion
/// Per-request retry state and policy.
```

---

_Review comment by @EliteTK on `crates/uv-client/src/base_client.rs`:1159 on 2025-12-16 11:52_

Maybe it would be good to add a comment mentioning that new starts a timer? Otherwise someone might initialise this too early by accident.

---

_Review comment by @EliteTK on `crates/uv-client/src/base_client.rs`:1192 on 2025-12-16 13:23_

I think it looks odd for a function named `should_retry` to be async and to sleep.

Maybe the alternative is to return e.g. the metadata for how long the function expects to sleep for, and handling that externally? the debug! and URL could also then be done externally...

Not sure, I think I have a better overall idea for how this may be approached later but maybe this is fine for now.

---

_Review comment by @EliteTK on `crates/uv-client/src/base_client.rs`:1176 on 2025-12-16 14:04_

I still think this comment is confusing as the point at which we are "considering" the upstream retries is when we add them. I would just drop it.

```suggestion
```

---

_Review comment by @EliteTK on `crates/uv-bin-install/src/lib.rs`:262 on 2025-12-16 14:11_

You modified `attempts` in another place to be `retries` and removed the +1. Is this use of `attempts` still accurate? In the original code, `1` is subtracted from this value.

---

_@EliteTK approved on 2025-12-16 15:04_

I spent a bit more time on this because I wanted to understand the code better.

I think the approach of hiding the timing/debug stuff may be confusing. I think maybe this would be better served as a `get_retry_delay<E, P>(err: E, policy: &P, time, retries) -> Result<Duration, (E, u32)>` function or something, with the time tracking, waiting, and `debug!` being explicitly outside.

To demonstrate the above idea, and to understand if it would help, I tried having a function like:

```rust
pub async fn retry_with_policy<P, T, E, F>(
    retry_policy: &P,
    url: &Url,
    mut make_request: F,
) -> Result<T, (E, u32)>
where
    P: RetryPolicy,
    E: TriedRequestError,
    F: AsyncFnMut() -> Result<T, E>,
{
    let start_time = SystemTime::now();
    let mut total_retries = 0;

    loop {
        match make_request().await {
            Ok(ok) => return Ok(ok),
            Err(err) => {
                let delay = get_retry_delay(err, retry_policy, start_time, total_retries)?;
                debug!(
                    "Transient failure while handling response from {}; retrying after {:.1}s...",
                    DisplaySafeUrl::ref_cast(url),
                    delay.as_secs_f32(),
                );
                tokio::time::sleep(delay).await;
                total_retries += 1;
            }
        }
    }
}
```

Which you would use like (in `CachedClient::get_cacheable_with_retry`):

```rust
retry_with_policy(&self.uncached().retry_policy(), &req.url(), async || {
    let fresh_req = req.try_clone().expect("HTTP request must be cloneable");
    self.get_cacheable(fresh_req, cache_entry, cache_control, &response_callback)
        .await
})
.await
.map_err(|(err, retries)| err.with_retries(retries))
```

But I couldn't come up with a good approach which would work for `uv_publish::upload`. It does work for all the other cases though.

I guess one option would then be to just use the function in the plases where it works and use `get_retry_delay` maybe with some helper for waiting and `debug!`.

This would reduce the boilerplate even further, railroad users (developers) towards the happy path more, and avoid the possibility of starting the timer too early.

Just an idea though, may not be appropriate for this PR in particular.

---

_Comment by @konstin on 2025-12-16 16:15_

> I think the approach of hiding the timing/debug stuff may be confusing. I think maybe this would be better served as a `get_retry_delay<E, P>(err: E, policy: &P, time, retries) -> Result<Duration, (E, u32)>` function or something, with the time tracking, waiting, and `debug!` being explicitly outside.

The motivation for this change is that the implementations for this diverged slightly between the different code locations so I wanted to move them into a single location, otherwise I'd kept the logging and the waiting in the `loop` where it was before

---

_Review comment by @konstin on `crates/uv-bin-install/src/lib.rs`:262 on 2025-12-16 16:17_

Missed that this one does that too, fixed!

---

_@konstin reviewed on 2025-12-16 16:17_

---

_@konstin reviewed on 2025-12-16 16:25_

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:1159 on 2025-12-16 16:25_

Good idea, renamed to `start`

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:1192 on 2025-12-16 16:27_

I've renamed the function, the motivation is that I want to avoid the implementation from diverging. A secondary motivation that we have this boilerplate for each streaming request, and I'd like to keep that concise; Ideally it would be a part of the client, that doesn't work with e.g. our streaming hashing and unpacking:

```rust
loop {
    let result = fetch(...).await;
    match result {
        Ok(download_result) => return Ok(download_result),
        Err(err) => {
            if retry_state
                .handle_retry_and_backoff(&err, err.retries())
                .await
            {
                continue;
            }
            return if retry_state.total_retries() > 0 {
                Err(Error::NetworkErrorWithRetries {
                    err: Box::new(err),
                    retries: retry_state.total_retries(),
                })
            } else {
                Err(err)
            };
        }
    };
}
```

---

_@konstin reviewed on 2025-12-16 16:27_

---

_@EliteTK reviewed on 2025-12-16 16:35_

---

_Review comment by @EliteTK on `crates/uv-client/src/base_client.rs`:1192 on 2025-12-16 16:35_

Yeah I understand the motivation, the previous situation wasn't great, but that's why I suggested this potential boilerplate (which would work for most of the cases here):

```rust
return retry_with_policy(retry_policy, url, async || fetch(...).await)
    .await
    .map_err(|(err, retries)| {
        if retries > 0 {
            Error::NetworkErrorWithRetries { err: Box::new(err), retries }
        } else {
            err
        }
    })
```

But I agree that this code is better than the previous situation, it was just something to consider for the future.

---

_@konstin reviewed on 2025-12-17 11:34_

---

_Review comment by @konstin on `crates/uv-client/src/base_client.rs`:1159 on 2025-12-17 11:34_

FWIW the timer is currently unused in our strategy, which only looks at the count altogether.

---

_@EliteTK approved on 2025-12-18 10:08_

I prefer this split up "should_retry" and "sleep_backoff" approach more than the previous combined function.

The names much better represent what the functions are doing.

There are a few things I would do differently but at this point they're just stylistic nitpicks/different opinions.

---

_Merged by @konstin on 2025-12-18 12:44_

---

_Closed by @konstin on 2025-12-18 12:44_

---

_Branch deleted on 2025-12-18 12:44_

---
