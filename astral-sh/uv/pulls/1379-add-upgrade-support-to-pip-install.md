```yaml
number: 1379
title: "Add `--upgrade` support to `pip install`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/upgrade-pip-install
created_at: 2024-02-15T23:08:02Z
updated_at: 2024-02-16T01:25:29Z
url: https://github.com/astral-sh/uv/pull/1379
synced_at: 2026-01-12T16:04:36Z
```

# Add `--upgrade` support to `pip install`

---

_@zanieb_

Adds support for `--upgrade` — similar to `--reinstall`.

Closes https://github.com/astral-sh/uv/issues/1391

---

_Label `enhancement` added by @zanieb on 2024-02-15 23:08_

---

_Review requested from @charliermarsh by @zanieb on 2024-02-15 23:52_

---

_@bothra90 reviewed on 2024-02-16 00:01_

---

_Review comment by @bothra90 on `crates/uv/tests/pip_install.rs`:1011 on 2024-02-16 00:01_

n00b question: won't this test break once httpcore has a newer version? Or have you snapshotted pypi as of a certain date that would make this deterministic? I guess the latter, but curious anyway :) 

---

_@zanieb reviewed on 2024-02-16 00:23_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:1011 on 2024-02-16 00:23_

We have a hidden `--exclude-newer` flag which ignores releases after a certain date.

---

_@charliermarsh approved on 2024-02-16 01:22_

---

_Merged by @zanieb on 2024-02-16 01:25_

---

_Closed by @zanieb on 2024-02-16 01:25_

---

_Branch deleted on 2024-02-16 01:25_

---
