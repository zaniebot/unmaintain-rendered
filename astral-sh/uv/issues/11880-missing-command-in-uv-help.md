```yaml
number: 11880
title: "Missing command in `uv --help`"
type: issue
state: closed
author: thezbm
labels:
  - bug
assignees: []
created_at: 2025-03-01T08:35:57Z
updated_at: 2025-03-01T16:24:34Z
url: https://github.com/astral-sh/uv/issues/11880
synced_at: 2026-01-12T16:00:49Z
```

# Missing command in `uv --help`

---

_@thezbm_

### Summary

There are discrepancies between the output of the two commands:

```shell
❯ diff -b <(uv --help) <(uv help)   
22a23
>   generate-shell-completion  Generate shell completion
50c51,52
< Use `uv help` for more details.
---
> Use `uv help <command>` for more information on a specific command.
```

Notably, "generate-shell-completion" is not shown in the output of `uv --help`.

### Platform

Darwin 24.3.0 arm64

### Version

uv 0.6.3 (a0b9f22a2 2025-02-24)

### Python version

Python 3.13.2

---

_Label `bug` added by @thezbm on 2025-03-01 08:35_

---

_Comment by @thezbm on 2025-03-01 09:59_

Per the doc,

> When using the --help flag, uv displays a condensed help menu. To view a longer help menu for a command, use uv help:

Maybe this is how the help menu is condensed? Feel free to close this issue if this is an intentional behavior :)

---

_Comment by @zanieb on 2025-03-01 16:24_

Yeah this is intentional, https://github.com/astral-sh/uv/pull/11882#issuecomment-2692304870

---

_Closed by @zanieb on 2025-03-01 16:24_

---
