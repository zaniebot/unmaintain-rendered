---
number: 15840
title: Uninterpreted color codes in some error messages
type: issue
state: closed
author: fleetingbytes
labels:
  - bug
assignees: []
created_at: 2025-09-14T15:27:28Z
updated_at: 2025-09-17T14:30:44Z
url: https://github.com/astral-sh/uv/issues/15840
synced_at: 2026-01-07T13:12:19-06:00
---

# Uninterpreted color codes in some error messages

---

_Issue opened by @fleetingbytes on 2025-09-14 15:27_

### Summary

In the issue #15799 (now fixed) @konstin and @eabase noticed that certain error messages contain uninterpreted color code tags.

minimal example of reproducing this on FreeBSD (amd64):
```sh
uv init rebuild
cd rebuild
uv add cffi
uv sync --verbose
```

produces:

```log
DEBUG uv 0.8.17 (21a92c163 2025-09-11)
DEBUG Found project root: `/home/tejul/src/rebuild`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/home/tejul/src/rebuild`
DEBUG Reading Python requests from version file at `/home/tejul/src/rebuild/.python-version`
DEBUG Using Python request `3.11` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.11`
DEBUG Released lock at `/tmp/uv-6ba0532bfdeb457a.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: rebuild @ file:///home/tejul/src/rebuild
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 3 packages in 0.57ms
DEBUG Using request timeout of 30s
DEBUG Platform tags mismatch for cffi==2.0.0: The distribution is compatible with \x1b[36mFreeBSD\x1b[39m (`\x1b[36mfreebsd_14_2_release_amd64\x1b[39m`), but you're on \x1b[36mFreeBSD\x1b[39m (`\x1b[36mfreebsd_14_2_RELEASE_x86_64\x1b[39m`)
```

The lines appear with properly interpreted color. The word `DEBUG` at the start of the line is cyan (or light blue), but the last line about platform tags mismatch has some uninterpreted color codes later on.

The error about the mismatched platform tags has been fixed in #15799, but the issue with the rogue color codes still remains.

 It may be masked by the fix because as far I have seen, this type of message is the only one where it occurs.

Attached is the output of `python -m sysconfig`

[system-python-sysconfig.txt](https://github.com/user-attachments/files/22321299/system-python-sysconfig.txt)


### Platform

FreeBSD 14.2-RELEASE amd64

### Version

uv 0.8.17 (21a92c163 2025-09-11)

### Python version

Python 3.11.13

---

_Label `bug` added by @fleetingbytes on 2025-09-14 15:27_

---

_Referenced in [astral-sh/uv#15799](../../astral-sh/uv/issues/15799.md) on 2025-09-14 15:28_

---

_Comment by @zanieb on 2025-09-14 15:46_

What terminal and shell are you using?

---

_Comment by @fleetingbytes on 2025-09-14 15:55_

Alacrity and zsh. I can give you their exact versions later. I currently don't have access to my machine.

---

_Referenced in [PyO3/maturin#2741](../../PyO3/maturin/pulls/2741.md) on 2025-09-14 16:02_

---

_Comment by @konstin on 2025-09-14 16:03_

Thank you for the sysconfig!

---

_Comment by @fleetingbytes on 2025-09-14 17:40_

Just for completeness:

- the uninterpreted color codes appear also when I use `--color never`.
- alacritty 0.16.0-dev (985def87)
- zsh 5.9 (amd64-portbld-freebsd14.1)

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-14 18:16_

---

_Comment by @charliermarsh on 2025-09-14 18:16_

I actually see the same thing (but it's masked by only being present in `--verbose`):
```
DEBUG Platform tags mismatch for numpy==2.3.3: The distribution is compatible with \x1b[36mmacOS\x1b[39m (`\x1b[36mmacosx_14_0_arm64\x1b[39m`), but you're on \x1b[36mLinux\x1b[39m (`\x1b[36mmanylinux_2_28_x86_64\x1b[39m`)
```

---

_Comment by @charliermarsh on 2025-09-14 18:18_

It looks like a general problem whereby color codes in debug messages get misinterpreted (we rarely use these):
```
DEBUG Identified uncached distribution: \x1b[32mnumpy==2.3.3\x1b[39m
```

---

_Referenced in [astral-sh/uv#15843](../../astral-sh/uv/pulls/15843.md) on 2025-09-14 18:52_

---

_Comment by @charliermarsh on 2025-09-14 18:57_

As far as I can tell, https://github.com/astral-sh/uv/issues/11560 was closed but is still broken:

```
❯ uv python install 3.13 -v --color=never
DEBUG uv 0.8.16 (2de677b0d 2025-09-09)
DEBUG Acquired lock for `/Users/crmarsh/.local/share/uv/python`
DEBUG Found `\x1b[32mcpython-3.13.6-macos-aarch64-none\x1b[39m` for request `\x1b[36m3.13 (cpython-3.13-macos-aarch64-none)\x1b[39m`
DEBUG Using request timeout of 30s
DEBUG Inspecting existing executable at `/Users/crmarsh/.local/bin/python3.13`
DEBUG Executable at `/Users/crmarsh/.local/bin/python3.13` is already for `cpython-3.13.6-macos-aarch64-none`
DEBUG Released lock at `/Users/crmarsh/.local/share/uv/python/.lock`
```

---

_Comment by @charliermarsh on 2025-09-14 19:24_

Ahh, I see... This was changed in https://github.com/tokio-rs/tracing/pull/3368 in response to a security vulnerability. And there doesn't seem to be a way to turn it back on: https://github.com/tokio-rs/tracing/issues/3369. So this likely worked after https://github.com/astral-sh/uv/pull/11604, but "regressed" when we upgraded `tracing-subscriber`.

---

_Referenced in [tokio-rs/tracing#3378](../../tokio-rs/tracing/issues/3378.md) on 2025-09-15 07:26_

---

_Closed by @charliermarsh on 2025-09-17 14:30_

---

_Referenced in [astral-sh/uv#15913](../../astral-sh/uv/issues/15913.md) on 2025-09-17 14:32_

---
