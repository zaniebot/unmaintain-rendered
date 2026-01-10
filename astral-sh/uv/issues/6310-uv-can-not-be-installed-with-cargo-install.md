---
number: 6310
title: "uv can not be installed with \"cargo install\" ?"
type: issue
state: closed
author: fconil
labels:
  - question
assignees: []
created_at: 2024-08-21T08:13:51Z
updated_at: 2024-08-21T08:35:43Z
url: https://github.com/astral-sh/uv/issues/6310
synced_at: 2026-01-10T01:23:58Z
---

# uv can not be installed with "cargo install" ?

---

_Issue opened by @fconil on 2024-08-21 08:13_

Hi,

I saw a lot of enthusiasm for the `uv` 3.0 release announced yesterday.

I am on Linux (Ubuntu) and I have cargo running on my computer as I have installed a few tools with it (difftastic, xsv, …).

In the documentation, it is adviced to use an install script on Linux. `uv` is written in Rust but I do not see any `cargo install` option.

 `cargo search` show me another package for `uv`.

```shell
$ cargo search uv
uv = "0.1.0"                      # universal getter
```

I am not a Rust developper, can you explain me why `cargo install` is not adviced and where your install script will place `uv` ?

```shell
$ ls ~/.cargo/bin/
cargo         cargo-fmt   clippy-driver  difft ...
```
Is there a risk of conflict between the `uv` (universal getter) rust package and the `uv ` tool I am interested in ?

Thanks for your work
Regards

---

_Label `question` added by @konstin on 2024-08-21 08:14_

---

_Comment by @konstin on 2024-08-21 08:19_

uv is not published to crates.io (we use some git dependencies and we don't provide a stable rust interface) and we don't recommend using cargo to install but one of the other recommended mechanisms, such as curl, pipx, pip, brew or downloading the binary from the releases page. If you still want to use cargo, `cargo install --git https://github.com/astral-sh/uv --tag 0.3.0 uv` works.

---

_Comment by @fconil on 2024-08-21 08:27_

Thanks for your prompt reply

I will use the install script then, I do not absolutely want to use `cargo` :)

---

_Closed by @konstin on 2024-08-21 08:35_

---

_Referenced in [astral-sh/uv#6956](../../astral-sh/uv/issues/6956.md) on 2024-09-03 06:31_

---
