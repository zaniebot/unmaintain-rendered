```yaml
number: 6005
title: "Support `uv add -r requirements.txt`"
type: pull_request
state: merged
author: blueraft
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: add-r-requirements
created_at: 2024-08-11T13:57:22Z
updated_at: 2024-08-16T21:57:46Z
url: https://github.com/astral-sh/uv/pull/6005
synced_at: 2026-01-10T13:09:50Z
```

# Support `uv add -r requirements.txt`

---

_Pull request opened by @blueraft on 2024-08-11 13:57_

## Summary

Resolves https://github.com/astral-sh/uv/issues/4537

- First commit avoids overwriting dependencies with different markers.
- Second commit supports adding from requirements files.

## Test Plan

`cargo test`


---

_Review comment by @zanieb on `crates/uv/src/commands/project/remove.rs`:98 on 2024-08-11 14:58_

I don't think we should do this cast / change the removal APIs, why was this needed? Removal operates on package names, not full requirements.

---

_@zanieb reviewed on 2024-08-11 14:58_

---

_@zanieb reviewed on 2024-08-11 14:59_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2363 on 2024-08-11 14:59_

Not quite "Install" here, can we retain the previous description? It'd be good to move the "as PEP 508" part to a second line though so it's only present in the long help.

---

_@zanieb reviewed on 2024-08-11 15:00_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2372 on 2024-08-11 15:00_

Should we allow these files? These seem like a bit of a misuse. We don't allow them in `uv run` https://github.com/astral-sh/uv/blob/2822dde8cb24f1486cef59228c723da931690c30/crates/uv/src/commands/project/run.rs#L74-L92

---

_@zanieb reviewed on 2024-08-11 15:01_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2367 on 2024-08-11 15:01_

Same as above, not "install".

---

_Comment by @zanieb on 2024-08-11 15:03_

Could you separate the change to avoid marker collisions from the `requirements.txt` support? I think they'll make more sense to discuss in two pull requests.

---

_Label `cli` added by @zanieb on 2024-08-11 15:03_

---

_Label `preview` added by @zanieb on 2024-08-11 15:03_

---

_@zanieb reviewed on 2024-08-11 15:15_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/remove.rs`:98 on 2024-08-11 15:15_

I see this probably is related to https://github.com/astral-sh/uv/pull/6005/commits/0fbde82c7dbe1ac9fd40919d642f5976d2b79d08 now, perhaps we can cast to a `Requirement` later instead if needed?

---

_@blueraft reviewed on 2024-08-11 17:36_

---

_Review comment by @blueraft on `crates/uv-cli/src/lib.rs`:2372 on 2024-08-11 17:36_

Supporting `setup.py` and `setup.cfg` files could be a helpful feature, making it easier for users to transition to the pyproject.toml standard.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-16 17:26_

---

_Comment by @charliermarsh on 2024-08-16 17:27_

Assigning myself.

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2372 on 2024-08-16 21:29_

It's possible that this could be a killer feature because you could import Poetry projects this way... But it doesn't actually work as-is. I'm gonna remove it and look at it in a separate PR.

---

_@charliermarsh reviewed on 2024-08-16 21:29_

---

_@charliermarsh approved on 2024-08-16 21:37_

---

_Merged by @charliermarsh on 2024-08-16 21:57_

---

_Closed by @charliermarsh on 2024-08-16 21:57_

---
