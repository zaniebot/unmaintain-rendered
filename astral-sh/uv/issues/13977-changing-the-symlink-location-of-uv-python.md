---
number: 13977
title: "Changing the symlink location of `uv python install --default`"
type: issue
state: closed
author: glin
labels:
  - question
assignees: []
created_at: 2025-06-11T21:39:59Z
updated_at: 2025-06-12T10:02:52Z
url: https://github.com/astral-sh/uv/issues/13977
synced_at: 2026-01-10T01:25:41Z
---

# Changing the symlink location of `uv python install --default`

---

_Issue opened by @glin on 2025-06-11 21:39_

### Question

Hi, I came across `uv python install --default` from https://github.com/astral-sh/uv/pull/8650 and thought it was a nice feature. I want to use uv to install a single global python automatically on the PATH for a Docker image.

I've installed `uv` to `/usr/local/bin` already via:
```sh
curl -LsSf https://astral.sh/uv/install.sh | env UV_INSTALL_DIR="/usr/local/bin" sh
```

However, `uv python install --default` currently does not seem to have a configurable location. It always uses `~/.local/bin` unless there's some env var I haven't found yet. Is it possible to change the location for the default symlinks?

I tried `UV_INSTALL_DIR`, but it is not used here. I know there's also `UV_PYTHON_INSTALL_DIR`, but that serves a different purpose.
```sh
UV_INSTALL_DIR=/usr/local/bin uv python install --default --preview 3.12
```

### Platform

RHEL 8, x86_64

### Version

uv 0.7.12

---

_Label `question` added by @glin on 2025-06-11 21:40_

---

_Comment by @zanieb on 2025-06-11 21:54_

https://docs.astral.sh/uv/reference/environment/#uv_python_bin_dir

Will be covered in #13976 too

---

_Comment by @glin on 2025-06-11 22:06_

@zanieb That's great, thanks for the quick response and additional docs! I could also suggest adding a mention to `uv python install --help`, as that was the first place I looked for docs.

You can close this issue now if you want.

---

_Referenced in [astral-sh/uv#13978](../../astral-sh/uv/pulls/13978.md) on 2025-06-11 22:29_

---

_Referenced in [astral-sh/uv#13979](../../astral-sh/uv/issues/13979.md) on 2025-06-11 22:31_

---

_Closed by @zanieb on 2025-06-12 10:02_

---
