```yaml
number: 3964
title: "``uv pip sync`` removes pip when syncing dependencies from a ``requirements.txt`` without pip"
type: issue
state: closed
author: FishAlchemist
labels:
  - question
assignees: []
created_at: 2024-06-02T10:42:00Z
updated_at: 2024-06-02T18:02:28Z
url: https://github.com/astral-sh/uv/issues/3964
synced_at: 2026-01-10T05:31:37Z
```

# ``uv pip sync`` removes pip when syncing dependencies from a ``requirements.txt`` without pip

---

_Issue opened by @FishAlchemist on 2024-06-02 10:42_

uv 0.2.5 (47db418ba 2024-05-28)

Is this the expected behavior? I'm concerned that UV might not handle some exceptional cases yet. Therefore, I always add the ``--seed`` flag when initializing a virtual environment with UV.  If this is indeed the expected behavior, I suppose I'll need to add pip to the ``requirements.in`` file, even though including pip in there seems a bit strange.

```powershell
❯ uv pip sync .\uv_requirements.txt --verbose
DEBUG Searching for Python interpreter in virtual environments
DEBUG Found CPython 3.10.11 at `C:\Users\user\Documents\source\temp\.venv\Scripts\python.exe` (active virtual environment)
DEBUG Using Python 3.10.11 environment at .venv\Scripts\python.exe
DEBUG Acquired lock for `.venv`
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.10.11
DEBUG Adding direct dependency: certifi==2024.6.2
DEBUG Adding direct dependency: charset-normalizer==3.3.2
DEBUG Adding direct dependency: idna==3.7
DEBUG Adding direct dependency: requests==2.32.3
DEBUG Adding direct dependency: urllib3==2.2.1
DEBUG Found fresh response for: https://pypi.org/simple/certifi/
DEBUG Found fresh response for: https://pypi.org/simple/idna/
DEBUG Searching for a compatible version of certifi (==2024.6.2)
DEBUG Found fresh response for: https://pypi.org/simple/requests/
DEBUG Found installed version of certifi==2024.6.2 that satisfies preference in ==2024.6.2
DEBUG Selecting: certifi==2024.6.2 (installed)
DEBUG Found fresh response for: https://pypi.org/simple/urllib3/
DEBUG Found fresh response for: https://pypi.org/simple/charset-normalizer/
DEBUG Searching for a compatible version of charset-normalizer (==3.3.2)
DEBUG Found installed version of charset-normalizer==3.3.2 that satisfies preference in ==3.3.2
DEBUG Selecting: charset-normalizer==3.3.2 (installed)
DEBUG Searching for a compatible version of idna (==3.7)
DEBUG Found installed version of idna==3.7 that satisfies preference in ==3.7
DEBUG Selecting: idna==3.7 (installed)
DEBUG Searching for a compatible version of requests (==2.32.3)
DEBUG Found installed version of requests==2.32.3 that satisfies preference in ==2.32.3
DEBUG Selecting: requests==2.32.3 (installed)
DEBUG Searching for a compatible version of urllib3 (==2.2.1)
DEBUG Found installed version of urllib3==2.2.1 that satisfies preference in ==2.2.1
DEBUG Selecting: urllib3==2.2.1 (installed)
DEBUG Tried 6 versions: certifi 1, charset-normalizer 1, idna 1, requests 1, root 1, urllib3 1
Resolved 5 packages in 5ms
DEBUG Requirement already installed: certifi==2024.6.2
DEBUG Requirement already installed: charset-normalizer==3.3.2
DEBUG Requirement already installed: idna==3.7
DEBUG Requirement already installed: requests==2.32.3
DEBUG Requirement already installed: urllib3==2.2.1
DEBUG Unnecessary package: build==1.2.1
DEBUG Unnecessary package: click==8.1.7
DEBUG Unnecessary package: colorama==0.4.6
DEBUG Unnecessary package: packaging==24.0
DEBUG Unnecessary package: pip==24.0
DEBUG Unnecessary package: pip-tools==7.4.1
DEBUG Unnecessary package: pyproject-hooks==1.1.0
DEBUG Unnecessary package: setuptools==70.0.0
DEBUG Unnecessary package: tomli==2.0.1
DEBUG Unnecessary package: wheel==0.43.0
DEBUG Uninstalled build (34 files, 5 directories)
DEBUG Uninstalled click (39 files, 3 directories)
DEBUG Uninstalled colorama (31 files, 6 directories)
DEBUG Uninstalled packaging (36 files, 3 directories)
DEBUG Uninstalled pip (529 files, 106 directories)
DEBUG Uninstalled pip-tools (55 files, 9 directories)
DEBUG Uninstalled pyproject-hooks (13 files, 5 directories)
DEBUG Uninstalled setuptools (494 files, 63 directories)
DEBUG Uninstalled tomli (14 files, 3 directories)
DEBUG Uninstalled wheel (62 files, 9 directories)
Uninstalled 10 packages in 176ms
 - build==1.2.1
 - click==8.1.7
 - colorama==0.4.6
 - packaging==24.0
 - pip==24.0
 - pip-tools==7.4.1
 - pyproject-hooks==1.1.0
 - setuptools==70.0.0
 - tomli==2.0.1
 - wheel==0.43.0
```
[uv_requirements.txt](https://github.com/user-attachments/files/15524953/uv_requirements.txt)


---

_Comment by @charliermarsh on 2024-06-02 16:49_

Yes, `uv pip sync` removes any dependencies that aren't in the requirements file. Are you looking for `uv pip install`?

---

_Label `question` added by @charliermarsh on 2024-06-02 16:49_

---

_Comment by @FishAlchemist on 2024-06-02 17:06_

> Yes, `uv pip sync` removes any dependencies that aren't in the requirements file. Are you looking for `uv pip install`?

I know ``uv pip install`` , but I recently wanted to try using ``uv pip compile`` and ``uv pip sync`` to manage my environment dependencies. 
However, I was surprised to find that ``uv pip sync`` removes **pip** if it is not specified in the dependency file. 
This is different from pip-tools's pip-sync, which does not remove **pip**.
Therefore, if I need to keep **pip**, **setuptools**, and **wheel** in my environment, I need to explicitly include them in my requirements.in file.
I need to keep them because I use them occasionally. For example, in a previous version of uv, I was unable to install PyTorch. Although this issue has been fixed in the new version, I still need them before it is fixed.

---

_Comment by @charliermarsh on 2024-06-02 17:12_

I think you should mark them as necessary dependencies then, yeah.


---

_Comment by @FishAlchemist on 2024-06-02 17:22_

> I think you should mark them as necessary dependencies then, yeah.

Since it is expected behavior, I will write them(**pip**, **setuptools**, and **wheel**) to ``requirements.in``.

---

_Closed by @FishAlchemist on 2024-06-02 17:22_

---

_Comment by @charliermarsh on 2024-06-02 17:43_

We actually do preserve them, I think, if you create the virtualenv with a tool _other_ than uv, with the assumption being that you need the seed packages in that case, but don't if you're using uv.

---

_Comment by @FishAlchemist on 2024-06-02 18:02_

> We actually do preserve them, I think, if you create the virtualenv with a tool _other_ than uv, with the assumption being that I 

Just tried other ways of creating a virtual environment, and **seed packages** are indeed retained.
When creating a virtual environment with UV, **seed packages** can be optionally installed. This suggests that there is a possibility of using UV but requiring **seed packages**.




---
