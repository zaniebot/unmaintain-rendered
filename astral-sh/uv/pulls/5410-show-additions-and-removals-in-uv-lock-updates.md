```yaml
number: 5410
title: "Show additions and removals in `uv lock` updates"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/update
created_at: 2024-07-24T14:55:59Z
updated_at: 2024-07-24T15:33:10Z
url: https://github.com/astral-sh/uv/pull/5410
synced_at: 2026-01-12T16:06:48Z
```

# Show additions and removals in `uv lock` updates

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/5401.


---

_Label `enhancement` added by @charliermarsh on 2024-07-24 14:56_

---

_Label `preview` added by @charliermarsh on 2024-07-24 14:56_

---

_@zanieb reviewed on 2024-07-24 15:00_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/lock.rs`:593 on 2024-07-24 15:00_

I think I may prefer "Updated", "Added", "Removed", etc. — I think we generally prefer "Did thing" over "Doing thing"? Maybe another one for the style guide.

---

_@charliermarsh reviewed on 2024-07-24 15:03_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:593 on 2024-07-24 15:03_

No strong opinion here, sounds fine to me.

---

_@zanieb approved on 2024-07-24 15:27_

---

_Merged by @charliermarsh on 2024-07-24 15:33_

---

_Closed by @charliermarsh on 2024-07-24 15:33_

---

_Branch deleted on 2024-07-24 15:33_

---
