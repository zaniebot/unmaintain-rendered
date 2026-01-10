```yaml
number: 2325
title: Avoid duplicating authorization header with netrc
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/req
created_at: 2024-03-10T04:16:51Z
updated_at: 2024-03-10T15:03:04Z
url: https://github.com/astral-sh/uv/pull/2325
synced_at: 2026-01-10T14:54:43Z
```

# Avoid duplicating authorization header with netrc

---

_Pull request opened by @charliermarsh on 2024-03-10 04:16_

## Summary

The netrc middleware we added in https://github.com/astral-sh/uv/pull/2241 has a slight problem. If you include credentials in your index URL, _and_ in the netrc file, the crate blindly adds the netrc credentials as a header. And given the `ReqwestBuilder` API, this means you end up with _two_ `Authorization` headers, which always leads to an invalid request, though the exact failure can take different forms.

This PR removes the middleware crate in favor of our own middleware. Instead of using the `RequestInitialiser` API, we have to use the `Middleware` API, so that we can remove the header on the request itself.

Closes https://github.com/astral-sh/uv/issues/2323.

## Test Plan

- Verified that running against a private index with credentials in the URL (but no netrc file) worked without error.
- Verified that running against a private index with credentials in the netrc file (but not the URL) worked without error.
- Verified that running against a private index with a mix of credentials in  both _also_ worked without error.


---

_Review requested from @zanieb by @charliermarsh on 2024-03-10 04:17_

---

_Label `bug` added by @charliermarsh on 2024-03-10 04:17_

---

_Comment by @zanieb on 2024-03-10 14:50_

This is going to conflict with #2254 

cc @BakerNet okay if we land this then you can rebase on top of it (or merge, whatever makes you comfortable)?

---

_Comment by @charliermarsh on 2024-03-10 14:52_

I think we should merge and ship this in the soonest release, since it was a regression.


---

_@zanieb reviewed on 2024-03-10 14:54_

---

_Review comment by @zanieb on `crates/uv-client/src/middleware.rs`:79 on 2024-03-10 14:54_

Does this mean for a URL with explicit auth attached we will replace it with what's in the netrc file? That seems wrong.

---

_@charliermarsh reviewed on 2024-03-10 14:54_

---

_Review comment by @charliermarsh on `crates/uv-client/src/middleware.rs`:79 on 2024-03-10 14:54_

Yes. I believe this is correct actually.

---

_@charliermarsh reviewed on 2024-03-10 14:54_

---

_Review comment by @charliermarsh on `crates/uv-client/src/middleware.rs`:79 on 2024-03-10 14:54_

`pip` changed this in https://github.com/pypa/pip/pull/10998. It was then reverted in https://github.com/pypa/pip/pull/11134.

---

_@zanieb reviewed on 2024-03-10 14:55_

---

_Review comment by @zanieb on `crates/uv-client/src/middleware.rs`:79 on 2024-03-10 14:55_

Why? Wouldn't it be very confusing if you passed authentication via the CLI and we overwrote it with something from a configuration file?

---

_@charliermarsh reviewed on 2024-03-10 14:56_

---

_Review comment by @charliermarsh on `crates/uv-client/src/middleware.rs`:79 on 2024-03-10 14:56_

Or maybe not? I honestly can't tell: https://github.com/pypa/pip/blob/a33caa26f2bf525eafb4ec004f9c1bd18d238a31/src/pip/_internal/network/auth.py#L347. Anyway, I can switch the precedence if you prefer. Doesn't matter to me.

---

_Review comment by @zanieb on `crates/uv-client/src/middleware.rs`:79 on 2024-03-10 14:57_

pip's behavior is more complicated because they only attach auth after seeing a 401 — it looks like they changed that for netrc? I'm not sure I didn't give it a close read yet.

I think it's important to respect _directly_ passed credentials over the persistent configuration.

---

_@zanieb reviewed on 2024-03-10 14:57_

---

_@charliermarsh reviewed on 2024-03-10 14:58_

---

_Review comment by @charliermarsh on `crates/uv-client/src/middleware.rs`:79 on 2024-03-10 14:58_

I changed it.

---

_@zanieb reviewed on 2024-03-10 14:58_

---

_Review comment by @zanieb on `crates/uv-client/src/middleware.rs`:79 on 2024-03-10 14:58_

I wish there was more context on that revert pull request...

---

_Review comment by @zanieb on `crates/uv-client/src/middleware.rs`:79 on 2024-03-10 14:58_

but the issue it closes is https://github.com/pypa/pip/issues/11113 which doesn't affect us

---

_@zanieb reviewed on 2024-03-10 14:58_

---

_Comment by @charliermarsh on 2024-03-10 14:58_

We should be really careful in that PR that (1) we have clear order of precedence for credentials, and (2) we never introduce duplicate credentials.

---

_Comment by @zanieb on 2024-03-10 14:59_

I think introducing a dedicated authentication store abstraction is intended to solve those problems

---

_@zanieb approved on 2024-03-10 14:59_

---

_@charliermarsh reviewed on 2024-03-10 15:01_

---

_Review comment by @charliermarsh on `crates/uv-client/src/middleware.rs`:79 on 2024-03-10 15:01_

Yeah it seems like pip's _current_ intent is to prioritize the URL over the netrc. This does make sense to me, I was just trying to follow pip and misread the chain of reverts. Thanks!

---

_Merged by @charliermarsh on 2024-03-10 15:02_

---

_Closed by @charliermarsh on 2024-03-10 15:02_

---

_Branch deleted on 2024-03-10 15:02_

---

_Comment by @charliermarsh on 2024-03-10 15:03_

Sounds good. Feel free to ping me there when the time is right if you want my input.

---
