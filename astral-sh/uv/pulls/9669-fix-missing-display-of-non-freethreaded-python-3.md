```yaml
number: 9669
title: "Fix missing display of non-freethreaded Python 3.13 in `python list`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/list-variant
created_at: 2024-12-06T04:28:44Z
updated_at: 2024-12-06T14:28:30Z
url: https://github.com/astral-sh/uv/pull/9669
synced_at: 2026-01-12T16:08:55Z
```

# Fix missing display of non-freethreaded Python 3.13 in `python list`

---

_@zanieb_

Closes https://github.com/indygreg/python-build-standalone/issues/407

```
❯ cargo run -q -- python list | head -n 2
cpython-3.13.0+freethreaded-macos-aarch64-none    <download available>
cpython-3.13.0-macos-aarch64-none                 <download available>
❯ uv python list | head -n 2
cpython-3.13.0+freethreaded-macos-aarch64-none    <download available>
cpython-3.12.7-macos-aarch64-none                 <download available>
```

---

_Marked ready for review by @zanieb on 2024-12-06 04:30_

---

_Label `bug` added by @zanieb on 2024-12-06 04:30_

---

_@charliermarsh approved on 2024-12-06 11:51_

---

_Merged by @zanieb on 2024-12-06 14:28_

---

_Closed by @zanieb on 2024-12-06 14:28_

---

_Branch deleted on 2024-12-06 14:28_

---
