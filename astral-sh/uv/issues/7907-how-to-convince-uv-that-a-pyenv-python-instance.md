```yaml
number: 7907
title: How to convince uv that a pyenv Python instance is a venv?
type: issue
state: closed
author: MichaIng
labels:
  - question
  - great writeup
assignees: []
created_at: 2024-10-03T19:14:54Z
updated_at: 2024-10-03T21:34:40Z
url: https://github.com/astral-sh/uv/issues/7907
synced_at: 2026-01-12T15:59:17Z
```

# How to convince uv that a pyenv Python instance is a venv?

---

_@MichaIng_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

We are a Debian-based distribution which offers a bare-metal Home Assistant Core installation option. Home Assistant does not only have a lot of specific dependencies, but also a relatively strict Python version requirement, so that oldstable and at the end of release cycle even stable Debian version do not natively support it anymore. We hence use `pyenv` to compile a specific compatible Python version, to install Home Assistant into. And we do not use a `virtualenv`, since the whole `pyenv` instance is exclusive for Home Assistant already.

Home Assistant installs further Python modules internally, based on the smart home integrations installed with its UI. Its last version switched from `pip` to `uv pip` for this, which broke our instances with
```
error: No virtual environment found; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment
```
Since the `uv` calls are done within HA code, we have no way to pass `--system`. From #1374, https://github.com/astral-sh/uv/issues/1374#issuecomment-1950266614 in particular, I got that setting `VIRTUAL_ENV` should work, however, for the `pyenv` Python instance it somehow does not seem to work. I checked that that variable is passed through, but it seems to be ignored.

https://github.com/astral-sh/uv/blob/main/docs/pip/environments.md mentions that it is ignored if the directory does not comply with [PEP-405](https://peps.python.org/pep-0405/#specification). But I am not sure in how for this is the case, as `pyenv` basically provides complete functional Python instances, just like `/usr/local` in the linked issue where setting the variable worked for a container setup:
```console
homeassistant@VM-Bookworm:~$ l /mnt/dietpi_userdata/homeassistant/deps/
total 16K
drwxr-xr-x 3 homeassistant homeassistant 4.0K Oct  3 19:24 bin
drwxr-xr-x 3 homeassistant homeassistant 4.0K Oct  3 19:22 include
drwxr-xr-x 4 homeassistant homeassistant 4.0K Oct  3 19:22 lib
drwxr-xr-x 3 homeassistant homeassistant 4.0K Oct  3 19:22 share
homeassistant@VM-Bookworm:~$ l /mnt/dietpi_userdata/homeassistant/deps/bin/python*
lrwxrwxrwx 1 homeassistant homeassistant   10 Oct  3 19:22 /mnt/dietpi_userdata/homeassistant/deps/bin/python -> python3.12
lrwxrwxrwx 1 homeassistant homeassistant   17 Oct  3 19:22 /mnt/dietpi_userdata/homeassistant/deps/bin/python-config -> python3.12-config
lrwxrwxrwx 1 homeassistant homeassistant   10 Oct  3 19:22 /mnt/dietpi_userdata/homeassistant/deps/bin/python3 -> python3.12
lrwxrwxrwx 1 homeassistant homeassistant   17 Oct  3 19:22 /mnt/dietpi_userdata/homeassistant/deps/bin/python3-config -> python3.12-config
-rwxr-xr-x 1 homeassistant homeassistant  18K Oct  3 19:22 /mnt/dietpi_userdata/homeassistant/deps/bin/python3.12
-rwxr-xr-x 1 homeassistant homeassistant 3.3K Oct  3 19:22 /mnt/dietpi_userdata/homeassistant/deps/bin/python3.12-config
-rwxr-xr-x 1 homeassistant homeassistant  70K Oct  3 19:22 /mnt/dietpi_userdata/homeassistant/deps/bin/python3.12-gdb.py
homeassistant@VM-Bookworm:~$ l -d /mnt/dietpi_userdata/homeassistant/deps/lib/python3.12/site-packages/
drwxr-xr-x 188 homeassistant homeassistant 12K Oct  3 20:00 /mnt/dietpi_userdata/homeassistant/deps/lib/python3.12/site-packages/
```
Is there a way to convince `uv` that this is a virtualenv, without adding the actual unnecessary (in our case) virtualenv layer on top of the pyenv?

---

_Comment by @zanieb on 2024-10-03 19:35_

Hi! You can set `UV_SYSTEM_PYTHON=1` as an equivalent to `--system`.

We check if a given interpreter is a compliant virtual environment with

https://github.com/astral-sh/uv/blob/f0659e76cf215480d30d554118961c7c59ab3e2b/crates/uv-python/src/interpreter.rs#L201-L207

And the filtering of discovered interpreters happens at

https://github.com/astral-sh/uv/blob/891c91dd3a0141f05adea20674f3ea7f4477c510/crates/uv-python/src/discovery.rs#L609-L657

You can also set `UV_PYTHON` to the full path to the Python interpreter, i.e., `which python`, and we'll allow mutating a system environment without opt-in.

---

_Label `great writeup` added by @zanieb on 2024-10-03 19:37_

---

_Label `question` added by @zanieb on 2024-10-03 19:37_

---

_Comment by @MichaIng on 2024-10-03 19:52_

Okay, so the path of the virtualenv must differ from the system Python path, which is not the case in our setup, as the `pyenv` instance is interpreted as system instance. And if they are the same, `VIRTUAL_ENV` is ignored?

`UV_SYSTEM_PYTHON=1` works perfectly, many thanks 👍!

---

_Comment by @zanieb on 2024-10-03 19:58_

`VIRTUAL_ENV` isn't ignored, i.e., we'll read it when discovering interpreters but you can't pass an environment that isn't actually a virtual environment using that variable. But yeah, in this case it's "ignored" in the sense that it's not a compliant virtual environment and it is filtered from consideration.

---

_Assigned to @zanieb by @zanieb on 2024-10-03 19:58_

---

_Comment by @MichaIng on 2024-10-03 20:01_

I then wonder how it did work work in case of this container setup: https://github.com/astral-sh/uv/issues/1374#issuecomment-1950266614

However, generally it makes sense, and `UV_SYSTEM_PYTHON=1` is the logical explicit solution, when args cannot be passed. I consider this as solved/answered 🙂.

---

_Closed by @MichaIng on 2024-10-03 20:01_

---

_Comment by @InnovoDeveloper on 2024-10-03 20:08_

how do we apply the fix?  or is this coming in a patch?  

---

_Comment by @MichaIng on 2024-10-03 20:08_

@InnovoDeveloper 
Please see my last comment on the issue at our GitHub repo.

---

_Comment by @InnovoDeveloper on 2024-10-03 20:10_

sorry, had multiple windows open and looked at wrong window.  Cheers!

---

_Comment by @zanieb on 2024-10-03 21:34_

@MichaIng that's before we started enforcing this check in 0.2.0 e.g., here are some discussions following that change:

- https://github.com/astral-sh/uv/issues/3905
- https://github.com/astral-sh/uv/issues/3765

---
