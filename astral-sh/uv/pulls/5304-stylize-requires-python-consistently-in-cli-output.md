```yaml
number: 5304
title: "Stylize `Requires-Python` consistently in CLI output"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/requires-python-field
created_at: 2024-07-22T18:12:07Z
updated_at: 2024-07-23T18:05:22Z
url: https://github.com/astral-sh/uv/pull/5304
synced_at: 2026-01-12T16:06:44Z
```

# Stylize `Requires-Python` consistently in CLI output

---

_@zanieb_

_No description provided._

---

_Label `cli` added by @zanieb on 2024-07-22 18:12_

---

_Label `preview` added by @zanieb on 2024-07-22 18:12_

---

_@konstin reviewed on 2024-07-22 18:13_

---

_Review comment by @konstin on `crates/uv/src/commands/project/lock.rs`:226 on 2024-07-22 18:13_

The problem is that toml is case sensitive, so `Requires-Python = ">=3.8"` does not work.

---

_@konstin reviewed on 2024-07-22 18:15_

---

_Review comment by @konstin on `crates/uv/src/commands/project/lock.rs`:226 on 2024-07-22 18:15_

I'd probably go with lowercasing it everywhere, the lowercase form is the only one that the user sees, `Requires-Python` is only the core metadata email header format that users should never have to interact with.

---

_@charliermarsh reviewed on 2024-07-22 18:15_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:226 on 2024-07-22 18:15_

@konstin is probably right on that one.

---

_Review comment by @zanieb on `crates/uv/src/commands/project/lock.rs`:226 on 2024-07-22 18:37_

Tragic.

---

_@zanieb reviewed on 2024-07-22 18:37_

---

_@charliermarsh reviewed on 2024-07-22 18:40_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:226 on 2024-07-22 18:40_

I know, aesthetically I dislike it.

---

_@zanieb reviewed on 2024-07-22 18:41_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/lock.rs`:226 on 2024-07-22 18:41_

This is a far more invasive change if we want to be consistent throughout our internal comments as well.

---

_Comment by @zanieb on 2024-07-22 21:05_

I made the minimal changes to the user output but didn't touch our internal commentary.

---

_@charliermarsh approved on 2024-07-22 22:24_

---

_@charliermarsh reviewed on 2024-07-22 22:25_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/pin.rs`:274 on 2024-07-22 22:25_

"value", for consistency above?

---

_@charliermarsh reviewed on 2024-07-22 22:25_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/pin.rs`:274 on 2024-07-22 22:25_

Defer to you though

---

_Renamed from "Stylize `Requires-Python` consistently" to "Stylize `Requires-Python` consistently in CLI output" by @zanieb on 2024-07-23 17:57_

---

_Label `preview` removed by @zanieb on 2024-07-23 17:57_

---

_Merged by @zanieb on 2024-07-23 18:05_

---

_Closed by @zanieb on 2024-07-23 18:05_

---

_Branch deleted on 2024-07-23 18:05_

---
