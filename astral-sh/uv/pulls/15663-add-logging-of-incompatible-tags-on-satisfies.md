```yaml
number: 15663
title: Add logging of incompatible tags on satisfies check
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
assignees: []
merged: true
base: main
head: zb/incompatible-tag-log
created_at: 2025-09-03T13:46:31Z
updated_at: 2025-09-03T16:45:51Z
url: https://github.com/astral-sh/uv/pull/15663
synced_at: 2026-01-12T16:11:52Z
```

# Add logging of incompatible tags on satisfies check

---

_@zanieb_

I was trying to understand https://github.com/astral-sh/uv/issues/9559 and think we need more logs to see what's going on. 

---

_Label `tracing` added by @zanieb on 2025-09-03 13:46_

---

_@charliermarsh reviewed on 2025-09-03 14:17_

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/expanded_tags.rs`:63 on 2025-09-03 14:17_

It would be nice to avoid these but I understand that this is a rare case.

---

_@zanieb reviewed on 2025-09-03 14:19_

---

_Review comment by @zanieb on `crates/uv-distribution-filename/src/expanded_tags.rs`:63 on 2025-09-03 14:19_

👍 I think we could change the `compatibility` API, I just didn't want to do so here.

---

_Marked ready for review by @zanieb on 2025-09-03 14:20_

---

_Merged by @zanieb on 2025-09-03 16:45_

---

_Closed by @zanieb on 2025-09-03 16:45_

---

_Branch deleted on 2025-09-03 16:45_

---
