---
number: 8608
title: "Empty `UV_TOOL_DIR` and `UV_TOOL_DIR_BIN` environment variables treated differently"
type: issue
state: closed
author: NathanVaughn
labels:
  - configuration
assignees: []
created_at: 2024-10-27T17:23:52Z
updated_at: 2025-04-21T19:15:04Z
url: https://github.com/astral-sh/uv/issues/8608
synced_at: 2026-01-07T13:12:18-06:00
---

# Empty `UV_TOOL_DIR` and `UV_TOOL_DIR_BIN` environment variables treated differently

---

_Issue opened by @NathanVaughn on 2024-10-27 17:23_

Hello,

I noticed that `uv` treats the environment variables `UV_TOOL_DIR` and `UV_TOOL_BIN_DIR` differently when they exist, but are set to blank values. I was trying to make a devcontainer with some tools pre-installed by uv in a different directory, so that the user's home directory could be cached separately as a mounted volume. Simplified Dockerfile:

```dockerfile
FROM ghcr.io/astral-sh/uv:0.4.27-python3.12-bookworm

ENV UV_TOOL_DIR=/opt/uv/tools
ENV UV_TOOL_BIN_DIR=/opt/uv/tools/bin

ENV PATH="$PATH:/root/.local/bin:/opt/uv/tools/bin"

RUN uv tool install cowsay

ENV UV_TOOL_DIR=""
ENV UV_TOOL_BIN_DIR=""
```

Running `unset UV_TOOL_DIR` doesn't do anything in this context (see [this thread](https://stackoverflow.com/a/57189273/9944427)), so I set the environment variables to a blank value instead.

Now, running the following commands get this output:

```bash
docker build -t test .

echo "Tool dir"
docker run --rm test uv tool dir
echo "Tool bin dir"
docker run --rm test uv tool dir --bin
```

```bash
$ ./test.sh 
Tool dir

Tool bin dir
/root/.local/bin
```

With `uv tool dir` returning a blank value, installing or running a tool now installs it in the current directory. However, `uv tool dir --bin` does what I would expect.

I believe this is a bug, and `UV_TOOL_DIR` set to a blank value should be treated as unset, and default to the fallback options like `$HOME/.local/share/uv/tools` on UNIX, as described at <https://docs.astral.sh/uv/reference/cli/#uv-tool-dir>

---

_Comment by @gaby on 2024-10-27 23:22_

@NathanVaughn Do this instead:

```Dockerfile
ENV UV_TOOL_DIR=
ENV UV_TOOL_BIN_DIR=
```

---

_Comment by @NathanVaughn on 2024-10-27 23:25_

I tried that as well and it has the same result. The easy fix is to just redefine it back to the default like

```dockerfile
ENV UV_TOOL_DIR=/root/.local/share/uv/tools
ENV UV_TOOL_BIN_DIR=/root/.local/bin
```

---

_Comment by @gaby on 2024-10-27 23:26_

Sounds like a bug to me then

---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-10-28 16:23_

---

_Label `configuration` added by @charliermarsh on 2024-10-28 16:23_

---

_Removed from milestone `v0.5.0` by @zanieb on 2024-11-25 20:24_

---

_Added to milestone `v0.6.0` by @zanieb on 2024-11-25 20:24_

---

_Removed from milestone `v0.6.0` by @zanieb on 2025-02-15 02:34_

---

_Added to milestone `v0.7.0` by @zanieb on 2025-02-15 02:34_

---

_Referenced in [astral-sh/uv#12905](../../astral-sh/uv/pulls/12905.md) on 2025-04-15 20:46_

---

_Comment by @zanieb on 2025-04-21 19:15_

Will be fixed in #12843 

---

_Closed by @zanieb on 2025-04-21 19:15_

---
