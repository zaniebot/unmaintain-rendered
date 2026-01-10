```yaml
number: 10165
title: Project entry point and generated executable not working using Python version 3.7
type: issue
state: closed
author: kevintuwsp
labels:
  - windows
assignees: []
created_at: 2024-12-26T04:42:03Z
updated_at: 2024-12-27T11:06:11Z
url: https://github.com/astral-sh/uv/issues/10165
synced_at: 2026-01-10T04:36:21Z
```

# Project entry point and generated executable not working using Python version 3.7

---

_Issue opened by @kevintuwsp on 2024-12-26 04:42_

When initialising a project on Windows using the below commands, the resulting project executable does not run correctly.

> uv init --app --project newpackage
> uv run newpackage
SyntaxError: Non-UTF-8 code starting with '\x90' in file C:\Users\mrkev\Documents\Github\newpackage\.venv\Scripts\newpackage.exe on line 2, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details
(newpackage)

Is there anything I am doing wrong? How can I check if the generated executable is correct?



---

_Comment by @FishAlchemist on 2024-12-26 11:28_

![Image](https://github.com/user-attachments/assets/381453df-7cc7-4a5e-abc7-fd3d19876d5a)
I cannot reproduce the issue you're describing. 
However, when I encounter similar encoding problems, it's usually because the source code is not in valid UTF-8.

---

_Comment by @kevintuwsp on 2024-12-26 13:24_

> ![Image](https://github.com/user-attachments/assets/381453df-7cc7-4a5e-abc7-fd3d19876d5a) I cannot reproduce the issue you're describing. However, when I encounter similar encoding problems, it's usually because the source code is not in valid UTF-8.

Apologies, you will need to change directory to the created project folder
> uv init --app --project newpackage
> cd newpackage
> uv run newpackage

---

_Comment by @FishAlchemist on 2024-12-26 13:30_

@kevintuwsp 
![Image](https://github.com/user-attachments/assets/e377b498-1f95-4e79-97ae-7929b549f29f)
Which version of UV are you using? I'm using UV 0.5.11 on Windows 11 x86-64, but I can't reproduce the issue.

---

_Comment by @kevintuwsp on 2024-12-26 13:46_

I am using version 0.5.11 also. 
Seems like your system isn't adding the executable to your path.
Can you try run the executable that UV generates in /.venv/Scripts/newpackage.exe?

---

_Comment by @FishAlchemist on 2024-12-26 13:55_

@kevintuwsp No, my Scripts folder doesn't have ``newpackage.exe``, so I want to create it from scratch and it won't have ``newpackage.exe``.
![Image](https://github.com/user-attachments/assets/9f55c205-afa3-41d8-a368-6072a90d394e)


---

_Comment by @kevintuwsp on 2024-12-26 14:03_

Sorry it is a little late, my instructions were not correct.
It looks like this is working correctly on the later versions of python, e.g. 3.13.1.

However, for my use case I need to use the older 3.7.9 python version which shows the error.

> uv init --app --**package** newpackage
> cd newpackage
> uv python install 3.7.9
> uv python pin 3.7.9
> uv run newpackage

---

_Renamed from "Project entry point and generated executable not working" to "Project entry point and generated executable not working using Python version 3.7" by @kevintuwsp on 2024-12-26 14:10_

---

_Comment by @vivodi on 2024-12-26 14:35_

Python 3.7 is not supported.
https://github.com/astral-sh/uv/blob/e6126ce0dc105c329dac4d45cf74c2ea8c944882/pyproject.toml#L10

---

_Label `windows` added by @charliermarsh on 2024-12-26 14:35_

---

_Comment by @FishAlchemist on 2024-12-26 16:07_

@kevintuwsp 
If I use Python 3.7, this issue will occur. 
However, as @vivodi  mentioned, uv doesn't support 3.7, so even if this issue needs to be fixed, it will probably take a long time.
So the quickest solution is to avoid using 3.7. It seems like 3.8 works fine.

---

_Comment by @kevintuwsp on 2024-12-27 11:06_

Thanks all for your help, closing this issue.

---

_Closed by @kevintuwsp on 2024-12-27 11:06_

---
