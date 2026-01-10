---
number: 9838
title: "Handle verbose output for `uv pip list --verbose`"
type: issue
state: open
author: julienp
labels:
  - compatibility
assignees: []
created_at: 2024-12-12T12:58:57Z
updated_at: 2024-12-12T16:00:02Z
url: https://github.com/astral-sh/uv/issues/9838
synced_at: 2026-01-10T01:24:46Z
---

# Handle verbose output for `uv pip list --verbose`

---

_Issue opened by @julienp on 2024-12-12 12:58_

Currently `uv pip list` does not handle the `-v/--verbose` flag.

This option makes `pip list` print the location for each package, as well as the installer. 
`Location` is the location where the package is installed, usually site-packages. `Installer` is read from `${PACKAGE}-${VERSION}.dist-info/INSTALLER`. 

https://github.com/pypa/pip/blob/947917be5d8d826efde3f0ad398ec6128cf5bfe8/src/pip/_internal/commands/list.py#L348

```
% uv run pip list -v
Package       Version Location                                                     Installer
------------- ------- ------------------------------------------------------------ ---------
Arpeggio      2.0.2   /Users/julien/tmp/uv-demo/.venv/lib/python3.13/site-packages uv
attrs         24.2.0  /Users/julien/tmp/uv-demo/.venv/lib/python3.13/site-packages uv
debugpy       1.8.8   /Users/julien/tmp/uv-demo/.venv/lib/python3.13/site-packages uv
...
````

When running `uv pip list -v`, instead the `-v` option prints verbose logs for uv instead of using the verbose list format:

```
uv pip list -v
DEBUG uv 0.5.8 (Homebrew 2024-12-11)
DEBUG Searching for default Python interpreter in virtual environments or search path
DEBUG Found `cpython-3.13.0-macos-aarch64-none` at `/Users/julien/tmp/uv-demo/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.13.0 environment at: .venv
Package       Version
------------- -------
arpeggio      2.0.2
attrs         24.2.0
debugpy       1.8.8
```

---

_Referenced in [pulumi/pulumi#18023](../../pulumi/pulumi/issues/18023.md) on 2024-12-12 13:00_

---

_Referenced in [astral-sh/uv#9839](../../astral-sh/uv/pulls/9839.md) on 2024-12-12 13:11_

---

_Label `compatibility` added by @charliermarsh on 2024-12-12 14:25_

---

_Comment by @charliermarsh on 2024-12-12 14:38_

I find that to be a slightly strange behavior but not opposed for compatibility.

---

_Comment by @julienp on 2024-12-12 16:00_

> I find that to be a slightly strange behavior but not opposed for compatibility.

I agree that it is a little weird, I wish `pip` had a flag specific to `pip list` to include more information instead of using the global `-v`.

My actual use case is getting a list of installed packages and their installation path, so that I can go look for a special file inside the packages. This is to handle some special behaviour in http://github.com/pulumi/pulumi.

Does uv provide another way to retrieve this? There's `uv tree`, but that does not provide machine readable output (nor the location information).

---

_Referenced in [pulumi/pulumi#18107](../../pulumi/pulumi/pulls/18107.md) on 2024-12-23 13:50_

---

_Referenced in [bernhard-42/vscode-ocp-cad-viewer#127](../../bernhard-42/vscode-ocp-cad-viewer/issues/127.md) on 2025-02-02 18:23_

---
