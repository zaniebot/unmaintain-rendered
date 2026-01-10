---
number: 6802
title: "False positive in warning: uv does not recognize \"~\" in $PATH"
type: issue
state: closed
author: paravoid
labels:
  - bug
assignees: []
created_at: 2024-08-29T10:45:50Z
updated_at: 2024-08-29T21:24:00Z
url: https://github.com/astral-sh/uv/issues/6802
synced_at: 2026-01-10T01:24:06Z
---

# False positive in warning: uv does not recognize "~" in $PATH

---

_Issue opened by @paravoid on 2024-08-29 10:45_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

uv warns about `/home/$user/.local/bin` not being in my PATH, although `~/.local/bin` is, i.e. not recognizing the tilde.

Demo:
```
$ uv --version
uv 0.4.0

$ echo $SHELL
/bin/bash

$ cat /etc/debian_version 
trixie/sid

$ echo $PATH
~/.local/bin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games

$ uv tool install ruff
Resolved 1 package in 7ms
Installed 1 package in 1ms
 + ruff==0.6.2
Installed 1 executable: ruff
warning: `/home/paravoid/.local/bin` is not on your PATH. To use installed tools, run `export PATH="/home/paravoid/.local/bin:$PATH"` or `uv tool update-shell`.

$ which ruff
/home/paravoid/.local/bin/ruff
```

---

_Label `bug` added by @charliermarsh on 2024-08-29 14:46_

---

_Comment by @charliermarsh on 2024-08-29 14:46_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-29 14:50_

---

_Comment by @charliermarsh on 2024-08-29 15:28_

Oddly, this doesn't seem to work for me on macOS. Do you know if support here is OS-specific?

---

_Comment by @paravoid on 2024-08-29 15:37_

I can't imagine this being OS-specific (but I don't know for sure). It may be shell-specific, though: are you using bash or zsh? bash expands the tilde, but zsh does not, AFAIK. 

---

_Comment by @charliermarsh on 2024-08-29 15:50_

Ah yeah, I'm using Zsh. That would explain it.

---

_Referenced in [astral-sh/uv#6829](../../astral-sh/uv/pulls/6829.md) on 2024-08-29 19:36_

---

_Closed by @charliermarsh on 2024-08-29 19:50_

---

_Closed by @charliermarsh on 2024-08-29 19:50_

---

_Comment by @paravoid on 2024-08-29 21:23_

Thanks so much!

---
