```yaml
number: 16855
title: "Fix ``uv pip install -r /dev/stdin``"
type: pull_request
state: merged
author: nsoranzo
labels:
  - bug
assignees: []
merged: true
base: main
head: fix_pip_install_dev_stdin
created_at: 2025-11-26T02:59:29Z
updated_at: 2025-11-26T03:14:41Z
url: https://github.com/astral-sh/uv/pull/16855
synced_at: 2026-01-12T16:12:29Z
```

# Fix ``uv pip install -r /dev/stdin``

---

_@nsoranzo_

## Summary

Fix ``uv pip install -r /dev/stdin`` which was broken in uv 0.9.12 by https://github.com/astral-sh/uv/pull/16805 .

Example of the issue:

```
$ echo "flask" | uv pip install -r /dev/stdin
warning: Requirements file `/dev/stdin` does not contain any dependencies
Audited in 8ms
```

Note that "upstream" ``pip install`` does support `-r /dev/stdin` and doesn't support `-r -` .

## Test Plan

2 new tests added.


---

_@charliermarsh approved on 2025-11-26 03:01_

---

_Label `bug` added by @charliermarsh on 2025-11-26 03:01_

---

_Merged by @charliermarsh on 2025-11-26 03:13_

---

_Closed by @charliermarsh on 2025-11-26 03:13_

---

_Branch deleted on 2025-11-26 03:14_

---
