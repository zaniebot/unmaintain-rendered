---
number: 17112
title: Cannot use pylock.toml https link
type: issue
state: closed
author: paugier
labels:
  - bug
assignees: []
created_at: 2025-12-12T20:24:59Z
updated_at: 2025-12-17T00:16:24Z
url: https://github.com/astral-sh/uv/issues/17112
synced_at: 2026-01-10T01:26:13Z
---

# Cannot use pylock.toml https link

---

_Issue opened by @paugier on 2025-12-12 20:24_

### Summary

It's possible to use links towards requirements.txt files. For example

```sh
uv pip install -r https://raw.githubusercontent.com/serge-sans-paille/pythran/refs/heads/master/requirements.txt
```

works fine.

However, it does not work for pylock.toml:

```sh
$ uv pip install -r https://foss.heptapod.net/fluiddyn/install-locked-env/-/raw/branch/default/envs/env-pdm-pylock/pylock.toml
error: File not found: `https://foss.heptapod.net/fluiddyn/install-locked-env/-/raw/branch/default/envs/env-pdm-pylock/pylock.toml`
```

Related to https://github.com/astral-sh/uv/issues/15942#issuecomment-3647167383

See also https://discuss.python.org/t/new-project-cli-install-locked-env-what-about-security/105270/8

### Platform

Linux

### Version

0.9.17

### Python version

3.14

---

_Label `bug` added by @paugier on 2025-12-12 20:25_

---

_Comment by @woodruffw on 2025-12-12 23:40_

Hmm, does `pip install -r <link to pylock>` work? I would expect that to not work either, since `-r` implies a requirements file, not a `pylock.toml` file.

I tried this locally with `simple.http` and got the kind of parse error I'd expect:

```
% uv run python -m pip install -r http://localhost:8080/pylock.toml
ERROR: Invalid requirement: 'lock-version = "1.0"': Expected end or semicolon (after name and no valid version specifier)
    lock-version = "1.0"
                 ^ (from line 3 of http://localhost:8080/pylock.toml)
Hint: = is not a valid operator. Did you mean == ?
```

Given that, I'm not sure it makes sense for uv to support installing from pylock.toml files in the pip compatibility layer in a manner that pip itself doesn't support.

---

_Comment by @notatallshaw on 2025-12-12 23:55_

`uv pip install -r` already supports installing a `pylock.toml` file: https://docs.astral.sh/uv/reference/cli/#uv-pip-install--requirements

`uv pip install` supports lots of semantics pip doesn't support and may choose different semantics when it does support it, e.g. https://github.com/pypa/pip/pull/13052 and https://github.com/pypa/pip/pull/13625

---

_Comment by @woodruffw on 2025-12-13 00:04_

Oh huh, shows my own ignorance of uv's features 😅 

Yeah, given that `requirements.txt * (file, URL)` works and `pylock.toml * (file,)` works, it probably makes sense to complete the matrix and allow installation from URLs for `pylock.toml` too.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-13 03:23_

---

_Referenced in [astral-sh/uv#17119](../../astral-sh/uv/pulls/17119.md) on 2025-12-13 03:23_

---

_Closed by @charliermarsh on 2025-12-17 00:16_

---
