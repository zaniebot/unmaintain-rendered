```yaml
number: 7992
title: "Remove the \"nursery\" rule group"
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2023-10-16T19:09:54Z
updated_at: 2024-02-01T19:35:08Z
url: https://github.com/astral-sh/ruff/issues/7992
synced_at: 2026-01-10T11:09:50Z
```

# Remove the "nursery" rule group

---

_Issue opened by @zanieb on 2023-10-16 19:09_

`RuleGroup::Nursery` is currently included for backwards compatibility but should be removed in Ruff v0.2.0

This change will include removing backwards compatible selection of nursery rules. All nursery rules should be moved to preview.

---

_Added to milestone `v0.2.0` by @zanieb on 2023-10-16 19:19_

---

_Renamed from "Remove `RuleGroup::Nursery`" to "Remove the "nursery" rule group" by @zanieb on 2023-10-16 19:20_

---

_Comment by @zanieb on 2023-10-16 19:22_

We may want to keep `RuleSelector::Nursery` in order to throw a nicer error message then remove it in v0.3.0 but I'm not sure how feasible that will be. If we keep it, we should hide it from the schema.

---

_Assigned to @zanieb by @zanieb on 2024-01-29 17:55_

---

_Comment by @zanieb on 2024-01-29 21:35_

In #9683 we make this error in most cases, but retain the selector for error messages.

In v0.3.0 we should remove the group entirely.

---

_Removed from milestone `v0.2.0` by @zanieb on 2024-01-29 21:35_

---

_Added to milestone `v0.3.0` by @zanieb on 2024-01-29 21:35_

---

_Closed by @zanieb on 2024-02-01 19:35_

---
