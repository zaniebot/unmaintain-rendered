```yaml
number: 5396
title: "Add `uv init --virtual`"
type: pull_request
state: merged
author: j178
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: virtual
created_at: 2024-07-24T04:30:19Z
updated_at: 2024-07-25T03:33:40Z
url: https://github.com/astral-sh/uv/pull/5396
synced_at: 2026-01-12T16:06:47Z
```

# Add `uv init --virtual`

---

_@j178_

## Summary

Add `uv init --virtual` to create an explicit virtual workspace.

Relates to #5338

---

_@j178 reviewed on 2024-07-24 04:38_

---

_Review comment by @j178 on `crates/uv-workspace/src/workspace.rs`:999 on 2024-07-24 04:38_

Running from the current directory, this message is "Nested workspaces are not supported, but outer workspace includes existing workspace: ``", seems not clear.

---

_Marked ready for review by @j178 on 2024-07-24 04:46_

---

_@j178 reviewed on 2024-07-24 05:04_

---

_Review comment by @j178 on `crates/uv/src/commands/project/init.rs`:139 on 2024-07-24 05:04_

Should we warn or abort if nested workspace detected?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-24 18:35_

---

_Label `enhancement` added by @charliermarsh on 2024-07-24 18:35_

---

_Label `preview` added by @charliermarsh on 2024-07-24 18:35_

---

_@charliermarsh approved on 2024-07-24 18:43_

---

_Merged by @charliermarsh on 2024-07-24 18:52_

---

_Closed by @charliermarsh on 2024-07-24 18:52_

---

_@charliermarsh reviewed on 2024-07-24 18:59_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/init.rs`:139 on 2024-07-24 18:59_

I think warn is ok.

---

_@charliermarsh reviewed on 2024-07-24 18:59_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/workspace.rs`:999 on 2024-07-24 18:59_

Good call.

---

_Branch deleted on 2024-07-25 03:33_

---
