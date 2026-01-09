---
number: 6801
title: "Provide an option to start `uv` on Windows without a console window"
type: issue
state: closed
author: piepero
labels:
  - enhancement
  - windows
assignees: []
created_at: 2024-08-29T10:33:42Z
updated_at: 2025-06-06T09:10:31Z
url: https://github.com/astral-sh/uv/issues/6801
synced_at: 2026-01-07T13:12:17-06:00
---

# Provide an option to start `uv` on Windows without a console window

---

_Issue opened by @piepero on 2024-08-29 10:33_

I am using the Windows 11 Task Scheduler to periodically run a Python script using the `uv --quiet run` command.

The script has the `.pyw` file extension, and `uv` correctly uses `pythonw.exe` to run the script without a console window.

Unfortunately, `uv.exe` itself still opens an empty console window. It stays open until it has parsed the inline metadata, checked the  dependencies and started `pythonw.exe`. The window is both distracting and steals the focus.

It would be great, if there was an additional flag (e.g. `--hidden`, `--no-console`) to suppress the creation of the console window.
Alternatively, it may be possible to add this functionality to the `--quiet` flag.

---

_Label `enhancement` added by @zanieb on 2024-09-22 23:33_

---

_Label `windows` added by @zanieb on 2024-09-22 23:33_

---

_Comment by @zanieb on 2024-09-22 23:34_

Sorry nobody replied here. This seems reasonable, but I have no idea how it would be implemented.

---

_Comment by @akx on 2024-09-25 14:37_

On a general level, you'd need to set [the `windows_subsystem` attribute](https://doc.rust-lang.org/reference/runtime.html#the-windows_subsystem-attribute) to `"windows"` so no console is provided by Windows to the application.

However, the issue then is 
![image](https://github.com/user-attachments/assets/0183f2b2-58e2-463f-802c-b3aa3028372b)

– as I understand it, this is why `python.exe` and `pythonw.exe` are entirely separate executables: `python.exe` has the subsystem set to `CONSOLE`, `pythonw.exe` has it set to `WINDOWS` (and respectively, `.pyw` files are associated with `pythonw.exe`).

---

_Comment by @zanieb on 2024-09-25 14:39_

I guess we could ship a `uvw.exe` on Windows for this purpose? I'm definitely not an expert in this though. Is there no other way?

---

_Comment by @akx on 2024-09-26 08:01_

https://stackoverflow.com/q/19168763/51685 touches on a Windows subsystem app being able to request a console, but the comments are somewhat worrisome ("invariably works poorly"...).

---

_Comment by @CrendKing on 2025-02-24 16:26_

Is the `--gui-script` option meant to provide a way to start a script without a window on Windows? If so, I think it actually needs this issue to work. Since `uv` itself always spawns a console window, and it always waits for the subprocess to terminate, the console window is inevitable.

I suggest shipping a `uvw.exe` built with the `windows` subsystem, and deprecate `--script` and `--gui-script`. Make `uvw run` invoke `pythonw`.

---

_Referenced in [astral-sh/uv#11786](../../astral-sh/uv/pulls/11786.md) on 2025-02-26 01:40_

---

_Comment by @CrendKing on 2025-02-26 01:40_

Created a PR. Please take a look.

https://github.com/astral-sh/uv/pull/11786

---

_Comment by @CrendKing on 2025-05-31 02:06_

The new `uvw` binary is released in [version 0.7.9](https://github.com/astral-sh/uv/releases/tag/0.7.9). Maybe you guys can take a look?

---

_Closed by @zanieb on 2025-05-31 04:51_

---

_Comment by @piepero on 2025-06-06 09:10_

Using `uvw.exe` instead of `uv.exe` works like a charm!
Looking at the pull request, I can see that a surprising amount of work was involved to overcome all obstacles for such a 'simple' request.
Thanks a lot to all involved!

---
