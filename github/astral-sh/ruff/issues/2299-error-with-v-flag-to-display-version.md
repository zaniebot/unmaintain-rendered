---
number: 2299
title: Error with -V flag to display version
type: issue
state: closed
author: lcheylus
labels: []
assignees: []
created_at: 2023-01-28T15:33:51Z
updated_at: 2023-01-28T15:47:49Z
url: https://github.com/astral-sh/ruff/issues/2299
synced_at: 2026-01-07T13:12:14-06:00
---

# Error with -V flag to display version

---

_Issue opened by @lcheylus on 2023-01-28 15:33_

With the latest version (commit 861df12269fbe73931b0b41ea2561f665f16e3ba), build OK on Debian/testing amd64 with Rust 1.65.

I have an error when I'm trying to use `-V` flag to display the current version : 
```
$ ./target/release/ruff -V
error: unexpected argument '-V' found

  note: to pass '-V' as a value, use '-- -V'
```

No error with `--version` flag : 
```
./target/release/ruff --version
ruff 0.0.236
```


---

_Comment by @charliermarsh on 2023-01-28 15:42_

\cc @not-my-profile 

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-28 15:42_

---

_Comment by @charliermarsh on 2023-01-28 15:42_

I got it.

---

_Referenced in [astral-sh/ruff#2301](../../astral-sh/ruff/pulls/2301.md) on 2023-01-28 15:43_

---

_Closed by @charliermarsh on 2023-01-28 15:47_

---

_Closed by @charliermarsh on 2023-01-28 15:47_

---
