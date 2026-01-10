```yaml
number: 2445
title: pyinstaller installed by uv cannot work on Windows
type: issue
state: closed
author: du33169
labels:
  - bug
  - windows
assignees: []
created_at: 2024-03-14T03:17:07Z
updated_at: 2024-03-21T12:53:51Z
url: https://github.com/astral-sh/uv/issues/2445
synced_at: 2026-01-10T05:40:32Z
```

# pyinstaller installed by uv cannot work on Windows

---

_Issue opened by @du33169 on 2024-03-14 03:17_

**Description:**
I try to install pyinstaller using uv, the installation is successful but pyinstaller doesn't work.
(Pyinstaller installed via pip is working well).

- **uv version**: uv 0.1.18 (43dc9c87a 2024-03-13)
- **platform**: Windows 10 x86-64
- **python version**: 3.7.9

**Invoked Commands:**
```powershell
uv venv uvenv
.\uvenv\Scripts\activate
uv pip install -nv pyinstaller
pyinstaller --help
```

**Expected Output:**

Normal help message

**Actual Output:**
```powershell
  File "C:\Users\username\Desktop\uvtest\uvenv\Scripts\pyinstaller.exe", line 1
SyntaxError: Non-UTF-8 code starting with '\x90' in file C:\Users\username\Desktop\uvtest\uvenv\Scripts\pyinstaller.exe on line 1, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details
```

**Verbose Log:**
<details>
<summary>uv output</summary>

```powershell
uv : DEBUG Found a virtualenv through VIRTUAL_ENV at: C:\Users\username\Desktop\uvtest\uvenv
DEBUG Probing interpreter info for: \\?\C:\Users\username\Desktop\uvtest\uvenv\Scripts\python.exe
DEBUG Found Python 3.7.9 for: \\?\C:\Users\username\Desktop\uvtest\uvenv\Scripts\python.exe
DEBUG Using Python 3.7.9 environment at [36mC:\Users\username\Desktop\uvtest\uvenv\Scripts\python.exe[39m
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.7.9
DEBUG Adding direct dependency: pyinstaller*
DEBUG No cache entry for: https://pypi.org/simple/pyinstaller/
DEBUG Searching for a compatible version of pyinstaller (*)
DEBUG No compatible version found for: Python
DEBUG Searching for a compatible version of pyinstaller (<6.5.0 | >6.5.0)
DEBUG Searching for a compatible version of pyinstaller (<6.4.0 | >6.4.0, <6.5.0 | >6.5.0)
DEBUG Searching for a compatible version of pyinstaller (<6.3.0 | >6.3.0, <6.4.0 | >6.4.0, <6.5.0 | >6.5.0)
DEBUG Searching for a compatible version of pyinstaller (<6.2.0 | >6.2.0, <6.3.0 | >6.3.0, <6.4.0 | >6.4.0, <6.5.0 | >6.5.0)
DEBUG Searching for a compatible version of pyinstaller (<6.1.0 | >6.1.0, <6.2.0 | >6.2.0, <6.3.0 | >6.3.0, <6.4.0 | >6.4.0, <6.5.0 | >6.5.0)
DEBUG Searching for a compatible version of pyinstaller (<6.0.0 | >6.0.0, <6.1.0 | >6.1.0, <6.2.0 | >6.2.0, <6.3.0 | >6.3.0, <6.4.0 | >6.4.0, 
<6.5.0 | >6.5.0)
DEBUG Selecting: pyinstaller==5.13.2 (pyinstaller-5.13.2-py3-none-win_amd64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/1a/74/c65be869ae47649b98a928b3122c27d72fe372138aab4dc6cdd42e217d8a/pyinstall
er-5.13.2-py3-none-win_amd64.whl.metadata
DEBUG Adding transitive dependency: setuptools>=42.0.0
DEBUG Adding transitive dependency: altgraph*
DEBUG Adding transitive dependency: pyinstaller-hooks-contrib>=2021.4
DEBUG Adding transitive dependency: importlib-metadata>=1.4
DEBUG Adding transitive dependency: pefile>=2022.5.30
DEBUG Adding transitive dependency: pywin32-ctypes>=0.2.1
DEBUG No cache entry for: https://pypi.org/simple/setuptools/
DEBUG No cache entry for: https://pypi.org/simple/altgraph/
DEBUG No cache entry for: https://pypi.org/simple/pyinstaller-hooks-contrib/
DEBUG No cache entry for: https://pypi.org/simple/importlib-metadata/
DEBUG No cache entry for: https://pypi.org/simple/pefile/
DEBUG No cache entry for: https://pypi.org/simple/pywin32-ctypes/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d4/bc/7e593b05aecd3dcaa5a96ba567662ebae2384a27ae30d5aa03973249f19e/pyinstall
er_hooks_contrib-2024.3-py2.py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a4/bc/78b2c00cc64c31dbb3be42a0e8600bcebc123ad338c3b714754d668c7c2d/pywin32_c
types-0.2.2-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/55/26/d0ad8b448476d0a1e8d3ea5622dc77b916db84c6aa3cb1e1c0965af948fc/pefile-20
23.2.7-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/4d/3f/3bc3f1d83f6e4a7fcb834d3720544ca597590425be5ba9db032b2bf322a2/altgraph-
0.17.4-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of setuptools (>=42.0.0)
DEBUG No compatible version found for: Python
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <69.2.0 | >69.2.0)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <69.1.1 | >69.1.1, <69.2.0 | >69.2.0)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <69.1.0 | >69.1.0, <69.1.1 | >69.1.1, <69.2.0 | >69.2.0)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <69.0.3 | >69.0.3, <69.1.0 | >69.1.0, <69.1.1 | >69.1.1, <69.2.0 | >69.2.0)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <69.0.2 | >69.0.2, <69.0.3 | >69.0.3, <69.1.0 | >69.1.0, <69.1.1 | >69.1.1, 
<69.2.0 | >69.2.0)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <69.0.1 | >69.0.1, <69.0.2 | >69.0.2, <69.0.3 | >69.0.3, <69.1.0 | >69.1.0, 
<69.1.1 | >69.1.1, <69.2.0 | >69.2.0)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <69.0.0 | >69.0.0, <69.0.1 | >69.0.1, <69.0.2 | >69.0.2, <69.0.3 | >69.0.3, 
<69.1.0 | >69.1.0, <69.1.1 | >69.1.1, <69.2.0 | >69.2.0)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <68.2.2 | >68.2.2, <69.0.0 | >69.0.0, <69.0.1 | >69.0.1, <69.0.2 | >69.0.2, 
<69.0.3 | >69.0.3, <69.1.0 | >69.1.0, <69.1.1 | >69.1.1, <69.2.0 | >69.2.0)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <68.2.1 | >68.2.1, <68.2.2 | >68.2.2, <69.0.0 | >69.0.0, <69.0.1 | >69.0.1, 
<69.0.2 | >69.0.2, <69.0.3 | >69.0.3, <69.1.0 | >69.1.0, <69.1.1 | >69.1.1, <69.2.0 | >69.2.0)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <68.2.0 | >68.2.0, <68.2.1 | >68.2.1, <68.2.2 | >68.2.2, <69.0.0 | >69.0.0, 
<69.0.1 | >69.0.1, <69.0.2 | >69.0.2, <69.0.3 | >69.0.3, <69.1.0 | >69.1.0, <69.1.1 | >69.1.1, <69.2.0 | >69.2.0)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <68.1.2 | >68.1.2, <68.2.0 | >68.2.0, <68.2.1 | >68.2.1, <68.2.2 | >68.2.2, 
<69.0.0 | >69.0.0, <69.0.1 | >69.0.1, <69.0.2 | >69.0.2, <69.0.3 | >69.0.3, <69.1.0 | >69.1.0, <69.1.1 | >69.1.1, <69.2.0 | >69.2.0)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <68.1.0 | >68.1.0, <68.1.2 | >68.1.2, <68.2.0 | >68.2.0, <68.2.1 | >68.2.1, 
<68.2.2 | >68.2.2, <69.0.0 | >69.0.0, <69.0.1 | >69.0.1, <69.0.2 | >69.0.2, <69.0.3 | >69.0.3, <69.1.0 | >69.1.0, <69.1.1 | >69.1.1, <69.2.0 |
 >69.2.0)
DEBUG Selecting: setuptools==68.0.0 (setuptools-68.0.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/c7/42/be1c7bbdd83e1bfb160c94b9cafd8e25efc7400346cf7ccdbdb452c467fa/setuptool
s-68.0.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of altgraph (*)
DEBUG Selecting: altgraph==0.17.4 (altgraph-0.17.4-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of pyinstaller-hooks-contrib (>=2021.4)
DEBUG Selecting: pyinstaller-hooks-contrib==2024.3 (pyinstaller_hooks_contrib-2024.3-py2.py3-none-any.whl)
DEBUG Adding transitive dependency: setuptools>=42.0.0
DEBUG Adding transitive dependency: packaging>=22.0
DEBUG Adding transitive dependency: importlib-metadata>=4.6
DEBUG Searching for a compatible version of importlib-metadata (>=4.6)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <68.1.0 | >68.1.0, <68.1.2 | >68.1.2, <68.2.0 | >68.2.0, <68.2.1 | >68.2.1, 
<68.2.2 | >68.2.2, <69.0.0 | >69.0.0, <69.0.1 | >69.0.1, <69.0.2 | >69.0.2, <69.0.3 | >69.0.3, <69.1.0 | >69.1.0, <69.1.1 | >69.1.1, <69.2.0 |
 >69.2.0)
DEBUG Selecting: setuptools==68.0.0 (setuptools-68.0.0-py3-none-any.whl)
DEBUG Searching for a compatible version of altgraph (*)
DEBUG Selecting: altgraph==0.17.4 (altgraph-0.17.4-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of pyinstaller-hooks-contrib (>=2021.4)
DEBUG Selecting: pyinstaller-hooks-contrib==2024.3 (pyinstaller_hooks_contrib-2024.3-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (>=4.6, <7.0.2 | >7.0.2)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <68.1.0 | >68.1.0, <68.1.2 | >68.1.2, <68.2.0 | >68.2.0, <68.2.1 | >68.2.1, 
<68.2.2 | >68.2.2, <69.0.0 | >69.0.0, <69.0.1 | >69.0.1, <69.0.2 | >69.0.2, <69.0.3 | >69.0.3, <69.1.0 | >69.1.0, <69.1.1 | >69.1.1, <69.2.0 |
 >69.2.0)
DEBUG Selecting: setuptools==68.0.0 (setuptools-68.0.0-py3-none-any.whl)
DEBUG Searching for a compatible version of altgraph (*)
DEBUG Selecting: altgraph==0.17.4 (altgraph-0.17.4-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of pyinstaller-hooks-contrib (>=2021.4)
DEBUG Selecting: pyinstaller-hooks-contrib==2024.3 (pyinstaller_hooks_contrib-2024.3-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (>=4.6, <7.0.1 | >7.0.1, <7.0.2 | >7.0.2)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <68.1.0 | >68.1.0, <68.1.2 | >68.1.2, <68.2.0 | >68.2.0, <68.2.1 | >68.2.1, 
<68.2.2 | >68.2.2, <69.0.0 | >69.0.0, <69.0.1 | >69.0.1, <69.0.2 | >69.0.2, <69.0.3 | >69.0.3, <69.1.0 | >69.1.0, <69.1.1 | >69.1.1, <69.2.0 |
 >69.2.0)
DEBUG Selecting: setuptools==68.0.0 (setuptools-68.0.0-py3-none-any.whl)
DEBUG Searching for a compatible version of altgraph (*)
DEBUG Selecting: altgraph==0.17.4 (altgraph-0.17.4-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of pyinstaller-hooks-contrib (>=2021.4)
DEBUG Selecting: pyinstaller-hooks-contrib==2024.3 (pyinstaller_hooks_contrib-2024.3-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (>=4.6, <7.0.0 | >7.0.0, <7.0.1 | >7.0.1, <7.0.2 | >7.0.2)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <68.1.0 | >68.1.0, <68.1.2 | >68.1.2, <68.2.0 | >68.2.0, <68.2.1 | >68.2.1, 
<68.2.2 | >68.2.2, <69.0.0 | >69.0.0, <69.0.1 | >69.0.1, <69.0.2 | >69.0.2, <69.0.3 | >69.0.3, <69.1.0 | >69.1.0, <69.1.1 | >69.1.1, <69.2.0 |
 >69.2.0)
DEBUG Selecting: setuptools==68.0.0 (setuptools-68.0.0-py3-none-any.whl)
DEBUG Searching for a compatible version of altgraph (*)
DEBUG Selecting: altgraph==0.17.4 (altgraph-0.17.4-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of pyinstaller-hooks-contrib (>=2021.4)
DEBUG Selecting: pyinstaller-hooks-contrib==2024.3 (pyinstaller_hooks_contrib-2024.3-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (>=4.6, <6.11.0 | >6.11.0, <7.0.0 | >7.0.0, <7.0.1 | >7.0.1, <7.0.2 | >7.0.2)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <68.1.0 | >68.1.0, <68.1.2 | >68.1.2, <68.2.0 | >68.2.0, <68.2.1 | >68.2.1, 
<68.2.2 | >68.2.2, <69.0.0 | >69.0.0, <69.0.1 | >69.0.1, <69.0.2 | >69.0.2, <69.0.3 | >69.0.3, <69.1.0 | >69.1.0, <69.1.1 | >69.1.1, <69.2.0 |
 >69.2.0)
DEBUG Selecting: setuptools==68.0.0 (setuptools-68.0.0-py3-none-any.whl)
DEBUG Searching for a compatible version of altgraph (*)
DEBUG Selecting: altgraph==0.17.4 (altgraph-0.17.4-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of pyinstaller-hooks-contrib (>=2021.4)
DEBUG Selecting: pyinstaller-hooks-contrib==2024.3 (pyinstaller_hooks_contrib-2024.3-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (>=4.6, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7.0.0 | >7.0.0, <7.0.1 | >7.0.1,
 <7.0.2 | >7.0.2)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <68.1.0 | >68.1.0, <68.1.2 | >68.1.2, <68.2.0 | >68.2.0, <68.2.1 | >68.2.1, 
<68.2.2 | >68.2.2, <69.0.0 | >69.0.0, <69.0.1 | >69.0.1, <69.0.2 | >69.0.2, <69.0.3 | >69.0.3, <69.1.0 | >69.1.0, <69.1.1 | >69.1.1, <69.2.0 |
 >69.2.0)
DEBUG Selecting: setuptools==68.0.0 (setuptools-68.0.0-py3-none-any.whl)
DEBUG Searching for a compatible version of altgraph (*)
DEBUG Selecting: altgraph==0.17.4 (altgraph-0.17.4-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of pyinstaller-hooks-contrib (>=2021.4)
DEBUG Selecting: pyinstaller-hooks-contrib==2024.3 (pyinstaller_hooks_contrib-2024.3-py2.py3-none-any.whl)
DEBUG No cache entry for: https://pypi.org/simple/packaging/
DEBUG Searching for a compatible version of importlib-metadata (>=4.6, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0, <7.0.0 | >7.0.0,
 <7.0.1 | >7.0.1, <7.0.2 | >7.0.2)
DEBUG Searching for a compatible version of setuptools (>=42.0.0, <68.1.0 | >68.1.0, <68.1.2 | >68.1.2, <68.2.0 | >68.2.0, <68.2.1 | >68.2.1, 
<68.2.2 | >68.2.2, <69.0.0 | >69.0.0, <69.0.1 | >69.0.1, <69.0.2 | >69.0.2, <69.0.3 | >69.0.3, <69.1.0 | >69.1.0, <69.1.1 | >69.1.1, <69.2.0 |
 >69.2.0)
DEBUG Selecting: setuptools==68.0.0 (setuptools-68.0.0-py3-none-any.whl)
DEBUG Searching for a compatible version of altgraph (*)
DEBUG Selecting: altgraph==0.17.4 (altgraph-0.17.4-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of pyinstaller-hooks-contrib (>=2021.4)
DEBUG Selecting: pyinstaller-hooks-contrib==2024.3 (pyinstaller_hooks_contrib-2024.3-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of importlib-metadata (>=4.6, <6.8.0 | >6.8.0, <6.9.0 | >6.9.0, <6.10.0 | >6.10.0, <6.11.0 | >6.11.0,
 <7.0.0 | >7.0.0, <7.0.1 | >7.0.1, <7.0.2 | >7.0.2)
DEBUG Selecting: importlib-metadata==6.7.0 (importlib_metadata-6.7.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ff/94/64287b38c7de4c90683630338cf28f129decbba0a44f0c6db35a873c73c4/importlib
_metadata-6.7.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency: zipp>=0.5
DEBUG Adding transitive dependency: typing-extensions>=3.6.4
DEBUG Searching for a compatible version of pefile (>=2022.5.30)
DEBUG Selecting: pefile==2023.2.7 (pefile-2023.2.7-py3-none-any.whl)
DEBUG Searching for a compatible version of pywin32-ctypes (>=0.2.1)
DEBUG Selecting: pywin32-ctypes==0.2.2 (pywin32_ctypes-0.2.2-py3-none-any.whl)
DEBUG No cache entry for: https://pypi.org/simple/zipp/
DEBUG No cache entry for: https://pypi.org/simple/typing-extensions/
DEBUG Searching for a compatible version of packaging (>=22.0)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/49/df/1fceb2f8900f8639e278b056416d49134fb8d84c5942ffaa01ad34782422/packaging
-24.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of zipp (>=0.5)
DEBUG Searching for a compatible version of pefile (>=2022.5.30)
DEBUG Selecting: pefile==2023.2.7 (pefile-2023.2.7-py3-none-any.whl)
DEBUG Searching for a compatible version of pywin32-ctypes (>=0.2.1)
DEBUG Selecting: pywin32-ctypes==0.2.2 (pywin32_ctypes-0.2.2-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=22.0)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.18.0 | >3.18.0)
DEBUG Searching for a compatible version of pefile (>=2022.5.30)
DEBUG Selecting: pefile==2023.2.7 (pefile-2023.2.7-py3-none-any.whl)
DEBUG Searching for a compatible version of pywin32-ctypes (>=0.2.1)
DEBUG Selecting: pywin32-ctypes==0.2.2 (pywin32_ctypes-0.2.2-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=22.0)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0)
DEBUG Searching for a compatible version of pefile (>=2022.5.30)
DEBUG Selecting: pefile==2023.2.7 (pefile-2023.2.7-py3-none-any.whl)
DEBUG Searching for a compatible version of pywin32-ctypes (>=0.2.1)
DEBUG Selecting: pywin32-ctypes==0.2.2 (pywin32_ctypes-0.2.2-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=22.0)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0)
DEBUG Searching for a compatible version of pefile (>=2022.5.30)
DEBUG Selecting: pefile==2023.2.7 (pefile-2023.2.7-py3-none-any.whl)
DEBUG Searching for a compatible version of pywin32-ctypes (>=0.2.1)
DEBUG Selecting: pywin32-ctypes==0.2.2 (pywin32_ctypes-0.2.2-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=22.0)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 | >3.18.0)
DEBUG Searching for a compatible version of pefile (>=2022.5.30)
DEBUG Selecting: pefile==2023.2.7 (pefile-2023.2.7-py3-none-any.whl)
DEBUG Searching for a compatible version of pywin32-ctypes (>=0.2.1)
DEBUG Selecting: pywin32-ctypes==0.2.2 (pywin32_ctypes-0.2.2-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=22.0)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 |
 >3.18.0)
DEBUG Selecting: zipp==3.15.0 (zipp-3.15.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/5b/fa/c9e82bbe1af6266adf08afb563905eb87cab83fde00a0a08963510621047/zipp-3.15
.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of typing-extensions (>=3.6.4)
DEBUG Searching for a compatible version of pefile (>=2022.5.30)
DEBUG Selecting: pefile==2023.2.7 (pefile-2023.2.7-py3-none-any.whl)
DEBUG Searching for a compatible version of pywin32-ctypes (>=0.2.1)
DEBUG Selecting: pywin32-ctypes==0.2.2 (pywin32_ctypes-0.2.2-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=22.0)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 |
 >3.18.0)
DEBUG Selecting: zipp==3.15.0 (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (>=3.6.4, <4.10.0 | >4.10.0)
DEBUG Searching for a compatible version of pefile (>=2022.5.30)
DEBUG Selecting: pefile==2023.2.7 (pefile-2023.2.7-py3-none-any.whl)
DEBUG Searching for a compatible version of pywin32-ctypes (>=0.2.1)
DEBUG Selecting: pywin32-ctypes==0.2.2 (pywin32_ctypes-0.2.2-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=22.0)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 |
 >3.18.0)
DEBUG Selecting: zipp==3.15.0 (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (>=3.6.4, <4.9.0 | >4.9.0, <4.10.0 | >4.10.0)
DEBUG Searching for a compatible version of pefile (>=2022.5.30)
DEBUG Selecting: pefile==2023.2.7 (pefile-2023.2.7-py3-none-any.whl)
DEBUG Searching for a compatible version of pywin32-ctypes (>=0.2.1)
DEBUG Selecting: pywin32-ctypes==0.2.2 (pywin32_ctypes-0.2.2-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=22.0)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of zipp (>=0.5, <3.16.0 | >3.16.0, <3.16.1 | >3.16.1, <3.16.2 | >3.16.2, <3.17.0 | >3.17.0, <3.18.0 |
 >3.18.0)
DEBUG Selecting: zipp==3.15.0 (zipp-3.15.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typing-extensions (>=3.6.4, <4.8.0 | >4.8.0, <4.9.0 | >4.9.0, <4.10.0 | >4.10.0)
DEBUG Selecting: typing-extensions==4.7.1 (typing_extensions-4.7.1-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ec/6b/63cc3df74987c36fe26157ee12e09e8f9db4de771e0f3404263117e75b95/typing_ex
tensions-4.7.1-py3-none-any.whl.metadata
Resolved 10 packages in 4.39s
DEBUG Identified uncached requirement: altgraph ==0.17.4
DEBUG Identified uncached requirement: importlib-metadata ==6.7.0
DEBUG Identified uncached requirement: packaging ==24.0
DEBUG Identified uncached requirement: pefile ==2023.2.7
DEBUG Identified uncached requirement: pyinstaller ==5.13.2
DEBUG Identified uncached requirement: pyinstaller-hooks-contrib ==2024.3
DEBUG Identified uncached requirement: pywin32-ctypes ==0.2.2
DEBUG Identified uncached requirement: setuptools ==68.0.0
DEBUG Identified uncached requirement: typing-extensions ==4.7.1
DEBUG Identified uncached requirement: zipp ==3.15.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/1a/74/c65be869ae47649b98a928b3122c27d72fe372138aab4dc6cdd42e217d8a/pyinstall
er-5.13.2-py3-none-win_amd64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/c7/42/be1c7bbdd83e1bfb160c94b9cafd8e25efc7400346cf7ccdbdb452c467fa/setuptool
s-68.0.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/49/df/1fceb2f8900f8639e278b056416d49134fb8d84c5942ffaa01ad34782422/packaging
-24.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/55/26/d0ad8b448476d0a1e8d3ea5622dc77b916db84c6aa3cb1e1c0965af948fc/pefile-20
23.2.7-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/d4/bc/7e593b05aecd3dcaa5a96ba567662ebae2384a27ae30d5aa03973249f19e/pyinstall
er_hooks_contrib-2024.3-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ec/6b/63cc3df74987c36fe26157ee12e09e8f9db4de771e0f3404263117e75b95/typing_ex
tensions-4.7.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/a4/bc/78b2c00cc64c31dbb3be42a0e8600bcebc123ad338c3b714754d668c7c2d/pywin32_c
types-0.2.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ff/94/64287b38c7de4c90683630338cf28f129decbba0a44f0c6db35a873c73c4/importlib
_metadata-6.7.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/4d/3f/3bc3f1d83f6e4a7fcb834d3720544ca597590425be5ba9db032b2bf322a2/altgraph-
0.17.4-py2.py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/5b/fa/c9e82bbe1af6266adf08afb563905eb87cab83fde00a0a08963510621047/zipp-3.15
.0-py3-none-any.whl
Downloaded 10 packages in 1.42s
Installed 10 packages in 96ms
 + altgraph==0.17.4
 + importlib-metadata==6.7.0
 + packaging==24.0
 + pefile==2023.2.7
 + pyinstaller==5.13.2
 + pyinstaller-hooks-contrib==2024.3
 + pywin32-ctypes==0.2.2
 + setuptools==68.0.0
 + typing-extensions==4.7.1
 + zipp==3.15.0
```

</details>

**Addition Infomation:**

During PyInstaller installation, an executable file is created and placed in the `uvenv\Scripts` directory. The executable built using pip is significantly larger (103KB) compared to the one built using UV itself (only 18KB).

I investigated further by opening the PyInstaller executable (pyinstaller.exe) with a text editor and found some ASCII error messages within the file.

<details>
<summary>error in pyinstaller.exe</summary>

```txt
C:\Users\Micha\.rustup\toolchains\nightly-2024-01-23-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\alloc\src\raw_vec.rscapacity overflow  A @          A @   }       :     C:\Users\Micha\.rustup\toolchains\nightly-2024-01-23-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\alloc\src\alloc.rsmemory allocation of  bytes failed   CB @          XB @   
       ȁ @   {       ¢  
   C:\Users\Micha\.rustup\toolchains\nightly-2024-01-23-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\alloc\src\ffi\c_str.rs  B @            7   � called `Option::unwrap()` on a `None` value z @                  { @   00010203040506070809101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899range start index  out of range for slice of length     PD @          bD @   "src\bounce.rs""""Failed to set the file pointer to the end of the file.Failed to read the executable fileFailed to close file handle    ¨E @   
       A     Error from QueryInformationJobObjectError from SetInformationJobObjectFailed to spawn the python child processTEMP c:\ Error from AssignProcessToJobObjectError from GetExitCodeProcess     (couldn't format message)Child command line is not correctly null terminated.
Failed to get executable name (code: )
Executable name is not correctly null terminated.
Failed to open executable ''
Failed to get the size of the executable 'Magic number 'UVUV' not found at the end of the file. Did you append the magic number, the length and the path to the python executable at the end of the file?
Only paths with a length up to 32KBs are supported but the python path has a length of .
Python executable length missing. Did you write the length of the path to the Python executable before the Magic number?
Python executable path is not correctly null terminated.
The length of the python executable path exceeds the file size. Verify that the path length is appended to the end of the launcher script as a u32 in little endian.
Received error code: 
: with code  (failed to get error message)
(uv internal error) : panic at : (column ):  ¥οÿοÿοÿοÿ    ȉץ          ,K  ,1      ȉץ          ¼K  ¼1      ȉץ    
   (  Ћ  б                       RSDSֶj̘}3A媽¢*с   C:\Users\Micha\astral\puffin\crates\uv-trampoline\target\x86_64-pc-windows-msvc\release\deps\uv_trampoline_console.pdb                     GCTL   蜠 .text   謠 ·  .text$unlikely   @    .idata$5    A  
  .rdata  K     .rdata$voltmd   ,K  ܁  .rdata$zzzdbg   M  p  .xdata  xN  (   .idata$2     N     .idata$3    ¸N    .idata$4    ÀO  X  .idata$6     `     .bss     p  X  .pdata     `   .rsrc$01    `  x  .rsrc$02                     B  	 	B0p`ࠠ 0p` ¢   Bp`   2`    20p` r0p` 򋰊p	`ÀЄ° 20p`°   	 0P
p	`ÀЄ°   B0Pp`°     0Pp` R0p` b  
 M 0P
p	`ÀЄ°	  0
p	`ÀЄ°   Ҋ0	Pp`À° 0p`° B0`  
 
B	0p`À°   r`
 
򃰂p` p`¸N          þQ   @  °O          R  ø@                          ÀO      ҏ      ޏ      􏠠    P      P      "P      2P      FP      RP      `P      rP      P      P      ®P            ސ      øP      Q      $Q      4Q      NQ      fQ      ~Q      Q      ªQ      𑠠    đ      ґ      䑠             ¶Q              IGetProcessHeap  ݂HeapAlloc GetModuleFileNameA  õGetLastError  XSetLastError  w CreateFileA ၇etFileSizeEx KSetFilePointerEx  ؃ReadFile  C CloseHandle yGetCommandLineA eGetStartupInfoA USetHandleInformation   CreateProcessA   CreateJobObjectW  ºQueryInformationJobObject VSetInformationJobObject   AssignProcessToJobObject  gGetStdHandle  pSetStdHandle  ΁GetEnvironmentVariableA 0SetCurrentDirectoryA  SetConsoleCtrlHandler 儗aitForSingleObject сGetExitCodeProcess  WriteFile ÿMessageBoxA ExitProcess TFormatMessageA  ႈeapFree  䂈eapReAlloc kernel32.dll  user32.dll
```

</details>


---

_Label `bug` added by @charliermarsh on 2024-03-14 03:39_

---

_Label `windows` added by @charliermarsh on 2024-03-14 03:39_

---

_Comment by @charliermarsh on 2024-03-14 15:25_

Thanks! We'll take a look.

---

_Comment by @charliermarsh on 2024-03-14 15:26_

First step is I need to try and reproduce. Do you know if other executables installed via `uv` work on your machine?

---

_Comment by @du33169 on 2024-03-14 15:43_

No. Pyinstaller contains other executables like `pyi-makespec`, `pyi-bindepend`, etc. None of them worked.
I also installed other packages just now, for example, PySide2, and the executables(`pyside2-uic`,`pyside2-rcc`) won't work either.


---

_Comment by @charliermarsh on 2024-03-14 15:44_

Thanks, that makes sense, there's some issue getting the path on your machine.

---

_Comment by @MichaReiser on 2024-03-18 15:50_

Would you mind sharing the `C:\Users\username\Desktop\uvtest\uvenv\Scripts\pyinstaller.exe` file? I would like to look at open it in a HEX editor to inspect its content (we generate the exe dynamically and I wonder what goes wrong).

---

_Comment by @du33169 on 2024-03-18 15:53_

[pyinstaller.zip](https://github.com/astral-sh/uv/files/14638923/pyinstaller.zip)


---

_Comment by @MichaReiser on 2024-03-18 17:14_

Thanks. I'm not sure what's going wrong here. Running the above commands works successfully on my windows machine. I can even use your `pyinstaller.exe` and run it after patching the path to the python interpreter at the end of the file. 

How do you run the script? Do you use powershell or CMD.exe? What's the output of running `chgp` (should print your active code page)?

---

_Comment by @du33169 on 2024-03-19 03:13_

> How do you run the script? Do you use powershell or CMD.exe? What's the output of running `chgp` (should print your active code page)?

Powershell. `chcp` prints `936`.
I repeated the whole process after `chcp 65001` or `1252`, but still got the same error.

> Thanks. I'm not sure what's going wrong here. Running the above commands works successfully on my windows machine. I can even use your pyinstaller.exe and run it after patching the path to the python interpreter at the end of the file.

Are those error messages making sense? Normally they are not supposed to be left in `pyinstaller.exe`.





---

_Comment by @MichaReiser on 2024-03-19 07:32_

Okay that's interesting. I can reproduce with Python 3.7.9. Python 3.8.10 works fine. I quickly glanced over the Python 3.8 changelog to see if I can spot any changes to the zip module or encoding handling but couldn't find any.

The issue is also not specific to `pyinstaller` because I can reproduce the same behavior with black. 

---

_Comment by @MichaReiser on 2024-03-21 11:31_

After speaking to @charliermarsh I was informed that Python 3.7 isn't supported. See the [platform support](https://github.com/astral-sh/uv#platform-support) section in the readme. 

Could you try using Python 3.8 or newer?

---

_Comment by @du33169 on 2024-03-21 12:25_

Oh, I didn't see that earlier... Sorry for all the inconvenience. I tried Python 3.11 and uv works fine.
It might be good to give a warning when using unsupported Python version since uv has to [discover the python environment](https://github.com/astral-sh/uv#python-discovery) (Python 3.7 is still used as a example in this section, by the way). 
But that seems like another feature request, I'll close this one.
Sorry again and thank you for your patience.

---

_Closed by @du33169 on 2024-03-21 12:25_

---

_Reopened by @MichaReiser on 2024-03-21 12:52_

---

_Comment by @MichaReiser on 2024-03-21 12:53_

No worries and you're right. `uv` should do a better job at warning when it's used with an incompatible Python version. I'll create a separate issue to track this. Thanks for trying `uv`!

---

_Closed by @MichaReiser on 2024-03-21 12:53_

---
