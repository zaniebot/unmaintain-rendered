```yaml
number: 2947
title: URL decode passwords for storage
type: pull_request
state: closed
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: main
head: zb/decode-password
created_at: 2024-04-09T22:44:07Z
updated_at: 2024-04-15T20:43:01Z
url: https://github.com/astral-sh/uv/pull/2947
synced_at: 2026-01-12T16:05:19Z
```

# URL decode passwords for storage

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/2822
Same idea as https://github.com/astral-sh/uv/pull/2592

Needs a test plan still.

---

_Label `bug` added by @zanieb on 2024-04-09 22:44_

---

_@zanieb reviewed on 2024-04-09 22:48_

---

_Review comment by @zanieb on `crates/uv-auth/src/store.rs`:110 on 2024-04-09 22:48_

`url.password()` is explicitly a percent-encoded string so this makes a lot of sense to me.

---

_Comment by @zanieb on 2024-04-09 22:52_

I think the problem is roughly that we use `set_username` and `set_password` to apply the credentials

https://github.com/astral-sh/uv/blob/43ded54bd3f53d6ce9bab815cf2098a1814885af/crates/uv-auth/src/store.rs#L47-L55

but those percent-encode the credentials which are already be percent-encoded since we pulled from from `url.username()` and `url.password()` which do not perform decoding.

I want to make some changes to the documentation and structs here to clarify this.

---

_@charliermarsh approved on 2024-04-09 23:05_

---

_Comment by @zanieb on 2024-04-15 20:43_

Implemented in https://github.com/astral-sh/uv/pull/2976

---

_Closed by @zanieb on 2024-04-15 20:43_

---
