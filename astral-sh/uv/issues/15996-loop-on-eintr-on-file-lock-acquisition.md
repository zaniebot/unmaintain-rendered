```yaml
number: 15996
title: "Loop on `EINTR` on file lock acquisition"
type: issue
state: open
author: zanieb
labels:
  - help wanted
assignees: []
created_at: 2025-09-22T21:48:16Z
updated_at: 2025-11-11T17:50:08Z
url: https://github.com/astral-sh/uv/issues/15996
synced_at: 2026-01-10T03:23:54Z
```

# Loop on `EINTR` on file lock acquisition

---

_Issue opened by @zanieb on 2025-09-22 21:48_

> We should loop on `EINTR`, and probably in the blocking cases too, unless I'm missing something that already does that.

_Originally posted by @geofft in https://github.com/astral-sh/uv/pull/15992#discussion_r2370330069_
            

---

_Comment by @zanieb on 2025-09-22 21:48_

I'm not sure if something below us takes care of this.

---

_Label `help wanted` added by @zanieb on 2025-09-22 21:48_

---

_Comment by @aznszn on 2025-11-11 17:50_

I was looking into this and it seems like nothing below UV takes care of this

in fs_err
```rust
fn try_lock_exclusive(&self) -> Result<()> {
    sys::try_lock_exclusive(self)
}

pub fn try_lock_exclusive(file: &File) -> Result<()> {
    flock(file, libc::LOCK_EX | libc::LOCK_NB)
}

#[cfg(not(target_os = "solaris"))]
fn flock(file: &File, flag: libc::c_int) -> Result<()> {
    let ret = unsafe { libc::flock(file.as_raw_fd(), flag) };
    if ret < 0 { Err(Error::last_os_error()) } else { Ok(()) }
}
```

so should we unconditionally loop on EINTR until we get the lock or an error other than EINTR, or should we define something like `MAX_EINTR_RETRIES` after which we return a custom too many retries error

---
