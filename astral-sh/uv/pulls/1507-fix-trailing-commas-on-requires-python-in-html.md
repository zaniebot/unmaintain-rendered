```yaml
number: 1507
title: "Fix trailing commas on `Requires-Python` in HTML indexes "
type: pull_request
state: merged
author: davidszotten
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-trailing-commas
created_at: 2024-02-16T16:10:47Z
updated_at: 2024-02-17T07:42:44Z
url: https://github.com/astral-sh/uv/pull/1507
synced_at: 2026-01-12T16:04:38Z
```

# Fix trailing commas on `Requires-Python` in HTML indexes 

---

_@davidszotten_

illustration and suggested fix for #1464 

---

_Comment by @charliermarsh on 2024-02-16 16:11_

We tend to fix this via our `lenient_requirements.rs` thing -- can you take a look at that file?

---

_Comment by @davidszotten on 2024-02-16 21:07_

ah i see. how does this look?

---

_Comment by @charliermarsh on 2024-02-16 21:54_

Ahh yes, this looks good.

---

_Review requested from @charliermarsh by @charliermarsh on 2024-02-16 21:54_

---

_Label `bug` added by @charliermarsh on 2024-02-16 21:54_

---

_Renamed from "Fix trailing commas" to "Fix trailing commas on`Requires-Python` in HTML indexes " by @charliermarsh on 2024-02-16 21:54_

---

_Renamed from "Fix trailing commas on`Requires-Python` in HTML indexes " to "Fix trailing commas on `Requires-Python` in HTML indexes " by @charliermarsh on 2024-02-16 21:57_

---

_@charliermarsh approved on 2024-02-16 21:59_

Thanks!

---

_Merged by @charliermarsh on 2024-02-16 22:05_

---

_Closed by @charliermarsh on 2024-02-16 22:05_

---

_@davidszotten reviewed on 2024-02-17 07:42_

---

_Review comment by @davidszotten on `crates/uv-client/src/html.rs`:141 on 2024-02-17 07:42_

how come from is preferable? (to help me learn)

---
