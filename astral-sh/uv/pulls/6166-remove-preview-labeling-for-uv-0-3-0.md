```yaml
number: 6166
title: Remove preview labeling for uv 0.3.0
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
assignees: []
merged: true
base: release/3
head: zb/3-preview
created_at: 2024-08-17T01:02:44Z
updated_at: 2024-08-20T23:32:46Z
url: https://github.com/astral-sh/uv/pull/6166
synced_at: 2026-01-10T13:09:50Z
```

# Remove preview labeling for uv 0.3.0

---

_Pull request opened by @zanieb on 2024-08-17 01:02_

- Removes "experimental" labels from command documentation
- Removes preview warnings
- Removes `PreviewMode` from most structs and methods — we could keep it around but I figure we can propagate it again easily where needed in the future
- Enables preview behavior by default everywhere, e.g., `uv venv` will download Python versions

---

_Label `breaking` added by @zanieb on 2024-08-17 01:02_

---

_Marked ready for review by @zanieb on 2024-08-19 15:34_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-19 16:59_

---

_Merged by @zanieb on 2024-08-19 19:02_

---

_Closed by @zanieb on 2024-08-19 19:02_

---

_Branch deleted on 2024-08-19 19:02_

---

_@paveldikov reviewed on 2024-08-20 22:42_

---

_Review comment by @paveldikov on `crates/uv/src/commands/venv.rs`:137 on 2024-08-20 22:42_

does this mean that relocatable is now considered a stable API? (i'd be delighted with this -- not sure if i am right to make this inference, though)

---

_@zanieb reviewed on 2024-08-20 23:32_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:137 on 2024-08-20 23:32_

I guess so! I tore all of the preview labeling out pretty mechanically. cc @charliermarsh 

We can add an entry to the changelog if we're on the same page.

---
