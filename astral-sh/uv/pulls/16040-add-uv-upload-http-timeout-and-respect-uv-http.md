```yaml
number: 16040
title: "Add `UV_UPLOAD_HTTP_TIMEOUT` and respect `UV_HTTP_TIMEOUT` in uploads"
type: pull_request
state: merged
author: andrey-berenda
labels:
  - configuration
assignees: []
merged: true
base: main
head: move-http-timeout-to-EnvironmentOptions
created_at: 2025-09-26T13:10:48Z
updated_at: 2025-10-09T17:28:30Z
url: https://github.com/astral-sh/uv/pull/16040
synced_at: 2026-01-12T16:12:04Z
```

# Add `UV_UPLOAD_HTTP_TIMEOUT` and respect `UV_HTTP_TIMEOUT` in uploads

---

_@andrey-berenda_

## Summary
- Move  parsing `UV_HTTP_TIMEOUT`, `UV_REQUEST_TIMEOUT` and `HTTP_TIMEOUT` to `EnvironmentOptions`
- Add new env varialbe `UV_UPLOAD_HTTP_TIMEOUT`

Relates https://github.com/astral-sh/uv/issues/14720

## Test Plan

Tests with existing tests

---

_Review comment by @andrey-berenda on `crates/uv/src/commands/auth/token.rs`:34 on 2025-09-26 13:12_

We haven't used preview here, but right now I started to use it

---

_Review comment by @andrey-berenda on `crates/uv/src/commands/auth/login.rs`:45 on 2025-09-26 13:12_

We haven't used preview here, but right now I started to use it


---

_@andrey-berenda reviewed on 2025-09-26 13:12_

---

_Marked ready for review by @andrey-berenda on 2025-09-26 13:37_

---

_@zanieb reviewed on 2025-09-29 13:33_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:691 on 2025-09-29 13:33_

Doesn't this return a https://doc.rust-lang.org/std/num/struct.ParseIntError.html we should use as the error message not a value?

---

_@zanieb reviewed on 2025-09-29 13:35_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:687 on 2025-09-29 13:35_

I would probably not nest these matches, I'd do an early return if the environment variable is not present or not unicode.

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:706 on 2025-09-29 13:36_

I think as written, this will return an error on an empty string instead of `None`?

---

_@zanieb reviewed on 2025-09-29 13:36_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:312 on 2025-09-29 14:02_

We should probably remove `self.default_timeout` entirely and unwrap to that during settings resolution.

---

_@zanieb reviewed on 2025-09-29 14:02_

---

_@zanieb reviewed on 2025-09-29 14:02_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:3 on 2025-09-29 14:02_

This import should be in the group above.

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/logout.rs`:104 on 2025-09-29 14:04_

We should be using the user-provided `Preview` value here

---

_@zanieb reviewed on 2025-09-29 14:04_

---

_@andrey-berenda reviewed on 2025-09-29 14:15_

---

_Review comment by @andrey-berenda on `crates/uv-settings/src/lib.rs`:691 on 2025-09-29 14:15_

yes, fixed

---

_@andrey-berenda reviewed on 2025-09-29 14:16_

---

_Review comment by @andrey-berenda on `crates/uv-settings/src/lib.rs`:687 on 2025-09-29 14:16_

refactored

---

_Review comment by @andrey-berenda on `crates/uv/src/commands/auth/logout.rs`:104 on 2025-09-29 14:24_

added

---

_@andrey-berenda reviewed on 2025-09-29 14:24_

---

_@andrey-berenda reviewed on 2025-09-29 14:32_

---

_Review comment by @andrey-berenda on `crates/uv-client/src/base_client.rs`:312 on 2025-09-29 14:32_

I did not do this because we use 15min default for upload_client
is it ok to override timeout at all at upload_client? 

```
        // Set a very high timeout for uploads, connections are often 10x slower on upload than
        // download. 15 min is taken from the time a trusted publishing token is valid.
```

---

_@andrey-berenda reviewed on 2025-09-29 14:35_

---

_Review comment by @andrey-berenda on `crates/uv-client/src/base_client.rs`:312 on 2025-09-29 14:35_

I deleted `default_timeout` 
but it will be quite easy to revert it if it is better to have default timeout

---

_@andrey-berenda reviewed on 2025-09-29 14:37_

---

_Review comment by @andrey-berenda on `crates/uv-settings/src/lib.rs`:3 on 2025-09-29 14:37_

I think we cannot group path and time

---

_Review requested from @zanieb by @andrey-berenda on 2025-09-29 15:11_

---

_@andrey-berenda reviewed on 2025-09-29 22:28_

---

_Review comment by @andrey-berenda on `crates/uv-settings/src/lib.rs`:706 on 2025-09-29 22:28_

Fixed 
And right now return None
I have thought that it is better to return None only for String

---

_@andrey-berenda reviewed on 2025-09-29 22:29_

---

_Review comment by @andrey-berenda on `crates/uv-settings/src/lib.rs`:691 on 2025-09-29 22:29_

Added test for this

---

_@zanieb reviewed on 2025-09-30 17:56_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:3 on 2025-09-30 17:56_

There should be whitespace between the std imports and the uv imports

---

_@andrey-berenda reviewed on 2025-09-30 18:36_

---

_Review comment by @andrey-berenda on `crates/uv-settings/src/lib.rs`:3 on 2025-09-30 18:36_

Added

---

_Renamed from "Move parsing http-timeout to `EnvironmentOptions`" to "Add `UV_UPLOAD_HTTP_TIMEOUT` and respect `UV_HTTP_TIMEOUT` in uploads" by @zanieb on 2025-10-09 17:27_

---

_Label `configuration` added by @zanieb on 2025-10-09 17:27_

---

_Merged by @zanieb on 2025-10-09 17:28_

---

_Closed by @zanieb on 2025-10-09 17:28_

---
