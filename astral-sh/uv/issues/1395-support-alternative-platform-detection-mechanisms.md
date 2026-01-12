```yaml
number: 1395
title: Support alternative platform detection mechanisms other than reading /bin/ls
type: issue
state: closed
author: robin-nitrokey
labels:
  - bug
assignees: []
created_at: 2024-02-16T00:01:47Z
updated_at: 2024-02-16T12:51:35Z
url: https://github.com/astral-sh/uv/issues/1395
synced_at: 2026-01-12T15:58:27Z
```

# Support alternative platform detection mechanisms other than reading /bin/ls

---

_@robin-nitrokey_

The `find_libc` function that is used to determine whether `uv` is running on `musllinux` or `manylinux` tries to read `/bin/ls`:

https://github.com/astral-sh/uv/blob/c6a43e92f91df8cd133f8e13800c3dbc4b113bb4/crates/platform-host/src/linux.rs#L128-L141

If `/bin/ls` does not exist, `uv` cannot be run.  This is the case namely on NixOS which does not follow the Filesystem Hierarchy Standard.  I understand that this is a special case, but it would be great if you would provide a way to skip this check, e. g. by setting an environment variable (or maybe using `/bin/sh` instead which is available on NixOS).

---

_Label `bug` added by @zanieb on 2024-02-16 00:05_

---

_Comment by @zanieb on 2024-02-16 00:05_

Thanks for the clear report!

---

_Comment by @zanieb on 2024-02-16 00:06_

If you patch this to read `/bin/sh` does it "just work"?

---

_Comment by @robin-nitrokey on 2024-02-16 00:10_

Yes, with `/bin/sh` it works for me.

---

_Closed by @BurntSushi on 2024-02-16 12:51_

---
