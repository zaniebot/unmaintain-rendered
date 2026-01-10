```yaml
number: 9512
title: Copy files, then create directory on fallback
type: pull_request
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
draft: true
base: main
head: charlie/stable
created_at: 2024-11-28T18:10:51Z
updated_at: 2024-12-04T18:31:04Z
url: https://github.com/astral-sh/uv/pull/9512
synced_at: 2026-01-10T12:00:00Z
```

# Copy files, then create directory on fallback

---

_Pull request opened by @charliermarsh on 2024-11-28 18:10_

## Summary

Most wheel entries are files, not directories. So it seems more efficient to attempt to create the file, then create the directory on-error, rather than checking every entry to see if it's a directory.

(I need to benchmark this.)


---

_Label `performance` added by @charliermarsh on 2024-11-28 18:10_

---

_Comment by @konstin on 2024-12-04 18:02_

Isn't the filetype already retrieved by walkdir so this check is just a bitshift?

---

_Comment by @charliermarsh on 2024-12-04 18:18_

Hmm, I don't think so, it looks like this ultimately calls `self.is(libc::S_IFDIR)`.

---

_Comment by @konstin on 2024-12-04 18:21_

That call then does the bitmask and compare: https://github.com/rust-lang/rust/blob/96e51d9482405e400dec53750f3b263d45784ada/library/std/src/sys/pal/unix/fs.rs#L651-L657

---

_Comment by @charliermarsh on 2024-12-04 18:31_

Thank you!

---

_Closed by @charliermarsh on 2024-12-04 18:31_

---
