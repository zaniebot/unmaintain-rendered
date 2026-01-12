```yaml
number: 9782
title: "Add `uv python list --all-arches`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/all-arches
created_at: 2024-12-10T18:54:50Z
updated_at: 2024-12-10T20:02:43Z
url: https://github.com/astral-sh/uv/pull/9782
synced_at: 2026-01-12T16:08:58Z
```

# Add `uv python list --all-arches`

---

_@zanieb_

With #9781 this becomes even more compelling. This is generally useful as well.

e.g.,

```
❯ cargo run -- python list --all-arches
cpython-3.13.1+freethreaded-macos-x86_64-none     <download available>
cpython-3.13.1-macos-x86_64-none                  <download available>
cpython-3.13.1+freethreaded-macos-aarch64-none    <download available>
cpython-3.13.1-macos-aarch64-none                 <download available>
cpython-3.13.0-macos-aarch64-none                 /Users/zb/.local/bin/python3.13 -> /Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
cpython-3.13.0-macos-aarch64-none                 /Users/zb/.local/bin/python3 -> /Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
cpython-3.13.0-macos-aarch64-none                 /Users/zb/.local/bin/python -> /Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
cpython-3.13.0-macos-aarch64-none                 /Users/zb/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
cpython-3.12.8-macos-x86_64-none                  <download available>
cpython-3.12.8-macos-aarch64-none                 <download available>
...
```

---

_Label `enhancement` added by @zanieb on 2024-12-10 18:54_

---

_Marked ready for review by @zanieb on 2024-12-10 19:11_

---

_@charliermarsh approved on 2024-12-10 19:54_

---

_Merged by @zanieb on 2024-12-10 20:02_

---

_Closed by @zanieb on 2024-12-10 20:02_

---

_Branch deleted on 2024-12-10 20:02_

---
