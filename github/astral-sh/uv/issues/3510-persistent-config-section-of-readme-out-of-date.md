---
number: 3510
title: Persistent config section of README out of date
type: issue
state: closed
author: jonfitt
labels:
  - bug
assignees: []
created_at: 2024-05-10T15:49:17Z
updated_at: 2024-05-10T18:53:15Z
url: https://github.com/astral-sh/uv/issues/3510
synced_at: 2026-01-07T13:12:17-06:00
---

# Persistent config section of README out of date

---

_Issue opened by @jonfitt on 2024-05-10 15:49_

> uv supports persistent configuration at both the project- and user-level.
> 
> Specifically, uv will search for a pyproject.toml or uv.toml file in the current directory, or in the nearest parent directory.
> 
> If a pyproject.toml file is found, uv will read configuration from the [tool.uv.pip] table. For example, to set a persistent index URL, add the following to a pyproject.toml:
> 
> [tool.uv.pip]
> index-url = "https://test.pypi.org/simple"

This section of the README seems inaccurate with the current version (0.1.42)

When trying to use an index-url setting I get:

```
error: Failed to parse `pyproject.toml`
  Caused by: TOML parse error at line 196, column 10
    |
196 | [tool.uv.pip]
    |          ^^^
unknown field `pip`, expected `sources` or `workspace`

```

I tried "sources" literally but that's not what it was looking for. Could you please update the doc?

---

_Referenced in [astral-sh/uv#3511](../../astral-sh/uv/pulls/3511.md) on 2024-05-10 16:56_

---

_Comment by @charliermarsh on 2024-05-10 18:36_

My guess is this is happening when you run `pip install -r pyproject.toml`, but not `pip install flask` (for example) with a `pyproject.toml` in the same directory?

---

_Label `bug` added by @charliermarsh on 2024-05-10 18:36_

---

_Comment by @jonfitt on 2024-05-10 18:43_

It was `uv pip compile`

---

_Closed by @charliermarsh on 2024-05-10 18:50_

---

_Comment by @charliermarsh on 2024-05-10 18:51_

Sorry, yes, the distinction I'm drawing is: you were passing it directly as an argument, right?

---

_Comment by @jonfitt on 2024-05-10 18:53_

Ah yes:
`uv pip compile .\requirements_dev.in .\pyproject.toml -o requirements.txt` to be exact.

---
