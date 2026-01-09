---
number: 12748
title: Allow configuration of environment discovery (i.e., only check cwd)
type: issue
state: open
author: ollie-bell
labels:
  - configuration
assignees: []
created_at: 2025-04-08T16:47:21Z
updated_at: 2025-04-08T18:12:51Z
url: https://github.com/astral-sh/uv/issues/12748
synced_at: 2026-01-07T13:12:18-06:00
---

# Allow configuration of environment discovery (i.e., only check cwd)

---

_Issue opened by @ollie-bell on 2025-04-08 16:47_

### Question

The [docs explain](https://docs.astral.sh/uv/pip/environments/#discovery-of-python-environments) that `uv pip` commands will search for a virtual environment which ultimately resorts to looking up in parent directories.

Can this behaviour be configured (e.g. via an environment variable or in pyproject.toml `tool.uv.pip`) such that `uv pip` commands will strictly only search in the current directory, and not try to search parent directories?

### Platform

Darwin 24.3.0 arm64

### Version

uv 0.6.13 (a0f5c7250 2025-04-07)

---

_Label `question` added by @ollie-bell on 2025-04-08 16:47_

---

_Comment by @zanieb on 2025-04-08 17:14_

No, that's not configurable at this time. What's your use-case?

---

_Label `configuration` added by @zanieb on 2025-04-08 17:14_

---

_Comment by @ollie-bell on 2025-04-08 17:18_

I just found that it's not behaviour I necessarily want. e.g. I was in a project directory (say `~/projects/my-project`) without a environment folder and running `uv pip install` installed into some random virtualenv I happened to have in my home directory (`~/.venv`). IMO I'd prefer for `uv pip` to just tell me to make a environment in my current directory than install in another random one it could find in a parent directory.

---

_Comment by @zanieb on 2025-04-08 17:23_

I think you can do `UV_PYTHON="./.venv"` (or the matching setting in the `toml`)

---

_Comment by @ollie-bell on 2025-04-08 17:26_

Yeh that seems to work! It would be slightly more obvious to have an explicit setting for `uv pip` not searching parent directories, but that's just my 2c.. thanks!

---

_Comment by @ollie-bell on 2025-04-08 17:31_

Ah actually it doesn't work because now `uv venv` fails with

```
  × No interpreter found for path `./.venv` in managed installations or search path
```

after adding

```toml
[tool.uv.pip]
python = "./.venv"
```

to `pyproject.toml`

---

_Comment by @zanieb on 2025-04-08 17:39_

>  It would be slightly more obvious to have an explicit setting for uv pip not searching parent directories, but that's just my 2c.. thanks!

Perhaps, but we have a lot of options and we need to see significant need from multiple users before we can justify adding more.

> Ah actually it doesn't work because now uv venv fails with

Oh, funny... not sure what to do about that yet. `uv venv --no-config` would work as a hack, I guess.

---

_Comment by @ollie-bell on 2025-04-08 17:41_

Understood! I'll stick with the default behaviour for now and will keep a look out for any new config updates in the future :) Thanks.

Feel free to turn this into a feature request if that's how you track such requests (or I'm happy to make a feature request issue?)

---

_Renamed from "Configuring environment discovery for `uv pip`" to "Allow configuration of environment discovery (i.e., only check cwd)" by @zanieb on 2025-04-08 18:12_

---

_Label `question` removed by @zanieb on 2025-04-08 18:12_

---
