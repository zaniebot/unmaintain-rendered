```yaml
number: 7621
title: "`uv add pkg` adds extraneous new lines in `project.dependencies` if the pyproject.toml has `CRLF` ending"
type: issue
state: closed
author: Ravencentric
labels:
  - bug
  - good first issue
  - help wanted
  - windows
assignees: []
created_at: 2024-09-22T16:02:42Z
updated_at: 2024-09-23T13:58:26Z
url: https://github.com/astral-sh/uv/issues/7621
synced_at: 2026-01-10T04:45:10Z
```

# `uv add pkg` adds extraneous new lines in `project.dependencies` if the pyproject.toml has `CRLF` ending

---

_Issue opened by @Ravencentric on 2024-09-22 16:02_

I encountered this when I ported my existing `pyproject.toml` to use uv on Windows 11. Here's the reproduction with powershell on Windows 11:

```powershell
❯ uv --version
uv 0.4.15 (0d81bfbc6 2024-09-21)
```

Setup a dummy project:

```powershell
❯ uv init hello-world
Initialized project `hello-world` at `C:\Users\raven\Documents\GitHub\hello-world`
```

```powershell
❯ cd .\hello-world\
```

```powershell
❯ uv add pydantic httpx tomli requests typing-extensions wheel importlib-metadata
Using Python 3.12.4 interpreter at: C:\Users\raven\AppData\Local\Programs\Python\Python312\python.exe
Creating virtual environment at: .venv
Resolved 19 packages in 19ms
Installed 18 packages in 50ms
 + annotated-types==0.7.0
 + anyio==4.6.0
 + certifi==2024.8.30
 + charset-normalizer==3.3.2
 + h11==0.14.0
 + httpcore==1.0.5
 + httpx==0.27.2
 + idna==3.10
 + importlib-metadata==8.5.0
 + pydantic==2.9.2
 + pydantic-core==2.23.4
 + requests==2.32.3
 + sniffio==1.3.1
 + tomli==2.0.1
 + typing-extensions==4.12.2
 + urllib3==2.2.3
 + wheel==0.44.0
 + zipp==3.20.2
```

Everything looks fine here, uv created pyproject.toml file ends with `LF` on windows:
```powershell
❯ cat .\pyproject.toml
[project]
name = "hello-world"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "httpx>=0.27.2",
    "importlib-metadata>=8.5.0",
    "pydantic>=2.9.2",
    "requests>=2.32.3",
    "tomli>=2.0.1",
    "typing-extensions>=4.12.2",
    "wheel>=0.44.0",
]
```

Add another dep, still fine, still ends with LF.
```powershell
❯ uv add pyyaml
Resolved 20 packages in 20ms
Installed 1 package in 9ms
 + pyyaml==6.0.2
```

```powershell
❯ cat .\pyproject.toml
[project]
name = "hello-world"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "httpx>=0.27.2",
    "importlib-metadata>=8.5.0",
    "pydantic>=2.9.2",
    "pyyaml>=6.0.2",
    "requests>=2.32.3",
    "tomli>=2.0.1",
    "typing-extensions>=4.12.2",
    "wheel>=0.44.0",
]
```

Change LF to CRLF:
```powershell
❯ $text = Get-Content .\pyproject.toml
❯ echo $text > .\pyproject.toml
```

Check again:
```powershell
❯ cat .\pyproject.toml
[project]
name = "hello-world"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "httpx>=0.27.2",
    "importlib-metadata>=8.5.0",
    "pydantic>=2.9.2",
    "pyyaml>=6.0.2",
    "requests>=2.32.3",
    "tomli>=2.0.1",
    "typing-extensions>=4.12.2",
    "wheel>=0.44.0",
]
```

Add a new dep
```powershell
❯ uv add attrs
Resolved 21 packages in 229ms
Prepared 1 package in 168ms
Installed 1 package in 11ms
 + attrs==24.2.0
```


Extraneous new lines got added and the ending got changed to LF
```powershell
❯ cat .\pyproject.toml
[project]
name = "hello-world"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [

    "attrs>=24.2.0",

    "httpx>=0.27.2",

    "importlib-metadata>=8.5.0",

    "pydantic>=2.9.2",

    "pyyaml>=6.0.2",

    "requests>=2.32.3",

    "tomli>=2.0.1",

    "typing-extensions>=4.12.2",

    "wheel>=0.44.0",
]
```


---

_Comment by @charliermarsh on 2024-09-23 01:36_

Oh interesting, thank you for this.

---

_Label `bug` added by @charliermarsh on 2024-09-23 01:36_

---

_Label `windows` added by @charliermarsh on 2024-09-23 01:36_

---

_Label `good first issue` added by @charliermarsh on 2024-09-23 02:51_

---

_Label `help wanted` added by @charliermarsh on 2024-09-23 02:51_

---

_Comment by @my1e5 on 2024-09-23 09:33_

This bug also affects `tool.uv.dev-dependencies` in the same way.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-23 12:32_

---

_Closed by @charliermarsh on 2024-09-23 13:58_

---

_Closed by @charliermarsh on 2024-09-23 13:58_

---
