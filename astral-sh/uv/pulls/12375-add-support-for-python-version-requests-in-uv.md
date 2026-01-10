```yaml
number: 12375
title: "Add support for Python version requests in `uv python list`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/list-request
created_at: 2025-03-21T17:55:17Z
updated_at: 2025-03-23T03:13:59Z
url: https://github.com/astral-sh/uv/pull/12375
synced_at: 2026-01-10T11:10:39Z
```

# Add support for Python version requests in `uv python list`

---

_Pull request opened by @zanieb on 2025-03-21 17:55_

Allows `uv python list <request>` to filter the installed list. I often want this and it's not hard to add.

I tested the remote download filtering locally (#12381 is needed for snapshot tests)

```
❯ cargo run -q -- python list --all-versions 3.13
cpython-3.13.2-macos-aarch64-none    <download available>
cpython-3.13.1-macos-aarch64-none    /opt/homebrew/opt/python@3.13/bin/python3.13 -> ../Frameworks/Python.framework/Versions/3.13/bin/python3.13
cpython-3.13.1-macos-aarch64-none    <download available>
cpython-3.13.0-macos-aarch64-none    /Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
❯ cargo run -q -- python list --all-versions 3.13 --only-installed
cpython-3.13.1-macos-aarch64-none    /opt/homebrew/opt/python@3.13/bin/python3.13 -> ../Frameworks/Python.framework/Versions/3.13/bin/python3.13
cpython-3.13.0-macos-aarch64-none    /Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
```

---

_Label `enhancement` added by @zanieb on 2025-03-21 17:55_

---

_Marked ready for review by @zanieb on 2025-03-21 20:34_

---

_@charliermarsh approved on 2025-03-22 00:05_

Makes sense to me.

---

_Merged by @zanieb on 2025-03-23 03:13_

---

_Closed by @zanieb on 2025-03-23 03:13_

---

_Branch deleted on 2025-03-23 03:13_

---
