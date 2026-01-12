```yaml
number: 15675
title: Extract IO errors from h2 for streaming retries of Connection Reset
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - network
assignees: []
merged: true
base: main
head: konsti/h2ohwherearemyerrors
created_at: 2025-09-04T11:26:21Z
updated_at: 2025-09-04T12:45:02Z
url: https://github.com/astral-sh/uv/pull/15675
synced_at: 2026-01-12T16:11:53Z
```

# Extract IO errors from h2 for streaming retries of Connection Reset

---

_@konstin_

Our streaming retries were missing connection reset errors as h2 was shadowing IO errors (https://github.com/hyperium/h2/issues/862).

**Test plan**

In one terminal:

```
cargo python uninstall 3.12 && cargo run python install 3.12 -vv
```

In another:

```
sudo tcpkill -i wlp2s0 port 443
```

Output:

```
error: Failed to install cpython-3.12.11-linux-x86_64-gnu
  Caused by: Request failed after 3 retries
  Caused by: Failed to download https://github.com/astral-sh/python-build-standalone/releases/download/20250902/cpython-3.12.11%2B20250902-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
  Caused by: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250902/cpython-3.12.11%2B20250902-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz)
  Caused by: client error (SendRequest)
  Caused by: connection error
  Caused by: connection reset
```

I don't know how to test that from inside Rust.

Fix #14171 (again, hopefully)

---

_Review requested from @zanieb by @konstin on 2025-09-04 11:26_

---

_Label `bug` added by @konstin on 2025-09-04 11:26_

---

_Label `network` added by @konstin on 2025-09-04 11:26_

---

_@zanieb approved on 2025-09-04 12:22_

😭 

---

_Merged by @konstin on 2025-09-04 12:45_

---

_Closed by @konstin on 2025-09-04 12:45_

---

_Branch deleted on 2025-09-04 12:45_

---
