```yaml
number: 8100
title: Add support for managed installs of free-threaded Python
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/freethreaded-install
created_at: 2024-10-10T16:44:24Z
updated_at: 2024-10-15T08:55:03Z
url: https://github.com/astral-sh/uv/pull/8100
synced_at: 2026-01-10T12:54:02Z
```

# Add support for managed installs of free-threaded Python

---

_Pull request opened by @zanieb on 2024-10-10 16:44_

Closes https://github.com/astral-sh/uv/issues/7193

```

❯ cargo run -q -- python uninstall 3.13t
Searching for Python versions matching: Python 3.13t
Uninstalled Python 3.13.0 in 231ms
 - cpython-3.13.0+freethreaded-macos-aarch64-none
❯ cargo run -q -- python install 3.13t
Searching for Python versions matching: Python 3.13t
Installed Python 3.13.0 in 3.54s
 + cpython-3.13.0+freethreaded-macos-aarch64-none
❯ cargo run -q -- python install 3.12t
Searching for Python versions matching: Python 3.12t
error: No download found for request: cpython-3.12t-macos-aarch64-none
❯ cargo run -q -- python install 3.13rc3t
Searching for Python versions matching: Python 3.13rc3t
Found existing installation for Python 3.13rc3t: cpython-3.13.0+freethreaded-macos-aarch64-none
❯ cargo run -q -- run -p 3.13t python -c "import sys; print(sys.base_prefix)"
/Users/zb/.local/share/uv/python/cpython-3.13.0+freethreaded-macos-aarch64-none
```

---

_Label `enhancement` added by @zanieb on 2024-10-10 16:44_

---

_@zanieb reviewed on 2024-10-10 16:45_

---

_Review comment by @zanieb on `crates/uv-python/fetch-download-metadata.py`:111 on 2024-10-10 16:45_

In preparation for supporting download of debug builds too — I don't add that in the Rust side here though.

---

_Marked ready for review by @zanieb on 2024-10-10 16:59_

---

_@zanieb reviewed on 2024-10-10 17:04_

---

_Review comment by @zanieb on `crates/uv-python/fetch-download-metadata.py`:127 on 2024-10-10 17:04_

This `+variant` syntax is a little arbitrary — but I don't want to use `-` because I want the number of components to be constant. The alternative is using the short-form like `t` and `d` after the version e.g. `3.13.0t`, but I think this is clearer?

---

_@zanieb reviewed on 2024-10-10 17:44_

---

_Review comment by @zanieb on `crates/uv-python/fetch-download-metadata.py`:127 on 2024-10-10 17:44_

This sort of steals the "local version" slot which could be problematic? Those aren't supported for Python versions though.

---

_Review comment by @j178 on `crates/uv-python/fetch-download-metadata.py`:123 on 2024-10-10 20:21_

Can we add `variant` to the `sort_key` in `render`?

https://github.com/astral-sh/uv/blob/12a76690b2c39220806af5e992320e8e7a8e8967/crates/uv-python/fetch-download-metadata.py#L460

---

_@charliermarsh approved on 2024-10-10 23:49_

Sweet.

---

_@j178 reviewed on 2024-10-11 01:02_

---

_Review comment by @j178 on `crates/uv-python/fetch-download-metadata.py`:154 on 2024-10-11 03:40_

This list seems not being used?

---

_@j178 reviewed on 2024-10-11 03:58_

---

_@zanieb reviewed on 2024-10-12 14:50_

---

_Review comment by @zanieb on `crates/uv-python/fetch-download-metadata.py`:154 on 2024-10-12 14:50_

Yeah it's not used, we can drop it.

---

_Merged by @zanieb on 2024-10-14 20:18_

---

_Closed by @zanieb on 2024-10-14 20:18_

---

_Branch deleted on 2024-10-14 20:18_

---

_Comment by @farzbood on 2024-10-15 04:37_

Hi,
Just updated to the latest version (on Windows10 x86_64), seems no free-threaded build available though.
![image](https://github.com/user-attachments/assets/6180503d-7583-41c0-a95a-e01ce884fb83)


---

_Comment by @louis-jaris on 2024-10-15 08:55_

@farzbood run the following instead: `uv python install 3.13t` (like this PR description is suggesting)

---
