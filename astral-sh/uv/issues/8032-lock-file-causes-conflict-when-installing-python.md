---
number: 8032
title: .lock file causes conflict when installing python into a shared directory
type: issue
state: open
author: kwaegel
labels:
  - bug
  - great writeup
assignees: []
created_at: 2024-10-09T03:28:18Z
updated_at: 2025-02-20T15:08:05Z
url: https://github.com/astral-sh/uv/issues/8032
synced_at: 2026-01-10T01:24:22Z
---

# .lock file causes conflict when installing python into a shared directory

---

_Issue opened by @kwaegel on 2024-10-09 03:28_

I'm having trouble getting installing Python builds (and tools) into a directory shared by multiple users, due to a single-user owned `.lock` file created in `UV_PYTHON_INSTALL_DIR`. I'm not sure if what I am attempting is reasonable or not, but at the moment I can't tell if this is a bug or intended behaviour.

My setup is roughly following the suggestion from [this prior issue](https://github.com/astral-sh/uv/issues/6067).

Background: I'm trying to create a Dockerfile that's used in a CI environment. The Dockerfile itself runs all the setup commands as `root`, but the Jenkins bot uses the user `ubuntu`. I'm attempting to install everything in a shared directory `/opt/uv/*`, allowing either user to invoke `uv python install ...` or `uv tool install ...` commands.

Minimal Dockerfile replicating this issue:
```
FROM ubuntu:24.04 AS build
COPY --from=ghcr.io/astral-sh/uv:0.4.20 /uv /bin/uv
ENV UV_PYTHON_INSTALL_DIR="/opt/uv/python"
RUN uv python install 3.10
USER ubuntu
RUN uv python install 3.11
```
Error message:
```
#9 [build 4/4] RUN uv python install 3.11
#9 0.248 error: failed to create file `/opt/uv/python/.lock`
#9 0.248   Caused by: Permission denied (os error 13)
#9 ERROR: process "/bin/sh -c uv python install 3.11" did not complete successfully: exit code: 2
```

The situation for installing tools is analogous , with an owned `.lock` file created in `UV_TOOL_DIR`.

My current somewhat awkward workaround is to set the `setgid` bit on the shared folder and add `umask 002` before each command, which makes the `.lock` file writable by all group members. The downside is that it's easy to forget to include the umask prefix on every command that needs it.
```
FROM ubuntu:24.04 AS build
COPY --from=ghcr.io/astral-sh/uv:0.4.20 /uv /bin/uv
ENV UV_PYTHON_INSTALL_DIR="/opt/uv/python"
RUN mkdir -p "/opt/uv" && \
    chgrp ubuntu "/opt/uv" && \
    chmod g+s "/opt/uv"
RUN umask 002 && uv python install 3.10
USER ubuntu
RUN umask 002 && uv python install 3.11
```

---

_Label `bug` added by @zanieb on 2024-10-09 04:01_

---

_Comment by @zanieb on 2024-10-09 04:01_

Interesting. I'm not sure what we can do to fix this, but it seems worth trying to.

---

_Label `great writeup` added by @zanieb on 2024-10-09 04:02_

---

_Comment by @kwaegel on 2024-10-09 04:17_

I'm not entirely sure what the `.lock` file is used for, but I had initially assumed it would work something like:
* Create the lock file to get exclusive access to the python (or tools) directory.
* Preform any add/remove operations.
* Remove the lock file.

The lock file persisting between write operations is one thing that confuses me, so maybe it's doing more than I had assumed?

---

_Comment by @zanieb on 2024-10-09 14:03_

Oh it shouldn't persist across operations, afaik. Yes it's a file used for exclusive access, e.g.,

https://github.com/astral-sh/uv/blob/f0659e76cf215480d30d554118961c7c59ab3e2b/crates/uv-tool/src/lib.rs#L142-L145

---

_Comment by @charliermarsh on 2024-10-09 14:49_

I think It might need to persist across operations... Can a lock holder _know_ that's safe to delete? Other processes may be waiting for it.

---

_Comment by @zanieb on 2024-10-09 16:02_

Yeah I think you're right. Do we need to create the file with different permissions when we're in a globally writable directory?

---

_Comment by @kwaegel on 2024-10-09 17:25_

I'm curious why it would need to persist across operations. Wouldn't most operations be a lock/write/unlock cycle? If that was the case, the lock would only ever be created and removed by the same user. That might only be true for adding Python versions, though. Removing versions might be more problematic, but that's not something my CI images need to deal with.

Another consequence of the current state is that `uv python install x.y.z` fails even if x.y.z is installed, since it tries to acquire the lock before checking for an existing instillation. I can do a manual workaround like below, but it feels like this should be automatic (pyenv uses a `--skip-existing` flag for similar behaviour).
```
uv python find $(PYTHON_VERSION) || uv python install $(PYTHON_VERSION)
```

---

_Comment by @charliermarsh on 2024-10-09 17:33_

The purpose of the lock is to prevent shared modification of a virtual environment across two concurrent processes. I don't think the writer can remove it after unlock -- what if another writer is waiting to access it? I actually have no idea what happens in that case.

Should we just be writing it with more open permissions?

---

_Comment by @kwaegel on 2024-10-09 17:42_

For my purposes, open permissions (when globally writable, or when the setgid bit is enabled) would resolve the issue, since I'm doing multi-user but not concurrent writes, but I'm not sure if that's safe in the general case.

---

_Comment by @akmalsoliev on 2024-11-10 23:00_

Hello, are there any updates on this issue? 
I'm more running into an issue of inability to `uv pip install` inside of an Airflow with a nested venv, since I don't want to dump everything onto a system.
This works perfectly fine on macOS, but fails on Linux. 
NOTE: Airflow python version matches sub-venv one.
```dockerfile
FROM apache/airflow:2.10.3-python3.11

USER airflow

COPY ./requirements.txt requirements.txt
RUN uv pip install --no-cache-dir -r ./requirements.txt
```


---

_Comment by @guoci on 2024-12-28 19:01_

Can the locking implementation be changed to check the existence of a `.lock` file? Then, the `.lock` file must be deleted when unlocking.

If a `uv` process abnormally terminates without unlocking, subsequent runs may cause a deadlock. If that happens the Python environment is possibly broken and needs to be set up again anyway.

---

_Referenced in [astral-sh/uv#10265](../../astral-sh/uv/pulls/10265.md) on 2025-01-01 20:41_

---

_Comment by @BurntSushi on 2025-01-06 18:41_

Have folks tried setting `umask` yourself outside of uv? Does that work? If it does, then I kinda feel like that's the most appropriate solution here. Because you're fundamentally working in a shared environment, and it seems sensible to set a `umask` that supports that environment. (This doesn't address the "I want to run multiple `uv` processes simultaneously on the same environment" use case, but rather, "I want to run multiple serial `uv` processes under different Unix users on the same environment.")

(Also, to address some confusion around the lock file above, there are many different implementations of a "lock file." The simplest is using the existence of a file itself as the lock itself. But unlocking might not always work, leading to "user, please delete lock file or remove enitre environment" style of messages. Beyond that, there are a variety of advisory locking schemes available on Unix systems that don't use the _existence_ of files themselves as the implementation of locking. Instead, operating system manages locking/unlocking as a special primitive, and this in turn means unlocking always succeeds even when the process terminates unceremoniously. We use the style of advisory locks called BSD flocks.)

EDIT: I see that `umask` is mentioned in the OP. Whoops, sorry about that, I missed it. While awkward, I feel like that might be right solution here?

---

_Comment by @kwaegel on 2025-01-06 18:53_

I have and it works (I mentioned a `setgid` + `umask` workaround in my original bug report), but it tends to be a bit brittle (I have to remember to prepend it to every relevant command) and mildly surprising (it's not required for [pipx install --global ...](https://pipx.pypa.io/stable/installation/#-global-argument) commands).

I'd also call it a bit obscure...but that might reflect more on my experience with Linux administration than the command itself.

If this does end up being the recommended solution, could we put it in the docs somewhere? Either the [Docker integration](https://docs.astral.sh/uv/guides/integration/docker/) section or some more general "multi-user configuration" section would be helpful.

(Automatic handling or sort of `--global` flag would be ideal, but a documented workaround is fine if the implementation would be problematic.)

---

_Comment by @guoci on 2025-01-06 19:04_

@kwaegel 
I tested using `umask 000` instead of `umask 002` so that `chgrp` and `chmod` is not needed.

---

_Comment by @romaingyh on 2025-02-20 15:08_

https://github.com/astral-sh/uv/pull/11328 fixed the issue for lock files but we still need umask for temp folders @kwaegel @guoci 

```
STEP 3/6: ENV UV_PYTHON_INSTALL_DIR="/opt/uv/python"
--> 64ef12f3ab56
STEP 4/6: RUN uv python install 3.10
Downloading cpython-3.10.16-linux-x86_64-gnu (19.8MiB)
 Downloaded cpython-3.10.16-linux-x86_64-gnu
Installed Python 3.10.16 in 1.73s
 + cpython-3.10.16-linux-x86_64-gnu
--> ab08b7600094
STEP 5/6: USER ubuntu
--> 45b98a559415
STEP 6/6: RUN uv python install 3.11
Downloading cpython-3.11.11-linux-x86_64-gnu (20.5MiB)
error: Failed to install cpython-3.11.11-linux-x86_64-gnu
  Caused by: Failed to create download directory
  Caused by: Permission denied (os error 13) at path "/opt/uv/python/.temp/.tmp9rD0mb"
Error: building at STEP "RUN uv python install 3.11": while running runtime: exit status 1
```

---
