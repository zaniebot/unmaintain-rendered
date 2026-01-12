```yaml
number: 1780
title: "Support setting request timeout with `UV_HTTP_TIMEOUT` and `HTTP_TIMEOUT`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/tweak-timeout-vars
created_at: 2024-02-20T19:51:55Z
updated_at: 2024-02-21T00:48:19Z
url: https://github.com/astral-sh/uv/pull/1780
synced_at: 2026-01-12T16:04:43Z
```

# Support setting request timeout with `UV_HTTP_TIMEOUT` and `HTTP_TIMEOUT`

---

_@zanieb_

Follow-up to #1694 matching Cargo's environment variable names

https://doc.rust-lang.org/nightly/cargo/reference/config.html#httptimeout

---

_Label `enhancement` added by @zanieb on 2024-02-20 19:51_

---

_@charliermarsh approved on 2024-02-20 20:22_

---

_@charliermarsh reviewed on 2024-02-20 20:23_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:94 on 2024-02-20 20:23_

Should `UV_REQUEST_TIMEOUT` be last? I can't tell.

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:94 on 2024-02-20 20:36_

Probably before `HTTP_REQUEST` but after `UV_HTTP_TIMEOUT`? Unclear. Wish I could drop support for it haha.

---

_@zanieb reviewed on 2024-02-20 20:36_

---

_Marked ready for review by @zanieb on 2024-02-21 00:17_

---

_Merged by @zanieb on 2024-02-21 00:48_

---

_Closed by @zanieb on 2024-02-21 00:48_

---

_Branch deleted on 2024-02-21 00:48_

---
