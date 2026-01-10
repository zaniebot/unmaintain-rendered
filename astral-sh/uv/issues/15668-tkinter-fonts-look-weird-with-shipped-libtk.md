```yaml
number: 15668
title: tkinter fonts look weird with shipped libtk
type: issue
state: open
author: eliasdorneles
labels:
  - bug
assignees: []
created_at: 2025-09-03T20:49:35Z
updated_at: 2026-01-01T18:22:40Z
url: https://github.com/astral-sh/uv/issues/15668
synced_at: 2026-01-10T03:11:35Z
```

# tkinter fonts look weird with shipped libtk

---

_Issue opened by @eliasdorneles on 2025-09-03 20:49_

### Summary

Using this test program, in a file `font_test_tkinter.py`:

```python
import tkinter
import tkinter.font

root = tkinter.Tk()

title_font = tkinter.font.Font(size=20, weight="bold")
content_font = tkinter.font.Font(size=12)

title_label = tkinter.Label(root, text="My App", font=title_font)
content_text = tkinter.Text(root, font=content_font)

title_label.pack()
content_text.pack()
root.mainloop()
```

When I run:

    uv run --python 3.11.13 python font_test_tkinter.py

I get this ugliness:

<img width="662" height="473" alt="Image" src="https://github.com/user-attachments/assets/af6e93fc-5a18-4c25-b827-d71a6780d3ff" />


## Workaround

After some internet digging, I found this explanation for people having similar problems with anaconda: https://stackoverflow.com/questions/47769187/make-anacondas-tkinter-aware-of-system-fonts-or-install-new-fonts-for-anaconda/76794871#76794871

So I applied the suggested workaround of linking the system library version, as it matches the same version used in the uv installation (8.6):

```bash
cd $(dirname $(uv python find 3.11.13))/../lib  # cd to the managed python's lib folder
mv libtk8.6.so baklibtk8.6.so  # back up uv-installed lib libtk8.6.so, just in case
ln -s /usr/lib/x86_64-linux-gnu/libtk8.6.so   # link the libtk8.6.so from my system
```

After the above changes, when I run again `uv run --python 3.11.13 python font_test_tkinter.py`, I get this:

<img width="730" height="634" alt="Image" src="https://github.com/user-attachments/assets/e8c5e20a-0351-4da2-93ab-e44c7f1868d4" />

(I used python 3.11.3, but it's the same with other versions)

### Platform

Ubuntu 24.04

### Version

uv 0.8.15

### Python version

3.11.13

---

_Label `bug` added by @eliasdorneles on 2025-09-03 20:49_

---

_Comment by @zanieb on 2025-09-03 20:53_

cc @geofft 

Yeah, we don't ship any fonts so the default rendering is ugly. We're looking into a solution.

---

_Comment by @konstin on 2025-09-04 07:07_

Duplicate of #15602/#14042, but nice workaround!

---

_Comment by @psionman on 2025-09-25 08:38_

I have the same issue (Manjaro - no anaconda). My workaround is to load python into the virtualenv from the system python

```
uv venv --python /usr/bin/python3.13 .venv 
```

Not ideal because it by-passes the uv python version management system

---

_Comment by @ChiefBacon on 2025-11-15 16:32_

I have this issue as well, any update?

---

_Comment by @anpom21 on 2026-01-01 18:22_

I just faced the same issue. Both @eliasdorneles and @psionman workarounds worked for me on ubuntu 24.04 with uv 0.9.15.


---
