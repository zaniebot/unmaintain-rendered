---
number: 3829
title: "`--python` can't be used when `UV_SYSTEM_PYTHON=false`"
type: issue
state: closed
author: thejcannon
labels: []
assignees: []
created_at: 2024-05-24T18:36:49Z
updated_at: 2024-05-24T19:15:18Z
url: https://github.com/astral-sh/uv/issues/3829
synced_at: 2026-01-07T13:12:17-06:00
---

# `--python` can't be used when `UV_SYSTEM_PYTHON=false`

---

_Issue opened by @thejcannon on 2024-05-24 18:36_

👋  I think this is a CLI quirk, but both of this errors for me:

```console
$ UV_SYSTEM_PYTHON=false uv pip install --python .venv/bin/python```

(Our use case is that by default CI uses `UV_SYSTEM_PYTHON` but in certain tests it uses `uv` to make dedicated venvs for the test)

---

_Comment by @thejcannon on 2024-05-24 18:38_

(I assume this is a quirk of the group: https://github.com/astral-sh/uv/blob/ce4d862b0674e782aa936a1f25db627c1ada4177/crates/uv/src/cli.rs#L776)

---

_Renamed from "`--python` can't be used when system python is _disabled_" to "`--python` can't be used when system python is _disabled_ via env var" by @thejcannon on 2024-05-24 18:38_

---

_Comment by @charliermarsh on 2024-05-24 18:41_

Yes there's no way to support this in Clap. We'd have to remove the group IIRC.

---

_Comment by @thejcannon on 2024-05-24 18:41_

Also note that `uv pip install --python .venv/bin/python --no-system` works.

Also just scrubbing the env before calling `uv` works.

---

_Comment by @zanieb on 2024-05-24 18:43_

Ah I recently ran into this at https://github.com/tox-dev/tox-uv/pull/57#issuecomment-2129813665 too

It would be nice to override it...

---

_Comment by @thejcannon on 2024-05-24 18:47_

@epage 👋 Do you think this is something Clap would reasonably support (some kind of "its only a collision if two in the group are "truthy"")? If not, @charliermarsh you can probably just close this issue as not planned. There's an easy workaround.

(I couldn't tell if this fit in any of the existing issues, like https://github.com/clap-rs/clap/issues/5041 or if [`multiple`](https://docs.rs/clap/latest/clap/struct.ArgGroup.html#method.multiple) plus manual validation was the way to go)

---

_Comment by @charliermarsh on 2024-05-24 18:48_

We ran into it before and I think we ended up removing the group.

---

_Renamed from "`--python` can't be used when system python is _disabled_ via env var" to "`--python` can't be used when `UV_SYSTEM_PYTHON=false`" by @thejcannon on 2024-05-24 18:49_

---

_Comment by @charliermarsh on 2024-05-24 18:50_

Actually confused because I thought I fixed this in https://github.com/astral-sh/uv/issues/3000.

---

_Comment by @charliermarsh on 2024-05-24 18:51_

(But clearly I didn't.)

---

_Referenced in [astral-sh/uv#3830](../../astral-sh/uv/pulls/3830.md) on 2024-05-24 18:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-24 18:53_

---

_Comment by @charliermarsh on 2024-05-24 18:53_

I'll just remove the group for now. Better experience.

---

_Comment by @thejcannon on 2024-05-24 18:54_

Oh, well also I should point out `uv --version` gives me `uv 0.1.44 (d417daad7 2024-05-14)`

---

_Comment by @zanieb on 2024-05-24 18:56_

Weird...
```
❯ uv self update
info: Checking for updates...
success: Upgraded `uv` to v0.2.3! https://github.com/astral-sh/uv/releases/tag/0.2.3
❯ which uv
/Users/zb/.cargo/bin/uv
❯ uv --version
uv 0.2.3 (ce4d862b0 2024-05-24)
❯ uv venv
Using Python 3.12.3 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install uv
Resolved 1 package in 151ms
Downloaded 1 package in 1.00s
Installed 1 package in 6ms
 + uv==0.2.3
❯ .venv/bin/uv --version
uv 0.2.3 (ce4d862b0 2024-05-24)
```

edit: Oh, you mean you're actually on an older version :D

---

_Comment by @zanieb on 2024-05-24 18:57_

Definitely not fixed on the latest version though
```
❯ UV_SYSTEM_PYTHON=1 uv pip install --python .venv anyio
error: the argument '--python <PYTHON>' cannot be used with '--system'

Usage: uv pip install --python <PYTHON> <PACKAGE|--requirement <REQUIREMENT>|--editable <EDITABLE>|--unstable-uv-lock-file <UNSTABLE_UV_LOCK_FILE>>

For more information, try '--help'.

```

---

_Comment by @thejcannon on 2024-05-24 18:59_

Yeah I just wanted to get ahead of "oh maybe Josh is using an older version 😂 "

(I should type more words in my comments 🤔 )

---

_Closed by @charliermarsh on 2024-05-24 19:03_

---

_Comment by @epage on 2024-05-24 19:15_

For context, clap doesn't consider there to be a conflict for default values, only explicit user values.  Environment variables are a weird middle ground that we currently treat as user values which cause problems like this.

---
