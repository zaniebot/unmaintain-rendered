```yaml
number: 8963
title: "Enable `uv tool uninstall uv` on Windows"
type: pull_request
state: merged
author: charliermarsh
labels:
  - windows
assignees: []
merged: true
base: main
head: charlie/self-tool
created_at: 2024-11-09T01:31:55Z
updated_at: 2024-12-10T18:13:27Z
url: https://github.com/astral-sh/uv/pull/8963
synced_at: 2026-01-12T16:08:34Z
```

# Enable `uv tool uninstall uv` on Windows

---

_@charliermarsh_

## Summary

Extending self-delete and self-replace functionality to uv itself on Windows.

Closes https://github.com/astral-sh/uv/issues/6400.


---

_Review requested from @zanieb by @charliermarsh on 2024-11-09 01:31_

---

_Label `windows` added by @charliermarsh on 2024-11-09 01:32_

---

_@charliermarsh reviewed on 2024-11-09 01:33_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/common.rs`:130 on 2024-11-09 01:33_

@zanieb -- I noticed this logic is such that we delete the existing entrypoints, then we write them in the `for (name, source_path, target_path) in &target_entry_points ` loop below. For self-replace purposes, it's easier to model it as a replace rather than a delete + write. Do you think that's a problem?

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/common.rs`:130 on 2024-11-15 18:20_

I think that's fine, though it'll change the error behavior. Perhaps we should defer errors until the end so we accomplish as many replacements as we can? 

---

_@zanieb reviewed on 2024-11-15 18:20_

---

_Comment by @zanieb on 2024-11-15 18:21_

I figure this pull request is loosely on hold until we figure out what we want to do with #9143 and https://github.com/astral-sh/uv/issues/9144

---

_Comment by @zanieb on 2024-12-10 18:11_

@charliermarsh security flagging looks fixed, let's merge this?

---

_Merged by @charliermarsh on 2024-12-10 18:13_

---

_Closed by @charliermarsh on 2024-12-10 18:13_

---

_Branch deleted on 2024-12-10 18:13_

---

_Comment by @charliermarsh on 2024-12-10 18:13_

Sg.

---
