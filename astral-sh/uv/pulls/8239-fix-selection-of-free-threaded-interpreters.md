```yaml
number: 8239
title: Fix selection of free-threaded interpreters during default Python discovery
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/option-version-req
created_at: 2024-10-16T03:11:48Z
updated_at: 2024-10-16T19:44:35Z
url: https://github.com/astral-sh/uv/pull/8239
synced_at: 2026-01-12T16:08:13Z
```

# Fix selection of free-threaded interpreters during default Python discovery

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/8228

e.g., on this branch

```
❯ uv python install 3.13t 3.13
❯ cargo build
❯ cargo run -q --bin uvx -- --from build python -c "import sys; print(sys.base_prefix)"
/Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none
❯ cargo run -q --bin uvx -- -p 3.13 --from build python -c "import sys; print(sys.base_prefix)"
/Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none
❯ cargo run -q --bin uvx -- -p 3.13t --from build python -c "import sys; print(sys.base_prefix)"
/Users/zb/.local/share/uv/python/cpython-3.13.0+freethreaded-macos-aarch64-none
```

and on main

```
❯ cargo build
❯ cargo run -q --bin uvx -- --from build python -c "import sys; print(sys.base_prefix)"
Installed 3 packages in 12ms
/Users/zb/.local/share/uv/python/cpython-3.13.0+freethreaded-macos-aarch64-none
```

I want to add more test coverage around this, but I've noticed the free-threaded discovery tests are a bit off as-is and it'll be a bigger task. I think the recent bugs around discovery indicate we should invest more into that test framework.



---

_Label `bug` added by @zanieb on 2024-10-16 03:11_

---

_Comment by @zanieb on 2024-10-16 04:16_

The failing integration test looks like a real problem with this approach. Will investigate.

---

_@charliermarsh approved on 2024-10-16 19:36_

---

_Merged by @zanieb on 2024-10-16 19:44_

---

_Closed by @zanieb on 2024-10-16 19:44_

---

_Branch deleted on 2024-10-16 19:44_

---
