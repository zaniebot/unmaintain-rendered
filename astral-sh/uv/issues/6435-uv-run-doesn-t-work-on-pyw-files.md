```yaml
number: 6435
title: "`uv run` doesn't work on .pyw files"
type: issue
state: closed
author: Gaiko-sw
labels:
  - good first issue
  - windows
assignees: []
created_at: 2024-08-22T13:45:36Z
updated_at: 2024-08-22T23:07:32Z
url: https://github.com/astral-sh/uv/issues/6435
synced_at: 2026-01-12T15:59:04Z
```

# `uv run` doesn't work on .pyw files

---

_@Gaiko-sw_

Platform: Both Windows 10 & Ubuntu 24.04
uv version: uv 0.3.1

I have a python UI application at work that we start through `main.pyw` in order to prevent a console window opening if it's double clicked in Windows Explorer.
It's now managed through poetry, which launches it fine through the main.pyw file. In trying to switch the project to uv, I found that it's not able to start. I've attempted this minimal example on Ubuntu and got a similar result:

- `pip` or `pipx install uv`
- `uv init pyw_test`, `cd pyw_test`
- Put a tkinter hello world in `main.pyw`:
```
import tkinter as tk
root = tk.Tk()
root.mainloop()
```
- `uv run main.pyw`

You get an error like **"Failed to spawn: main.pyw"** then an OS specific **"Caused by: No such file/%1 is not a valid Win32 application"**

Changing main.pyw to main.py works fine


I think this might just be that pyw files aren't handled like py files. The .pyw only has an effect on the windows launcher, but I still think it should be handled by uv.

---

_Label `windows` added by @konstin on 2024-08-22 14:14_

---

_Label `good first issue` added by @zanieb on 2024-08-22 15:54_

---

_Comment by @zanieb on 2024-08-22 15:54_

Thanks for the report! This should be a quick fix.

---

_Assigned to @zanieb by @zanieb on 2024-08-22 15:54_

---

_Comment by @zanieb on 2024-08-22 15:54_

Does `uv run python main.pyw` work too?

---

_Closed by @zanieb on 2024-08-22 23:07_

---

_Closed by @zanieb on 2024-08-22 23:07_

---
