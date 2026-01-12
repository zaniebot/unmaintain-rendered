```yaml
number: 10626
title: "QUESTION: Python-3.13.1+freethreaded"
type: issue
state: closed
author: killcoder
labels:
  - question
assignees: []
created_at: 2025-01-15T07:12:00Z
updated_at: 2025-01-16T03:06:34Z
url: https://github.com/astral-sh/uv/issues/10626
synced_at: 2026-01-12T16:00:17Z
```

# QUESTION: Python-3.13.1+freethreaded

---

_@killcoder_

How do I make the python free threading version default for a new project? 

I have both versions of python 3.13.1 installed and when I init a uv project with 

```
 uv init --python cpython-3.13.1+freethreaded-windows-x86_64-none
```

the project is intialised and the basic files are created. The .python-version file shows 3.13 and the toml file shows 

```
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
```

However when I run python the GIL is enabled 

```
(test) PS C:\Users\x\test> python 
Python 3.13.1 (main, Jan  5 2025, 05:37:13) [MSC v.1942 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys._is_gil_enabled()
True
>>> 
```

Running the free threaded version of python shows 

```
(test) PS C:\Users\x\test> C:\Users\x\AppData\Roaming\uv\python\cpython-3.13.1+freethreaded-windows-x86_64-none\python.exe
Python 3.13.1 experimental free-threading build (main, Jan  5 2025, 05:39:06) [MSC v.1942 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys._is_gil_enabled()
False
>>> 
```

How do I edit the toml file to default to the free threaded version of python?

---

_Comment by @FishAlchemist on 2025-01-15 08:15_

My uv version: uv 0.5.18 (27d1bad55 2025-01-11)

Try:  ``uv python pin 3.13t``
I'm not sure if ``requires-python`` can restrict the use of **freethread** versions, but ``.python-version`` can definitely pin it to a specific **freethread** version.

![Image](https://github.com/user-attachments/assets/d6f867e2-dcea-4b2e-bdf1-dc816bbc751d)

![Image](https://github.com/user-attachments/assets/6df36a7f-059c-451f-b10b-61229ed7b8a4)




---

_Comment by @charliermarsh on 2025-01-15 16:35_

Yeah, `uv python pin` sounds right.

---

_Closed by @charliermarsh on 2025-01-15 16:35_

---

_Label `question` added by @charliermarsh on 2025-01-15 16:35_

---

_Comment by @killcoder on 2025-01-16 03:06_

Thanks everyone that worked. Unfortunately, there isn't enough wheels available for 3.13.1t to make it useful for my project. 


---
