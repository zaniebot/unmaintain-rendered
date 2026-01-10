```yaml
number: 8947
title: Uv fails to activate environment in folder with apostrophe
type: issue
state: closed
author: mak448a
labels:
  - bug
assignees: []
created_at: 2024-11-08T15:40:58Z
updated_at: 2024-11-15T09:52:11Z
url: https://github.com/astral-sh/uv/issues/8947
synced_at: 2026-01-10T04:36:20Z
```

# Uv fails to activate environment in folder with apostrophe

---

_Issue opened by @mak448a on 2024-11-08 15:40_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I tried these commands in a folder with an apostrophe and a space.
```shell
uv venv
source .venv/bin/activate
```
Version: 0.4.25
Installation Method: Dnf package 
OS: Fedora Linux 41 KDE Spin

---

_Comment by @konstin on 2024-11-08 15:41_

What operating system are you on, and what's the name of the folder verbatim?

---

_Comment by @mak448a on 2024-11-08 15:45_

Fedora Linux 41. `Testing's`. I think it's the apostrophe.

---

_Renamed from "Uv fails to activate environment in folder with spaces (maybe it's because of apostrophes?" to "Uv fails to activate environment in folder with apostrophe" by @mak448a on 2024-11-08 15:47_

---

_Label `bug` added by @konstin on 2024-11-08 16:51_

---

_Comment by @konstin on 2024-11-08 17:20_

Documenting the background on shell escaping:

We want our `activate` script to support basically any posix shell. There's two kind of quotes: Single and double quotes. In bash, single quotes must not contain another single quote, you can't even escape it (https://linux.die.net/man/1/bash under "QUOTING"). Double quotes have escaping rules different from shell to shell, which we can't do. bash has `$'\''`, but that's not universal enough.

As a workaround, we can go through printf:

```
VIRTUAL_ENV=$(printf '/home/konsti/projects/uv/Testing\047s/.venv')
```

---

_Comment by @bluss on 2024-11-09 00:00_

You could maybe use what shlex suggests (python stdlib shlex), both `''` and `""` strings.

```python
print(shlex.quote("/home/Testing's/.venv"))
'/home/Testing'"'"'s/.venv'
```

That works in at least bash and dash

---

_Closed by @konstin on 2024-11-15 09:52_

---

_Closed by @konstin on 2024-11-15 09:52_

---
