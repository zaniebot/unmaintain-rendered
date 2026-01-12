```yaml
number: 4519
title: "Add guard to `replace_symlink` on Windows"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/guard-symlink
created_at: 2024-06-25T14:59:20Z
updated_at: 2024-06-25T18:47:41Z
url: https://github.com/astral-sh/uv/pull/4519
synced_at: 2026-01-12T16:06:17Z
```

# Add guard to `replace_symlink` on Windows

---

_@zanieb_

`junction::create` apparently will happily succeed but not create a link to files? Since our symlink function does not indicate that it cannot handle files, this was quite surprising.


Tested over in #4509 which previously failed on an assertion that `black.exe` existed.
```
error: Failed to install entrypoint
    Caused by: Cannot create a junction for [TEMP_DIR]/tools/black/Scripts/black.exe: is not a directory
```

We should file an issue upstream too, I think?

---

_Label `internal` added by @zanieb on 2024-06-25 14:59_

---

_@zanieb reviewed on 2024-06-25 15:04_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:55 on 2024-06-25 15:04_

I don't check `is_dir()` because I don't want to enforce that it exists in case that's too constraining, happy to change if people feel strongly.

---

_Comment by @zanieb on 2024-06-25 15:23_

I realized my test case was missing the `.exe` suffix in the assertion. Let me see what happens when you create a junction to a file...

---

_Converted to draft by @zanieb on 2024-06-25 15:25_

---

_Comment by @zanieb on 2024-06-25 16:03_

Though my assertion was dumb, this still looks generally problematic.

---

_Marked ready for review by @zanieb on 2024-06-25 16:05_

---

_Comment by @zanieb on 2024-06-25 16:05_

I'd like to follow this with adding an API that's safe to use for files.

---

_@charliermarsh approved on 2024-06-25 18:07_

---

_Merged by @zanieb on 2024-06-25 18:47_

---

_Closed by @zanieb on 2024-06-25 18:47_

---

_Branch deleted on 2024-06-25 18:47_

---
