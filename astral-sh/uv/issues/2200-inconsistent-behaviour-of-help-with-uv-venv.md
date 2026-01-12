```yaml
number: 2200
title: "Inconsistent behaviour of `help` with `uv venv`"
type: issue
state: closed
author: navneethc
labels:
  - question
assignees: []
created_at: 2024-03-05T12:08:40Z
updated_at: 2024-03-05T14:56:07Z
url: https://github.com/astral-sh/uv/issues/2200
synced_at: 2026-01-12T15:58:36Z
```

# Inconsistent behaviour of `help` with `uv venv`

---

_@navneethc_

Ubuntu 22.04
uv 0.1.14

I like the fact that `uv` doesn't always require me to type `--` before `help` to see the documentation, however this doesn't always work as intended. When you run `uv venv help`, as I did, it instead creates a venv named `help`.

```
❯ uv venv help
Using Python 3.10.12 interpreter at: /usr/bin/python3
Creating virtualenv at: help
Activate with: source help/bin/activate
```
And there's no mention of this at the documentation at the top level.

```
❯ uv help
Usage: uv [OPTIONS] <COMMAND>

Commands:
  pip      Resolve and install Python packages
  venv     Create a virtual environment
  cache    Manage the cache
  version  Display uv's version
  help     Print this message or the help of the given subcommand(s)
...
```

This kind of behaviour may appear 'obvious' in retrospect, but if someone wishes to quickly look up documentation for a subcommand like `venv` but keeps running into an issue like this, it may get irritating.

---

_Comment by @T-256 on 2024-03-05 12:26_

You can use `uv help venv` instead.
`help` is for commands that have subcommands like `uv pip`. even though I suggest you always use `help` as first command, because it accepts nested subcommands: `uv help pip install`.

---

_Comment by @navneethc on 2024-03-05 12:34_

> You can use `uv help venv` instead. `help` is for commands that have subcommands like `uv pip`. even though I suggest you always use `help` as first command, because it accepts nested subcommands: `uv help pip install`.

That is indeed helpful, thank you.

---

_Comment by @charliermarsh on 2024-03-05 13:48_

Thanks @T-256.

---

_Closed by @charliermarsh on 2024-03-05 13:48_

---

_Label `question` added by @zanieb on 2024-03-05 14:56_

---
