```yaml
number: 7815
title: "Read and write `.python-version` in the workspace root"
type: pull_request
state: closed
author: j178
labels: []
assignees: []
draft: true
base: main
head: version-file
created_at: 2024-09-30T16:28:26Z
updated_at: 2024-11-09T07:06:04Z
url: https://github.com/astral-sh/uv/pull/7815
synced_at: 2026-01-12T16:08:01Z
```

# Read and write `.python-version` in the workspace root

---

_@j178_

## Summary

This PR modifies `uv init` and `uv python pin` to read and write the Python version file from the discovered workspace root, instead of the current working directory.

It also includes some behavioral changes (noted in the self-review comments). The previous behavior seemed erroneous, but if it wasn't, please instruct me to revert these changes.

Resolves #7806

---

_Review comment by @j178 on `crates/uv/src/commands/project/init.rs`:775 on 2024-09-30 16:48_

If the version file already exists, but does not contain the requested version, should we update it? (add the version, or replace the existing one)
For now, we keep the existing behavior: if the file exists, we don't update it.

---

_@j178 reviewed on 2024-09-30 16:48_

---

_Review comment by @j178 on `crates/uv/src/commands/python/pin.rs`:145 on 2024-09-30 19:06_

Behavior change: `uv python pin` now updates the discovered version file, rather than always writing to `.python-version`.

---

_Review comment by @j178 on `crates/uv/src/commands/python/pin.rs`:143 on 2024-09-30 19:09_

Behavior change: `uv python pin` now cross-checks the requested version with all versions in the version file, not just the first one. It also avoids updating the version file if the required version already exists. The message updated to "`.python-version` is already pinned at `3.12`".

---

_@j178 reviewed on 2024-09-30 19:14_

---

_Marked ready for review by @j178 on 2024-09-30 19:18_

---

_Renamed from "Don't create .python-version in workspace members" to "Read and write `.python-version` in the workspace root" by @j178 on 2024-09-30 19:18_

---

_Assigned to @zanieb by @zanieb on 2024-09-30 19:43_

---

_Comment by @zanieb on 2024-10-02 18:14_

This is going to be a bit complicated as it conflicts with #6370 and #6372

---

_Converted to draft by @j178 on 2024-10-09 09:28_

---

_Closed by @j178 on 2024-11-09 07:06_

---
