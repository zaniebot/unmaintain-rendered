---
number: 4958
title: "`uv sync` fails to find matching python version with pyenv"
type: issue
state: closed
author: konstin
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-10T08:38:14Z
updated_at: 2024-12-14T13:26:56Z
url: https://github.com/astral-sh/uv/issues/4958
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv sync` fails to find matching python version with pyenv

---

_Issue opened by @konstin on 2024-07-10 08:38_

When using `uv sync`, uv fails to find a python interpreter even if it exists if the default `python`/`python3` are incompatible. This happens e.g. with pyenv.

For example with both `requires-python = "==3.10.*"` `requires-python = ">=3.10"`, where the default `python`/`python3` are any other version, say 3.9:

This fails:
```
uv sync --python-preference only-system
```

This passes:
```
uv sync --python-preference only-system -p 3.10
```

uv should find the python 3.10 interpreter in either case.

## Reproduction

```Dockerfile
FROM ubuntu
ENV DEBIAN_FRONTEND=noninteractive
RUN apt update
RUN apt install -yy build-essential libssl-dev zlib1g-dev \
    libbz2-dev libreadline-dev libsqlite3-dev curl git \
    libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
RUN curl https://pyenv.run | bash
ENV PATH="/root/.pyenv/bin:$PATH"
RUN CONFIGURE_OPTS="--enable-optimizations" pyenv install 3.8.12 3.8.18
RUN CONFIGURE_OPTS="--enable-optimizations" pyenv install 3.9.18
RUN CONFIGURE_OPTS="--enable-optimizations" pyenv install 3.10.13
RUN CONFIGURE_OPTS="--enable-optimizations" pyenv install 3.11.7
RUN CONFIGURE_OPTS="--enable-optimizations" pyenv install 3.12.1
RUN pyenv global 3.8.12 3.8.18 3.9.18 3.10.13 3.11.7 3.12.1
# https://github.com/pyenv/pyenv#set-up-your-shell-environment-for-pyenv
RUN echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc \
    && echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc \
    && echo 'eval "$(pyenv init -)"' >> ~/.bashrc
```

Create a dummy pyproject.toml, any `requires-python` that mismatches the default active python will do:
```toml
[project]
name = "a"
version = "0.1.0"
requires-python = "==3.10.*"
dependencies = ["tqdm"]
```

Compile uv:
```
cargo build --release --target x86_64-unknown-linux-musl
```

Start the docker container, mount the uv dir to `/io` and run commands with
```
/io/target/x86_64-unknown-linux-musl/release/uv sync
```

---

_Label `bug` added by @konstin on 2024-07-10 08:38_

---

_Label `preview` added by @konstin on 2024-07-10 08:38_

---

_Renamed from "`uv sync` fails to find matching python version " to "`uv sync` fails to find matching python version with pyenv" by @konstin on 2024-07-10 08:43_

---

_Comment by @zanieb on 2024-07-10 14:23_

This seems like a duplicate of #4709 right...?

---

_Comment by @konstin on 2024-07-10 14:35_

Ported it over

---

_Closed by @konstin on 2024-07-10 14:35_

---

_Comment by @mhechthz on 2024-12-14 08:08_

is there anything new on this, because is is a major show stopper for using uv in my opinion. even on modifying lock and toml uv sticks with default system version of python. furthermore, a just initialized venv with the right python is removed and replaced by the wrong python.

---

_Comment by @charliermarsh on 2024-12-14 13:26_

Please open a new issue describing your problem — it’s not clear from the above what you’re running into.

---
