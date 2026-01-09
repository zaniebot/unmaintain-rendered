---
number: 5522
title: "feat: remove the old uv binary when another one is installed"
type: issue
state: closed
author: ZeyadMoustafaKamal
labels: []
assignees: []
created_at: 2024-07-28T19:32:22Z
updated_at: 2024-07-29T16:58:40Z
url: https://github.com/astral-sh/uv/issues/5522
synced_at: 2026-01-07T13:12:17-06:00
---

# feat: remove the old uv binary when another one is installed

---

_Issue opened by @ZeyadMoustafaKamal on 2024-07-28 19:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
So this should be an easy one. I remember that I have installed `uv` using `pipx` in my arch linux which will install `uv` in `~/.local/bin` and then used the bash script to install the latest version but because of the placement of `~/.local/bin` in the `PATH` env variable I still got the old one when I executed `uv --version` and I had to remove it manually. One of the things you might do is to search for `uv` using the `which` command and the equivalent for other platforms as well and remove the binary.

---

_Comment by @charliermarsh on 2024-07-28 23:25_

It's a little dubious for an installer to remove binaries that it didn't install. But maybe there's a world where we warn if we find a `uv` earlier in PATH after installing via the `curl` installers?

---

_Comment by @ZeyadMoustafaKamal on 2024-07-29 16:58_

Fine. I think that this will be good enough.

---

_Closed by @ZeyadMoustafaKamal on 2024-07-29 16:58_

---
