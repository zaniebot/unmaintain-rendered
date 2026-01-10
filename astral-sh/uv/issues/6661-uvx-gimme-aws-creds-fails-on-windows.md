---
number: 6661
title: "`uvx gimme-aws-creds` fails on Windows"
type: issue
state: open
author: knowsuchagency
labels:
  - bug
  - windows
  - uv tool
assignees: []
created_at: 2024-08-26T22:52:10Z
updated_at: 2024-10-23T16:58:26Z
url: https://github.com/astral-sh/uv/issues/6661
synced_at: 2026-01-10T01:24:04Z
---

# `uvx gimme-aws-creds` fails on Windows

---

_Issue opened by @knowsuchagency on 2024-08-26 22:52_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

`uvx` seems to correctly identify the `gimme-aws-creds` executable but cannot invoke it.

This is with version `0.3.4`, specifically on Windows

```powershell
PS C:\Users\FOO> uvx gimme-aws-creds
Installed 45 packages in 2.42s
The executable `gimme-aws-creds` was not found.
warning: An executable named `gimme-aws-creds` is not provided by package `gimme-aws-creds`.
The following executables are provided by `gimme-aws-creds`:
- gimme-aws-creds
- gimme-aws-creds-autocomplete.sh
- gimme-aws-creds.cmd
```


---

_Comment by @zanieb on 2024-08-26 22:53_

Interesting, thanks for the report! Seems like a classic weird Windows thing.

---

_Label `bug` added by @zanieb on 2024-08-26 22:53_

---

_Label `windows` added by @zanieb on 2024-08-26 22:53_

---

_Label `uv tool` added by @zanieb on 2024-08-26 22:53_

---

_Comment by @samypr100 on 2024-08-26 23:04_

Hmmm, they don't define `console_scripts` entrypoints, just a [script section](https://github.com/Nike-Inc/gimme-aws-creds/blob/master/setup.py) which points to other scripts.

---

_Comment by @samypr100 on 2024-08-26 23:13_

From https://packaging.python.org/en/latest/guides/distributing-packages-using-setuptools/#scripts

> Although setup() supports a [scripts](https://setuptools.pypa.io/en/latest/deprecated/distutils/setupscript.html#distutils-installing-scripts) keyword for pointing to pre-made scripts to install, the recommended approach to achieve cross-platform compatibility is to use [Creating executable scripts](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#console-scripts) entry points

Seems like using `scripts` doesn't work well from a cross-platform perspective, especially on Windows.

---

_Comment by @zanieb on 2024-08-27 00:34_

Ah interesting. Thanks for the context. Does this work with `pipx`?

---

_Referenced in [astral-sh/uv#6682](../../astral-sh/uv/issues/6682.md) on 2024-08-27 11:52_

---

_Comment by @knowsuchagency on 2024-08-28 20:56_

It works on Mac, but not Windows. That's actually true for both `pipx` and `uvx`

---

_Referenced in [astral-sh/uv#8888](../../astral-sh/uv/issues/8888.md) on 2024-11-09 17:31_

---
