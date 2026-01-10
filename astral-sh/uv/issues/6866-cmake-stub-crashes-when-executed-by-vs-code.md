---
number: 6866
title: CMake stub crashes when executed by VS Code
type: issue
state: closed
author: petermbauer
labels: []
assignees: []
created_at: 2024-08-30T13:14:11Z
updated_at: 2024-09-05T06:13:30Z
url: https://github.com/astral-sh/uv/issues/6866
synced_at: 2026-01-10T01:24:07Z
---

# CMake stub crashes when executed by VS Code

---

_Issue opened by @petermbauer on 2024-08-30 13:14_

When installing CMake via `uv pip install` instead of `pip install`, the stub created by uv crashes with a segfault when executed by the [CMake Tools](https://github.com/microsoft/vscode-cmake-tools) extension.

![image](https://github.com/user-attachments/assets/f8272e33-9d30-4607-a540-ee8dc2aa70e3)

How to reproduce:
- make sure to have VS Code with the CMake Tools extension installed and some [CMakeLists.txt](https://github.com/user-attachments/files/16817335/CMakeLists.txt) file in the current directory which is to be picked up by VS Code automatically.
- run the following commands:
```
λ bin\uv.exe --version
uv 0.4.0 (d9bd3bc7a 2024-08-28)

λ bin\uv.exe venv myvenv
Using Python 3.10.9 interpreter at: ...
Creating virtualenv at: myvenv
Activate with: myvenv\Scripts\activate

λ bin\uv.exe pip install --python myvenv cmake==3.27.0
Resolved 1 package in 208ms
Installed 1 package in 1.97s
 + cmake==3.27.0

λ myvenv\Scripts\activate.bat

(myvenv) λ code .
```
- just select "Unspecified kit" to let CMake select one or any other installed kit that works for you
- check the CMake/Build output

When executed from the command line, `cmake` works as expected, same also from any terminal inside VS Code so i guess this is caused by VS Code executing the `cmake.exe` directly without a shell in between.

Most likely related to #6464.

Windows 10, Python 3.10

---

_Referenced in [astral-sh/uv#6792](../../astral-sh/uv/pulls/6792.md) on 2024-08-30 13:55_

---

_Comment by @petermbauer on 2024-08-31 05:53_

No idea why it did not work when testing with the binary from #6792 but i can confirm that it works now with uv 0.4.1 so thanks a lot for the fix!
However, the same error output as mentioned in https://github.com/astral-sh/uv/issues/6699#issuecomment-2322515962 also appears in VS Code.
```
[build] Failed to close child file descriptors at 1
[build] Failed to close child file descriptors at 2
```

---

_Closed by @petermbauer on 2024-08-31 05:53_

---

_Referenced in [astral-sh/uv#6955](../../astral-sh/uv/pulls/6955.md) on 2024-09-03 02:31_

---

_Comment by @samypr100 on 2024-09-04 22:45_

@petermbauer Would you be able to confirm 0.4.5 fixes the fd issue for you?

---

_Comment by @petermbauer on 2024-09-05 06:13_

> @petermbauer Would you be able to confirm 0.4.5 fixes the fd issue for you?

yes, i can confirm. Thanks a lot!

---
