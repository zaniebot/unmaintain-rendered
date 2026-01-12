```yaml
number: 2674
title: "Rename `uv-traits` and split into separate modules"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/traits-types
created_at: 2024-03-26T19:52:32Z
updated_at: 2024-03-26T20:39:44Z
url: https://github.com/astral-sh/uv/pull/2674
synced_at: 2026-01-12T16:05:09Z
```

# Rename `uv-traits` and split into separate modules

---

_@zanieb_

This is driving me a little crazy and is becoming a larger problem in #2596 where I need to move more types (like `Upgrade` and `Reinstall`) into this crate. Anything that's shared across our core resolver, install, and build crates needs to be defined in this crate to avoid cyclic dependencies. We've outgrown it being a single file with some shared traits.

There are no behavioral changes here.

---

_Label `internal` added by @zanieb on 2024-03-26 19:54_

---

_Marked ready for review by @zanieb on 2024-03-26 19:54_

---

_@charliermarsh approved on 2024-03-26 19:56_

Makes sense.

---

_Comment by @zanieb on 2024-03-26 19:58_

Sorry about the diff 🤷‍♀️ 

---

_Merged by @zanieb on 2024-03-26 20:39_

---

_Closed by @zanieb on 2024-03-26 20:39_

---

_Branch deleted on 2024-03-26 20:39_

---
