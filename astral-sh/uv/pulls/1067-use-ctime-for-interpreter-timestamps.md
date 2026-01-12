```yaml
number: 1067
title: Use ctime for interpreter timestamps
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/time
created_at: 2024-01-23T19:39:44Z
updated_at: 2024-01-23T19:52:21Z
url: https://github.com/astral-sh/uv/pull/1067
synced_at: 2026-01-12T16:04:23Z
```

# Use ctime for interpreter timestamps

---

_@charliermarsh_

Per https://apenwarr.ca/log/20181113, `ctime` should be a lot more conservative, and should detect things like the issue we see with the python-build-standalone builds, where the `mtime` is identical across builds.

On Windows, I'm just using `last_write_time`. But we should probably add `volume_serial_number` and other attributes via [`winapi_util`](https://docs.rs/winapi-util/latest/winapi_util/index.html).


---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-23 19:39_

---

_Review requested from @konstin by @charliermarsh on 2024-01-23 19:39_

---

_Marked ready for review by @charliermarsh on 2024-01-23 19:39_

---

_Review comment by @BurntSushi on `crates/puffin-interpreter/src/interpreter.rs`:344 on 2024-01-23 19:42_

Looks like a stray `It's`.

---

_@BurntSushi approved on 2024-01-23 19:43_

LGTM

---

_Label `bug` added by @charliermarsh on 2024-01-23 19:45_

---

_Merged by @charliermarsh on 2024-01-23 19:52_

---

_Closed by @charliermarsh on 2024-01-23 19:52_

---

_Branch deleted on 2024-01-23 19:52_

---
