```yaml
number: 16160
title: "Ban pre-release versions in `uv python upgrade` requests"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/pre-up
created_at: 2025-10-07T19:39:27Z
updated_at: 2025-10-07T20:07:12Z
url: https://github.com/astral-sh/uv/pull/16160
synced_at: 2026-01-10T06:36:15Z
```

# Ban pre-release versions in `uv python upgrade` requests

---

_Pull request opened by @zanieb on 2025-10-07 19:39_

_No description provided._

---

_Label `bug` added by @zanieb on 2025-10-07 19:39_

---

_Review requested from @geofft by @zanieb on 2025-10-07 19:53_

---

_Review comment by @geofft on `crates/uv-python/src/discovery.rs`:2589 on 2025-10-07 20:02_

`Self::Range` isn't totally correct here because this wrongly lets through `uv python upgrade ==3.14.0rc3`, but then again we currently wrongly permit `uv python upgrade ==3.13.6`, and we probably should just wholly reject range syntax from `uv python upgrade`, so it doesn't totally matter what we return here.

---

_Marked ready for review by @zanieb on 2025-10-07 20:02_

---

_@geofft approved on 2025-10-07 20:02_

---

_Merged by @zanieb on 2025-10-07 20:07_

---

_Closed by @zanieb on 2025-10-07 20:07_

---

_Branch deleted on 2025-10-07 20:07_

---
