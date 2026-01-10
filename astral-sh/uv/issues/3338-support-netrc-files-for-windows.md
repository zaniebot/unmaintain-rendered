---
number: 3338
title: Support _netrc files for windows
type: issue
state: open
author: jurkov
labels:
  - windows
  - external
assignees: []
created_at: 2024-05-02T13:21:08Z
updated_at: 2024-05-02T14:41:33Z
url: https://github.com/astral-sh/uv/issues/3338
synced_at: 2026-01-10T01:23:26Z
---

# Support _netrc files for windows

---

_Issue opened by @jurkov on 2024-05-02 13:21_

Uv does need a .netrc file on Windows. Unfortunately, pip needs a _netfile on Windows.
Uv should also use a _netfile on Windows.

This Issue is similar to #1405, but it's different.

Source: https://stackoverflow.com/questions/6031214/git-how-to-use-netrc-file-on-windows-to-save-user-and-password

---

_Comment by @zanieb on 2024-05-02 13:30_

Can you share some more about this? We're just using the `rust-netrc` crate which should look in your home directory as described?

https://docs.rs/rust-netrc/latest/src/netrc/lib.rs.html#60-71

---

_Comment by @zanieb on 2024-05-02 13:34_

Ah the title vs the issue confused me, you're saying we need to support `_netfile`? You can use the `NETRC` variable to point to an arbitrary file path.

---

_Comment by @jurkov on 2024-05-02 13:47_

Well, I created an issue at https://github.com/gribouille/netrc/issues/1 as well.

---

_Label `upstream` added by @charliermarsh on 2024-05-02 14:21_

---

_Label `windows` added by @konstin on 2024-05-02 14:41_

---
