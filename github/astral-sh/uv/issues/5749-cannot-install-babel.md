---
number: 5749
title: Cannot install Babel
type: issue
state: closed
author: RomainBrault
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-03T13:52:32Z
updated_at: 2024-08-08T14:28:00Z
url: https://github.com/astral-sh/uv/issues/5749
synced_at: 2026-01-07T13:12:17-06:00
---

# Cannot install Babel

---

_Issue opened by @RomainBrault on 2024-08-03 13:52_

I cannot install Babel with

`uv tool install Babel`

I get

```console
error: Failed to find dist-info directory `babel-2.15.0.dist-info` in environment at XXX/.local/share/uv/tools/babel
```

However the dist-info exists:

```console
ls ~/.local/share/uv/tools/babel/lib/python3.12/site-packages/
__pycache__  _virtualenv.pth  _virtualenv.py  babel  Babel-2.15.0.dist-info
```

I suspect this is due to the capital letter in the package name.

Maybe related: https://github.com/astral-sh/uv/pull/4686/files

Babel:
 * installation guide: https://babel.pocoo.org/en/latest/installation.html
 * pypi: https://pypi.org/project/Babel/
 * repository: https://github.com/python-babel/babel


---

_Comment by @RomainBrault on 2024-08-03 14:02_

NB: I am now sure. `babel-2.15.0.dist-info` is not `Babel-2.15.0.dist-info`

A dirty fix:

```console
uv too install Babel
cd ~/.local/share/uv/tools/babel/lib/python3.12/site-packages/
mv Babel-2.15.0.dist-info babel-2.15.0.dist-info
cd <my_project>
uv too install Babel
```

Ie: rename to remove the capital letter.

---

_Comment by @RomainBrault on 2024-08-03 14:13_

I think I found the "offending" line:

https://github.com/astral-sh/uv/blob/ba924d298f7aeb081711946e6ffc3db7c8d3b4fd/crates/uv-normalize/src/lib.rs#L31

There should be no conversion to lowercase for packages names?

---

_Comment by @charliermarsh on 2024-08-03 17:07_

Yeah we need to make that more robust.

Technically it's a bug in the package itself. The `.dist-info` directory is outlined [here](https://packaging.python.org/en/latest/specifications/recording-installed-packages/):

> This directory is named as {name}-{version}.dist-info, with name and version fields corresponding to [Core metadata specifications](https://packaging.python.org/en/latest/specifications/core-metadata/#core-metadata). Both fields must be normalized (see the [name normalization specification](https://packaging.python.org/en/latest/specifications/name-normalization/#name-normalization) and the [version normalization specification](https://packaging.python.org/en/latest/specifications/version-specifiers/#version-specifiers-normalization)), and replace dash (-) characters with underscore (_) characters, so the .dist-info directory always has exactly one dash (-) character in its stem, separating the name and version fields.

The Core Metadata specification asserts that the name should be normalized to lowercase. So it's correct to expect a lowercased directory, but we're supposed to be robust to it _not_ being lowercase due to historical packages (and we are elsewhere).

Anyway, TLDR is: it's a misbehavior by the package, but we shouldn't fail.


---

_Label `bug` added by @charliermarsh on 2024-08-03 17:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-03 17:07_

---

_Label `preview` added by @charliermarsh on 2024-08-03 17:07_

---

_Referenced in [python-babel/babel#1109](../../python-babel/babel/issues/1109.md) on 2024-08-03 19:56_

---

_Referenced in [astral-sh/uv#5756](../../astral-sh/uv/pulls/5756.md) on 2024-08-03 23:23_

---

_Closed by @charliermarsh on 2024-08-03 23:43_

---

_Comment by @CoolCat467 on 2024-08-05 16:12_

https://github.com/python-babel/babel/pull/1110 fixed the root cause of this issue.

---

_Comment by @akx on 2024-08-08 14:27_

Released in https://pypi.org/project/babel/2.16.0/ - sorry for the trouble! 

---
