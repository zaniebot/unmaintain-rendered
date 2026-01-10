```yaml
number: 4949
title: "Add support for serializing `PythonRequest` to a canonical string"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/python-request-ser
created_at: 2024-07-09T22:58:25Z
updated_at: 2024-07-10T15:24:47Z
url: https://github.com/astral-sh/uv/pull/4949
synced_at: 2026-01-10T13:42:52Z
```

# Add support for serializing `PythonRequest` to a canonical string

---

_Pull request opened by @zanieb on 2024-07-09 22:58_

For #4950 

---

_Label `internal` added by @zanieb on 2024-07-09 22:58_

---

_Marked ready for review by @zanieb on 2024-07-09 23:24_

---

_Review requested from @BurntSushi by @zanieb on 2024-07-10 14:28_

---

_Review requested from @ibraheemdev by @zanieb on 2024-07-10 14:28_

---

_@charliermarsh reviewed on 2024-07-10 15:13_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:1173 on 2024-07-10 15:13_

I kind of feel like this should be implementing serialize and deserialize... But I don't know what the "backend" would be, like it's not JSON etc.

---

_@charliermarsh approved on 2024-07-10 15:13_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1173 on 2024-07-10 15:19_

I'm a little naive about that but would be basically fine with `parse` and `to_canonical_string` moving to `Serialize` and `Deserialize` implementations. The only caveat is that `parse` is infallible.

I also considered moving to the `from_str` and `to_string` implementations and using some other helper like "user_display" for the cases where we want to show a pretty name to users?

---

_@zanieb reviewed on 2024-07-10 15:19_

---

_Merged by @zanieb on 2024-07-10 15:24_

---

_Closed by @zanieb on 2024-07-10 15:24_

---

_Branch deleted on 2024-07-10 15:24_

---
