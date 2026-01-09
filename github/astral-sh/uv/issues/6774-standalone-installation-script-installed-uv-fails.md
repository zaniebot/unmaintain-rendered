---
number: 6774
title: "Standalone installation script installed `uv` fails to `uv self update`"
type: issue
state: closed
author: scienceplease
labels:
  - bug
  - releases
assignees: []
created_at: 2024-08-28T20:54:40Z
updated_at: 2024-12-04T01:26:34Z
url: https://github.com/astral-sh/uv/issues/6774
synced_at: 2026-01-07T13:12:17-06:00
---

# Standalone installation script installed `uv` fails to `uv self update`

---

_Issue opened by @scienceplease on 2024-08-28 20:54_

Recent versions of `uv` have not been able to `uv self update` despite being installed via the standalone installation script. I have not been able to pinpoint the issue to anything in particular on my system. All relevant information below:

```bash
cat /etc/redhat-release 
# Fedora release 40 (Forty)

env | grep CARGO_HOME
# CARGO_HOME=/home/username/.local/opt/rust/cargo

curl -LsSf https://astral.sh/uv/install.sh | sh
# downloading uv 0.4.0 x86_64-unknown-linux-gnu
# installing to /home/username/.local/opt/rust/cargo/bin
#  uv
#  uvx
# everything's installed!
#
# To add /home/username/.local/opt/rust/cargo/bin to your PATH, either restart your shell or run:
#
#    source /home/username/.local/opt/rust/cargo/env (sh, bash, zsh)
#    source /home/username/.local/opt/rust/cargo/env.fish (fish)

which uv
# ~/.local/opt/rust/cargo/bin/uv

uv --version
# uv 0.4.0

uv self update
# warning: Self-update is only available for uv binaries installed via the standalone installation scripts.
#
# If you installed uv with pip, brew, or another package manager, update uv with `pip install # --upgrade`, `brew upgrade`, or similar.
```

---

_Comment by @telljmc on 2024-09-03 22:52_

I am encountering a similar issue on a Macbook Pro M3.  I installed uv using the curl command.  When I attempt to update I encounter the following:

```
❯ uv self update
info: Checking for updates...
error: The installation failed. Output from the installer:
downloading uv 0.4.3 aarch64-apple-darwin
```
I tried the "--verbose" flag which resulted in exactly the same message.  I tried closing everything and rebooting with the same result.  Next up will be to do an uninstall and fresh reinstall.



---

_Comment by @zanieb on 2024-09-03 22:54_

Hm. I'm on a M3 as well and can't reproduce that.

---

_Label `bug` added by @zanieb on 2024-09-03 22:55_

---

_Label `release` added by @zanieb on 2024-09-03 22:55_

---

_Comment by @telljmc on 2024-09-05 17:23_

> I am encountering a similar issue on a Macbook Pro M3. I installed uv using the curl command. When I attempt to update I encounter the following:
> 
> ```
> ❯ uv self update
> info: Checking for updates...
> error: The installation failed. Output from the installer:
> downloading uv 0.4.3 aarch64-apple-darwin
> ```
> 
> I tried the "--verbose" flag which resulted in exactly the same message. I tried closing everything and rebooting with the same result. Next up will be to do an uninstall and fresh reinstall.

To update, I finally had to uninstall uv 0.3.5 and then install uv 0.4.3.  Today I tried to update to uv 0.4.5 and I am running into the same issue as previous:

```
❯ uv self update
info: Checking for updates...
error: The installation failed. Output from the installer:
downloading uv 0.4.5 aarch64-apple-darwin
```




---

_Comment by @lucas-labs on 2024-10-27 02:06_

I'm having the same issue in Windows (installed via standalone script).

```
$ uv self update

warning: Self-update is only available for uv binaries installed via the
standalone installation scripts.

If you installed uv with pip, brew, or another package manager, update uv with
`pip install --upgrade`, `brew upgrade`, or similar.
```


#### [EDIT]

I managed to run self update successfully thanks to a comment here https://github.com/astral-sh/uv/issues/6417.

I was using `cache-dir` to change the cache directory to another drive. Apparently I also removed the entire `%LOCALAPPDATA%\uv` directory after doing so, instead of just removing `%LOCALAPPDATA%\uv\cache`. Therefore, I had removed `%LOCALAPPDATA%\uv\uv-receipt.json` file in the process (running `uv self update -v` gave me the hint... it couldn't find the receipt). I mannually created the receipt file and it all started working again.

---

_Comment by @leehanchung on 2024-11-27 22:59_

Got the same problem.

```bash
$ uv self update
warning: Self-update is only available for `uv` binaries installed via the standalone installation scripts.

If you installed `uv` with `pip`, `brew`, or another package manager, update `uv` with `pip install --upgrade`, `brew upgrade`, or similar.

$ uv self update -v
DEBUG receipt is not for this executable; assuming `uv` was installed via a package manager
warning: Self-update is only available for `uv` binaries installed via the standalone installation scripts.

If you installed `uv` with `pip`, `brew`, or another package manager, update `uv` with `pip install --upgrade`, `brew upgrade`, or similar.
han@VitaminC:~/courses/cs194-llm-agents$ uv cache prune
Pruning cache at: /home/han/.cache/uv
Removed 9 files (3.2MiB)
$ uv self update -v
DEBUG receipt is not for this executable; assuming `uv` was installed via a package manager
warning: Self-update is only available for `uv` binaries installed via the standalone installation scripts.

If you installed `uv` with `pip`, `brew`, or another package manager, update `uv` with `pip install --upgrade`, `brew upgrade`, or similar.
```

Seems like its something related to `receipt`?

---

_Comment by @zanieb on 2024-11-27 23:12_

@leehanchung Generally that means that some _other_ uv on the system was installed after the one that you're using. What does `where uv` output?

---

_Comment by @leehanchung on 2024-11-27 23:27_

> [@leehanchung](https://github.com/leehanchung) Generally that means that some _other_ uv on the system was installed after the one that you're using. What does `where uv` output?

found the issue. i had the old `~/.cargo/bin/uv`. removed and working now.

thanks!!

---

_Referenced in [axodotdev/axoupdater#221](../../axodotdev/axoupdater/pulls/221.md) on 2024-11-28 00:01_

---

_Referenced in [astral-sh/uv#9487](../../astral-sh/uv/pulls/9487.md) on 2024-11-28 00:03_

---

_Closed by @zanieb on 2024-12-04 01:26_

---

_Referenced in [astral-sh/uv#13672](../../astral-sh/uv/issues/13672.md) on 2025-05-28 11:20_

---
