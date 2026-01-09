---
number: 4614
title: py -3.11 -m uv venv not working proprely 
type: issue
state: closed
author: zakimimit
labels:
  - windows
assignees: []
created_at: 2024-06-28T13:20:28Z
updated_at: 2024-06-28T21:41:53Z
url: https://github.com/astral-sh/uv/issues/4614
synced_at: 2026-01-07T13:12:17-06:00
---

# py -3.11 -m uv venv not working proprely 

---

_Issue opened by @zakimimit on 2024-06-28 13:20_

Hello ;

> Python 3.11.9   and use also Python 3.11.7
> uv 0.2.17 (2024-06-26)


I have probelm with :

`py -3.11 -m uv venv
`
when run it i got 
```

DEBUG uv 0.2.17
DEBUG Searching for Python interpreter in system toolchains
DEBUG Found cpython 3.8.10 at `\AppData\Local\Programs\Python\Python38-32\python.exe` (search path)
Using Python 3.8.10 interpreter at: \AppData\Local\Programs\Python\Python38-32\python.exe
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate

```

the uv  installed only installed in python  3.11


I hope you can fix it 

not 

==========
I have this istalled on my pc :
>python 3.8 32bit
>python 3.8 64bit
>python 3.11 64bit



My best regards

---

_Comment by @charliermarsh on 2024-06-28 13:25_

I would suggest running `uv venv -p 3.11`.

---

_Comment by @charliermarsh on 2024-06-28 20:02_

I can take a look when I'm back with my Windows machine.

---

_Label `windows` added by @charliermarsh on 2024-06-28 20:02_

---

_Comment by @zakimimit on 2024-06-28 21:41_

this uv venv -p 3.11 worked 

thank you 

---

_Closed by @zakimimit on 2024-06-28 21:41_

---
