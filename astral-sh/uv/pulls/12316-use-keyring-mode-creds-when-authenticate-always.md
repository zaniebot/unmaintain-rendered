```yaml
number: 12316
title: "Use `keyring --mode creds` when `authenticate = \"always\"`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/keyring-always
created_at: 2025-03-19T16:46:44Z
updated_at: 2025-03-19T21:30:34Z
url: https://github.com/astral-sh/uv/pull/12316
synced_at: 2026-01-10T11:10:39Z
```

# Use `keyring --mode creds` when `authenticate = "always"`

---

_Pull request opened by @zanieb on 2025-03-19 16:46_

Previously, we required a username to perform a fetch from the keyring because the `keyring` CLI only supported fetching password for a given service and username. Unfortunately, this is different from the keyring Python API which supported fetching a username _and_ password for a given service. We can't (easily) use the Python API because we don't expect `keyring` to be installed in a specific environment during network requests. This means that we did not have parity with `pip`.

Way back in https://github.com/jaraco/keyring/pull/678 we got a `--mode creds` flag added to `keyring`'s CLI which supports parity with the Python API. Since `keyring` is expensive to invoke and we cannot be certain that users are on the latest version of keyring, we've not added support for invoking keyring with this flag. However, now that we have a mode that says authentication is _required_ for an index (#11896), we might as well _try_ to invoke keyring with `--mode creds` when there is no username. This will address use-cases where the username is non-constant and move us closer to `pip` parity.

---

_Label `enhancement` added by @zanieb on 2025-03-19 16:46_

---

_Comment by @zanieb on 2025-03-19 17:00_

There's not a good place to put this in the documentation yet. I'll follow up with some changes?

---

_@zanieb reviewed on 2025-03-19 17:10_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:1046 on 2025-03-19 17:10_

This error added in https://github.com/astral-sh/uv/pull/12313

It'd be nice to match on a specific type here.

---

_Review requested from @konstin by @zanieb on 2025-03-19 17:10_

---

_Review requested from @jtfmumm by @zanieb on 2025-03-19 17:10_

---

_@jtfmumm reviewed on 2025-03-19 18:57_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/middleware.rs`:1046 on 2025-03-19 18:57_

The problem is that this is in the context of the `reqwest_middleware::Middleware` trait. So `handle()` needs to return a `reqwest_middleware::Result<Response>`.

---

_@zanieb reviewed on 2025-03-19 18:58_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:1046 on 2025-03-19 18:58_

Ah. It might not be worth it then. Thanks for looking anyway.

---

_@jtfmumm reviewed on 2025-03-19 19:06_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/keyring.rs`:143 on 2025-03-19 19:06_

You could also do:

```
let username = lines.next()?;
let password = lines.next()?;
Some((username.to_string(), password.to_string()))
```

---

_@jtfmumm reviewed on 2025-03-19 19:11_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/keyring.rs`:174 on 2025-03-19 19:11_

```suggestion
            if service == service_name && username.map_or(true, |username| username == *user)
```

---

_@zanieb reviewed on 2025-03-19 19:14_

---

_Review comment by @zanieb on `crates/uv-auth/src/keyring.rs`:143 on 2025-03-19 19:14_

Ah that seems better 🤦‍♀️ 

---

_@zanieb reviewed on 2025-03-19 19:15_

---

_Review comment by @zanieb on `crates/uv-auth/src/keyring.rs`:174 on 2025-03-19 19:15_

I don't really find `map_or` easier to reason about personally, but happy to change it.

---

_@zanieb reviewed on 2025-03-19 19:16_

---

_Review comment by @zanieb on `crates/uv-auth/src/keyring.rs`:143 on 2025-03-19 19:16_

A note on this section, we should probably log if we find a username but no password. This implementation makes that even easier.

---

_Review comment by @jtfmumm on `crates/uv-auth/src/keyring.rs`:143 on 2025-03-19 19:19_

Good point

---

_@jtfmumm reviewed on 2025-03-19 19:19_

---

_@jtfmumm reviewed on 2025-03-19 19:23_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/keyring.rs`:174 on 2025-03-19 19:23_

Up to you. In this case, it signals to me we're returning a `bool` with a default for `None` before reading the closure.

---

_@jtfmumm reviewed on 2025-03-19 19:26_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/keyring.rs`:174 on 2025-03-19 19:26_

I really just prefer it because it's shorter though

---

_@jtfmumm approved on 2025-03-19 19:28_

---

_@zanieb reviewed on 2025-03-19 19:35_

---

_Review comment by @zanieb on `crates/uv-auth/src/keyring.rs`:174 on 2025-03-19 19:35_

Clippy actually wants `is_none_or` which is even better

---

_@jtfmumm reviewed on 2025-03-19 20:20_

---

_Review comment by @jtfmumm on `crates/uv-auth/src/keyring.rs`:174 on 2025-03-19 20:20_

Definitely better

---

_Merged by @zanieb on 2025-03-19 21:30_

---

_Closed by @zanieb on 2025-03-19 21:30_

---

_Branch deleted on 2025-03-19 21:30_

---
