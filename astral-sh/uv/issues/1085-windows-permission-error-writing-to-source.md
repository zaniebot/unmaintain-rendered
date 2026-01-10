---
number: 1085
title: "Windows: Permission error writing to source distribution cache due to symlinking"
type: issue
state: closed
author: konstin
labels:
  - windows
assignees: []
created_at: 2024-01-24T21:36:58Z
updated_at: 2024-01-25T09:06:40Z
url: https://github.com/astral-sh/uv/issues/1085
synced_at: 2026-01-10T01:23:05Z
---

# Windows: Permission error writing to source distribution cache due to symlinking

---

_Issue opened by @konstin on 2024-01-24 21:36_

```console
$ cargo run -q -- pip install black --no-cache
Resolved 7 packages in 559ms
error: Failed to download distributions
  Caused by: Failed to fetch wheel: mypy-extensions==1.0.0
  Caused by: Failed to write to source distribution cache
  Caused by: Dem Client fehlt ein erforderliches Recht. (os error 1314)
```

The problem is the lack of (non-dev mode, non-admin) symlinking on windows [here](https://github.com/astral-sh/puffin/blob/afb571643f2b8669d12cd09301f9a9f85a8d09d0/crates/puffin-fs/src/lib.rs#L14):
```
[crates\puffin-fs\src\lib.rs:14] std::os::windows::fs::symlink_dir(src, dst) = Err(
    Os {
        code: 1314,
        kind: Uncategorized,
        message: "Dem Client fehlt ein erforderliches Recht.",
    },
)
```

---

_Comment by @charliermarsh on 2024-01-24 21:37_

👍 This is expected. Do you want to try to reimplement for Windows via Junctions, or should I?

---

_Label `windows` added by @charliermarsh on 2024-01-24 21:38_

---

_Comment by @konstin on 2024-01-24 21:38_

Could you do this? I'd need this for the script launchers.

---

_Comment by @charliermarsh on 2024-01-24 21:39_

Yes I can try it tonight.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-24 21:39_

---

_Comment by @konstin on 2024-01-24 21:39_

Thanks!

Btw we should also make https://github.com/astral-sh/puffin/blob/d9cc9dbf88d0bfe4449e69dc8115235da4d70fc0/crates/puffin-distribution/src/source/error.rs#L26 more generic, this isn't the source dist cache anymore but i think all kinds of dists.


---

_Closed by @konstin on 2024-01-24 21:39_

---

_Reopened by @charliermarsh on 2024-01-24 21:40_

---

_Referenced in [astral-sh/uv#1087](../../astral-sh/uv/pulls/1087.md) on 2024-01-25 01:26_

---

_Closed by @konstin on 2024-01-25 09:06_

---
