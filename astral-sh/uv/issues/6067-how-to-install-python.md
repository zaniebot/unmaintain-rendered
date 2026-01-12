```yaml
number: 6067
title: How to install Python 
type: issue
state: closed
author: FCamborda
labels:
  - question
assignees: []
created_at: 2024-08-13T20:50:12Z
updated_at: 2025-04-03T13:10:59Z
url: https://github.com/astral-sh/uv/issues/6067
synced_at: 2026-01-12T15:59:01Z
```

# How to install Python 

---

_@FCamborda_

First and foremost thank you so much for the development of uv (and all other rust-based tools).
I did not find a  "discussion", hence why I'm creating an issue instead.

I'm wondering if there is a way to install a Python interpreter for all users in a system. We're using the latest uv `0.2.34` and locally installing a Python interpreter in `<data dir>/uv/python/<distribution>/bin/python3`, irrespective on what `--python-preference` does.

That is, all following commands use the same directory
`uv python install 3.9`
`uv python install 3.9 --python-preference system`
`uv python install 3.9 --python-preference <option>`

The background for this is that I'm trying to setup two Docker images with uv.
The first Docker image has a 'common' user and an installation. I'm taking advantage of the undocumented env. variable `CARGO_HOME`, but I believe this is not a problem (see https://github.com/astral-sh/uv/pull/5455)

```shell
# Production Docker image

# Install uv
ENV CARGO_HOME=/.cargo/
ADD --chmod=755 https://astral.sh/uv/install.sh /install.sh
RUN /install.sh && rm /install.sh

# Python setup
RUN uv python install ${PYVERSION}

# Python venv
...
```

The other Docker image is used for local development (i.e. devcontainer) and has to setup a new user matching the local UID and GID (due to permissions in the file system, etc). Ideally, it should look something like this.
```shell
# Devcontainer
from production:image
# Setup new user
...
# Workaround
RUN uv python install --python $(uv python find ${PYVERSION}
# Setup another venv
...
```

However since the first image installed Python in the data dir of the other user, the second image will not work. As a workaround and since the Python installations are so small, I have just done a second installation of the Python same interpreter, but it would be neat to have a single installation system-wide.

Is this something that makes sense as a feature request/already in the road map? Or maybe it is already supported and I just misread the documentation?

---

_Comment by @zanieb on 2024-08-13 22:12_

Hi! You should be able to set `UV_PYTHON_INSTALL_DIR` to a shared directory.

---

_Label `question` added by @zanieb on 2024-08-13 22:12_

---

_Comment by @FCamborda on 2024-08-15 13:36_

This worked flawlessly, thanks!
Are there plans of making this a CLI option? Also, should this variable be documented [here](https://github.com/astral-sh/uv?tab=readme-ov-file#environment-variables)?

---

_Comment by @zanieb on 2024-08-15 13:59_

It's not in the README because it's a preview feature — it'll be included in our new documentation.

I don't think we'll make it a CLI option, you'd have to pass it to every command. We might make it available in the `uv.toml` file though.

---

_Comment by @FCamborda on 2024-08-16 07:32_

Terrific, thanks for your support!

---

_Closed by @FCamborda on 2024-08-16 07:34_

---

_Comment by @FCamborda on 2024-08-19 06:59_

I have two more similar cases when using `uv tool`, hence reopening this issue.

@zanieb I see there is already an environment variable `UV_TOOL_DIR` to set where uv will place the managed tools. Is there a possibility to add an equivalent environment variable for where the binaries reside? So far I see you are relying on [standard directories](https://github.com/astral-sh/uv/blob/15dfb660ab7f1ccca7e57c985968a18a85bb1d86/crates/uv-tool/src/lib.rs#L352), but for system-wide installations I would prefer not to change/set these env. variables, as it could have implications for other tools. Is it possible to add something like `UV_TOOL_INSTALLATION_DIR` ?

I think Pipx defaults this to `/usr/local/bin`, and we would like to achieve a similar effect.

---

_Reopened by @FCamborda on 2024-08-19 06:59_

---

_Comment by @zanieb on 2024-08-19 14:18_

Sure, we can add support for that (`UV_TOOL_INSTALL_DIR`) to `uv-tool::find_executable_directory`.

---

_Comment by @zanieb on 2024-08-19 14:42_

See https://github.com/astral-sh/uv/pull/6207

---

_Closed by @zanieb on 2024-08-19 16:13_

---

_Comment by @hipertracker on 2024-10-21 14:55_

`asdf global python 3.13.0`, simple. it is weird uv cannot do that

---

_Comment by @punkpeye on 2024-12-30 19:15_

```
ENV UV_TOOL_BIN_DIR="/home/smith/.local/bin"
ENV UV_PYTHON_INSTALL_DIR="/home/smith/.local/bin"
RUN curl -LsSf https://astral.sh/uv/install.sh | sh
RUN uv python install --verbose 3.11
RUN python --version
```

I tried all of this, but still getting python not found. ideas?

---

_Comment by @zanieb on 2024-12-30 19:34_

@punkpeye you're looking for `uv python install --preview 3.11`

See the "Important" note in https://docs.astral.sh/uv/guides/install-python/#getting-started and the documentation in https://docs.astral.sh/uv/concepts/python-versions/#installing-python-executables

---

_Comment by @punkpeye on 2024-12-30 19:55_

Thank you. For others reading this, I was also wanting `--default`.

---

_Comment by @HernandoR on 2025-02-04 01:27_

after `uv python install 3.10 --default --preview` when i want `uv pip install jupyter-core --system` it throws 
``` 
Using Python 3.10.16 environment at: /usr/local/share/uv/python/cpython-3.10.16-linux-x86_64-gnu
error: The interpreter at /usr/local/share/uv/python/cpython-3.10.16-linux-x86_64-gnu is externally managed, and indicates the following:

  This Python installation is managed by uv and should not be modified.

Consider creating a virtual environment with `uv venv`.
``` 

I'm preparing a docker image, so it is intended to install Jupyter (and it's extensions) system wide

---

_Comment by @zanieb on 2025-02-04 02:48_

Yeah, sorry about that — I need to fix a bug where the installed managed Python versions are picked up by the `--system` flag.

---

_Comment by @sti1l-0 on 2025-04-03 07:47_

> after `uv python install 3.10 --default --preview` when i want `uv pip install jupyter-core --system` it throws
> 
> ```
> Using Python 3.10.16 environment at: /usr/local/share/uv/python/cpython-3.10.16-linux-x86_64-gnu
> error: The interpreter at /usr/local/share/uv/python/cpython-3.10.16-linux-x86_64-gnu is externally managed, and indicates the following:
> 
>   This Python installation is managed by uv and should not be modified.
> 
> Consider creating a virtual environment with `uv venv`.
> ```
> 
> I'm preparing a docker image, so it is intended to install Jupyter (and it's extensions) system wide

same issue, how do you fix it?

---

_Comment by @zanieb on 2025-04-03 13:10_

Pass `--python` to request the specific system interpreter you want to target. https://github.com/astral-sh/uv/pull/7934 tracks the fix.

---
