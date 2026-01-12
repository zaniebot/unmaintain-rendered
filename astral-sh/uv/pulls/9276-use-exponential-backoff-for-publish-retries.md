```yaml
number: 9276
title: Use exponential backoff for publish retries
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/pub
created_at: 2024-11-20T14:18:52Z
updated_at: 2024-11-20T15:02:34Z
url: https://github.com/astral-sh/uv/pull/9276
synced_at: 2026-01-12T16:08:44Z
```

# Use exponential backoff for publish retries

---

_@charliermarsh_

## Summary

Just trying to unify the retry handling, as in https://github.com/astral-sh/uv/pull/9274 and elsewhere. Right now, the publish handler doesn't use any backoff and always retries three times regardless of settings.


---

_Review requested from @konstin by @charliermarsh on 2024-11-20 14:18_

---

_Label `enhancement` added by @charliermarsh on 2024-11-20 14:18_

---

_@charliermarsh reviewed on 2024-11-20 14:19_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/lib.rs`:389 on 2024-11-20 14:19_

This now uses `is_extended_transient_error` instead of `UvRetryableStrategy.handle(&result) == Some(Retryable::Transient)`. Wouldn't the latter already be called in the middle itself...?

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:410 on 2024-11-20 14:21_

I think these should go on `upload_with_retry`

---

_@konstin approved on 2024-11-20 14:21_

Thanks!

---

_@charliermarsh reviewed on 2024-11-20 14:22_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/lib.rs`:389 on 2024-11-20 14:22_

@konstin -- just confirming, do you agree that this is ok?

---

_@charliermarsh reviewed on 2024-11-20 14:23_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/lib.rs`:389 on 2024-11-20 14:23_

It will miss the "default" cases handled by the retry strategy, like other transient request errors.

---

_@zanieb reviewed on 2024-11-20 14:31_

---

_Review comment by @zanieb on `crates/uv-publish/src/lib.rs`:389 on 2024-11-20 14:31_

I think that's okay... presuming that the default strategy doesn't cover anything that would occur at response read time.

---

_@konstin reviewed on 2024-11-20 14:43_

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:389 on 2024-11-20 14:43_

Thanks for the ping, i didn't realize we were missing that layer now. I don't have any reference upload intermittent errors, but I think we should preserve the `UvRetryableStrategy`, i can see things like timeouts and cancelled responses happening here too.

---

_@charliermarsh reviewed on 2024-11-20 14:45_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/lib.rs`:389 on 2024-11-20 14:45_

I'm a little way due to this comment: "Implements a custom retry flow since the request isn't cloneable". Does that mean the default middleware doesn't work here or something?

---

_Review comment by @charliermarsh on `crates/uv-publish/src/lib.rs`:389 on 2024-11-20 14:46_

Ok.

---

_@charliermarsh reviewed on 2024-11-20 14:46_

---

_@konstin reviewed on 2024-11-20 14:53_

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:389 on 2024-11-20 14:53_

So that we're on the same page: The retry middleware clones the request, and then resends it if it failed (https://github.com/TrueLayer/reqwest-middleware/blob/8a494c165734e24c62823714843e1c9347027e8a/reqwest-retry/src/middleware.rs#L150-L155). But a POST request with formdata and a streaming file isn't cloneable (limitation of reqwest, https://github.com/seanmonstar/reqwest/issues/2416). That's why the upload client is building a custom base client with `.retries(0)` that never internally tries to clone the request and instead has a loop that always rebuilds the request.

---

_@charliermarsh reviewed on 2024-11-20 14:53_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/lib.rs`:389 on 2024-11-20 14:53_

Given that, it ended up being much simpler to leave it all in a single method.

---

_@charliermarsh reviewed on 2024-11-20 14:58_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/lib.rs`:389 on 2024-11-20 14:58_

Ohh ok, I missed the `.retries(0)`.

---

_Merged by @charliermarsh on 2024-11-20 15:02_

---

_Closed by @charliermarsh on 2024-11-20 15:02_

---

_Branch deleted on 2024-11-20 15:02_

---
