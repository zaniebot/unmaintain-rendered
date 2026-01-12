```yaml
number: 5498
title: Use 666 rather than 644 for default permissions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/tem
created_at: 2024-07-26T23:59:31Z
updated_at: 2025-01-28T03:46:48Z
url: https://github.com/astral-sh/uv/pull/5498
synced_at: 2026-01-12T16:06:51Z
```

# Use 666 rather than 644 for default permissions

---

_@charliermarsh_

## Summary

I used 644 because that's the default when writing on my own machine. But -- that's because the default umask is 022. In reality, 666 is the correct default: https://github.com/rust-lang/rust/blob/7c2012d0ec3aae89fefc40e5d6b317a0949cda36/library/std/src/sys/pal/unix/fs.rs#L1069 (and then the umask is applied on top of it).

Closes https://github.com/astral-sh/uv/issues/5496.


---

_Label `bug` added by @charliermarsh on 2024-07-27 00:02_

---

_Merged by @charliermarsh on 2024-07-27 00:08_

---

_Closed by @charliermarsh on 2024-07-27 00:08_

---

_Branch deleted on 2024-07-27 00:08_

---

_Comment by @tmm1 on 2025-01-28 03:46_

Ended up here via #5496.. just wondering what the recommended set up is for a shared multiuser uv cache?

Do I need to tweak umasks for everyone?

---
