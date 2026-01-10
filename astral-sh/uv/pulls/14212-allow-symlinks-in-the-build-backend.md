```yaml
number: 14212
title: Allow symlinks in the build backend
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: konsti/allow-symlinks-in-the-build-backend
created_at: 2025-06-23T09:41:48Z
updated_at: 2025-06-25T07:44:23Z
url: https://github.com/astral-sh/uv/pull/14212
synced_at: 2026-01-10T11:10:43Z
```

# Allow symlinks in the build backend

---

_Pull request opened by @konstin on 2025-06-23 09:41_

In workspaces with multiple packages, you usually don't want to include shared files such as the license repeatedly. Instead, we reading from symlinked files. This would be supported if we had used std's `is_file` and read methods, but walkdir's `is_file` does not consider symlinked files as files.

See https://github.com/astral-sh/uv/issues/3957#issuecomment-2994675003

---

_Label `enhancement` added by @konstin on 2025-06-23 09:41_

---

_Label `preview` added by @konstin on 2025-06-23 09:41_

---

_Review requested from @BurntSushi by @konstin on 2025-06-23 09:58_

---

_@jtfmumm reviewed on 2025-06-23 10:18_

---

_Review comment by @jtfmumm on `crates/uv-build-backend/src/lib.rs`:320 on 2025-06-23 10:18_

I think this should be `is_fileish_dir_entry` since "fileish" is one concept. 

---

_@jtfmumm approved on 2025-06-23 10:18_

---

_Comment by @thejcannon on 2025-06-23 11:16_

Ohh this is great, thanks!

---

_@BurntSushi approved on 2025-06-24 16:17_

This LGTM. With two notes. The first is that I would somewhat expect an ideal implementation here to leap before it looks. That is, instead of trying to query a file's type, it should ideally just try to read the entry _as if_ it were a file. And if that fails, then react to the error you get back to determine what you should do next.

The other note is that std generally has the same behavior as walkdir here. In particular, see [`std::fs::DirEntry::file_type`](https://doc.rust-lang.org/std/fs/struct.DirEntry.html#method.file_type). What's different is [`std::path::Path::is_file`](https://doc.rust-lang.org/std/path/struct.Path.html#method.is_file), which does explicitly follow symlinks. The `DirEntry` behavior (in std and walkdir) is important because it means you don't need an extra stat call for every directory entry. If you do need that, then indeed, what you're doing is correct: you opt into that extra cost (in some cases) by doing additional syscalls to follow and look up the type of the symlink's target.

---

_Merged by @konstin on 2025-06-25 07:44_

---

_Closed by @konstin on 2025-06-25 07:44_

---

_Branch deleted on 2025-06-25 07:44_

---
