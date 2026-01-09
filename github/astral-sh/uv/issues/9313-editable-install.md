---
number: 9313
title: editable install
type: issue
state: closed
author: Helfrid
labels:
  - question
assignees: []
created_at: 2024-11-21T11:55:35Z
updated_at: 2024-11-21T13:49:53Z
url: https://github.com/astral-sh/uv/issues/9313
synced_at: 2026-01-07T13:12:18-06:00
---

# editable install

---

_Issue opened by @Helfrid on 2024-11-21 11:55_

I am encountering a problem when trying to work with editable installs using the uv packaging system.

I am on uv version uv 0.5.4 (c62c83c37 2024-11-20) on a Mac M1 MacOS 15.0.1.
Below I reproduce the steps I took:

I built a library uv-out with uv init --lib and generate a module hello_check.py with a hello_world function.
I generate a 2nd project using uv init and setup a venv using uv venv.
Both uv-in and uv-out using a local python 3.12 distribution.

Now I install uv-out in uv-in via uv pip install -e /Users/user/Desktop/uv-out      
with uv pip list I can see the install but I cannot import uv_out.
I get ModuleNotFoundError: No module named 'uv_out'
When I exectute uv sync the uv-out package is deleted.

Now I use uv add /Users/user/Desktop/uv-out 
This brings me one step further, and I can see the module but I cannot import hello_check
ImportError: cannot import name 'hello_check' from 'uv_out' 

When I now repeat uv pip install -e /Users/user/Desktop/uv-out 
everything works fine and I can load the functions and run them in my uv-in project.

When I execute uv sync, it reverts back and uv-out is imported but  'hello_check' not found.

I suspect this is an issue with caching, where the first version of uv-out is fixed that didn't have hello_check.py.

My question is why is the editable install overidden by uv sync? And why does it not work without prior
uv add?

Sorry for the convoluted message, but I hope you can reproduce these steps.



---

_Comment by @FishAlchemist on 2024-11-21 12:43_

Although I don't know why not having ``--editable`` would result in no packages being installed, if you want an editable installation in ``uv add``, you actually need to do it like this.
```
 uv add --editable  /Users/user/Desktop/uv-out
```

---

_Comment by @Helfrid on 2024-11-21 13:11_

ok, I understand now; uv pip install is cleared by uv sync. So I have to work only with uv add --editable. The -e flag doesnt work with add.

---

_Closed by @Helfrid on 2024-11-21 13:11_

---

_Label `question` added by @charliermarsh on 2024-11-21 13:49_

---
