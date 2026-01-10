---
number: 13101
title: "uv can't find installed packages"
type: issue
state: open
author: eromoe
labels:
  - question
assignees: []
created_at: 2025-04-25T08:02:27Z
updated_at: 2025-04-28T13:50:08Z
url: https://github.com/astral-sh/uv/issues/13101
synced_at: 2026-01-10T01:25:29Z
---

# uv can't find installed packages

---

_Issue opened by @eromoe on 2025-04-25 08:02_

### Summary

```
E:\Workspace\github_me\chroma-mcp>uv tool run mcp
Error: typer is required. Install with 'pip install mcp[cli]'

E:\Workspace\github_me\chroma-mcp>uv pip install mcp[cli]
Audited 1 package in 240ms

E:\Workspace\github_me\chroma-mcp>uv tool run mcp
Error: typer is required. Install with 'pip install mcp[cli]'
```

### Platform

Windows 10

### Version

uv 0.6.16 (d8ad9d3cd 2025-04-22)

### Python version

Python 3.12.9(mamba)

---

_Label `bug` added by @eromoe on 2025-04-25 08:02_

---

_Comment by @konstin on 2025-04-25 08:23_

`uv pip` and `uv tool` are two different interface: `uv pip` installs into the current environment, while `uv tool` installs into an isolated, managed environment. You need to use `uv tool install "mcp[cli]"`

---

_Label `bug` removed by @konstin on 2025-04-25 08:23_

---

_Label `question` added by @konstin on 2025-04-25 08:23_

---

_Comment by @eromoe on 2025-04-25 08:31_

@konstin It is make me confused , then how `uv tool` know the packages in `venv`? 
If I need `mcp dev  xxx.py` .  

# uv pip install 

1. `uv pip install mcp[cli]`
2. `uv run dev src\chroma_mcp\server.py`  got  `ModuleNotFoundError: No module named 'mcp'`

# uv tool install 

1. `uv tool install mcp[cli]`  
2.  `uv tool mcp dev src\chroma_mcp\server.py`  got  ModuleNotFoundError: No module named 'chromadb'

`chromadb` is installed in by uv sync . 

----

All these make me confused .. too difficult to use it.




---

_Comment by @eromoe on 2025-04-25 09:20_

```
E:\Workspace\github_me\chroma-mcp>uv run src\chroma_mcp\server.py
Traceback (most recent call last):
  File "E:\Workspace\github_me\chroma-mcp\src\chroma_mcp\server.py", line 3, in <module>
    import chromadb
ModuleNotFoundError: No module named 'chromadb'

E:\Workspace\github_me\chroma-mcp>uv run python -c "import chromadb"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'chromadb'

E:\Workspace\github_me\chroma-mcp>uv pip list|grep chroma
chroma-hnswlib                           0.7.6
chroma-mcp                               0.2.2           E:\Workspace\github_me\chroma-mcp
chromadb                                 1.0.3
```

Many many cases like these ... I have no idea .


---

_Comment by @eromoe on 2025-04-25 09:55_

@konstin 
I find chromadb is in the `.venv\Lib\site-packages`
but ` E:\Workspace\github_me\chroma-mcp\.venv\Lib\site-packages` doesn't not in `sys.path` .

`uv run python`  : 

```
>>> import sys
>>> print(sys.path)
['', 'C:\\Users\\Kasim\\AppData\\Roaming\\uv\\python\\cpython-3.10.17-windows-x86_64-none\\python310.zip', 'C:\\Users\\Kasim\\AppData\\Roaming\\uv\\python\\cpython-3.10.17-windows-x86_64-none\\DLLs', 'C:\\Users\\Kasim\\AppData\\Roaming\\uv\\python\\cpython-3.10.17-windows-x86_64-none\\lib', 'C:\\Users\\Kasim\\AppData\\Roaming\\uv\\python\\cpython-3.10.17-windows-x86_64-none', 'E:\\Workspace\\github_me\\chroma-mcp\\.venv']
>>> import chromadb
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'chromadb'
>>> import sys
>>> import os
>>> site_packages = os.path.join("E:\\Workspace\\github_me\\chroma-mcp\\.venv", "Lib", "site-packages")
>>> sys.path.append(site_packages)
>>> import chromadb
```



---

_Comment by @charliermarsh on 2025-04-25 12:36_

`uv tool` doesn't know anything about your current virtual environment. It's for tools that should be isolated and run in isolation, like `ruff`. What are you trying to do?

---

_Comment by @eromoe on 2025-04-26 04:24_

If you see my above comments, you would find the uv do something wrong on my machine . 
I can't access packages installed by uv . 


---

_Referenced in [astral-sh/uv#13098](../../astral-sh/uv/issues/13098.md) on 2025-04-26 04:28_

---

_Comment by @eromoe on 2025-04-28 07:42_

@konstin @zanieb @charliermarsh I tested on my MAC , uv behavior is normal.  
I remove all mamba settings from env path, but this problem still exits, I think this is some kind of bug in uv .

---

_Comment by @konstin on 2025-04-28 09:16_

Please refer to https://docs.astral.sh/uv/concepts/tools/, https://docs.astral.sh/uv/concepts/projects/dependencies/ and https://docs.astral.sh/uv/pip/environments/ for the differences between `uv tool`, `uv pip` and `uv run`/`uv sync`. When using `uv pip` before `uv run`, make sure that `uv pip` is using the same environment as `uv run`, otherwise you're installing packages in a non-project environment, and then `uv run` doesn't find them in the project environment. You can get more diagnostic output to debug the problem with `-v`.

---

_Comment by @eromoe on 2025-04-28 13:49_

@konstin Intuitively, and in fact on my mac : `uv pip` is using the same environment as `uv run` .
Such as below steps works on my mac
```
git clone  https://github.com/chroma-core/chroma-mcp && cd chroma-mcp
uv sync
uv run chroma-mcp
```
But not on my PC.  That's the main problem .  I have also post the detail : 

> [@konstin](https://github.com/konstin) I find chromadb is in the `.venv\Lib\site-packages` but ` E:\Workspace\github_me\chroma-mcp\.venv\Lib\site-packages` doesn't not in `sys.path` .
> 
> `uv run python` :
> 
> ```
> >>> import sys
> >>> print(sys.path)
> ['', 'C:\\Users\\Kasim\\AppData\\Roaming\\uv\\python\\cpython-3.10.17-windows-x86_64-none\\python310.zip', 'C:\\Users\\Kasim\\AppData\\Roaming\\uv\\python\\cpython-3.10.17-windows-x86_64-none\\DLLs', 'C:\\Users\\Kasim\\AppData\\Roaming\\uv\\python\\cpython-3.10.17-windows-x86_64-none\\lib', 'C:\\Users\\Kasim\\AppData\\Roaming\\uv\\python\\cpython-3.10.17-windows-x86_64-none', 'E:\\Workspace\\github_me\\chroma-mcp\\.venv']
> >>> import chromadb
> Traceback (most recent call last):
>   File "<stdin>", line 1, in <module>
> ModuleNotFoundError: No module named 'chromadb'
> >>> import sys
> >>> import os
> >>> site_packages = os.path.join("E:\\Workspace\\github_me\\chroma-mcp\\.venv", "Lib", "site-packages")
> >>> sys.path.append(site_packages)
> >>> import chromadb
> ```

I have no idea how to dig more 

---
