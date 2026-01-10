---
number: 15155
title: "`uv remove` does not delete packages in the virtual environment."
type: issue
state: closed
author: Mefamex
labels:
  - question
assignees: []
created_at: 2025-08-07T22:27:50Z
updated_at: 2025-08-09T14:29:00Z
url: https://github.com/astral-sh/uv/issues/15155
synced_at: 2026-01-10T01:25:53Z
---

# `uv remove` does not delete packages in the virtual environment.

---

_Issue opened by @Mefamex on 2025-08-07 22:27_

### Summary

`uv remove <package>` doesn't remove some packages in the virtual environment. It only removes them from the project's dependencies.

To completely remove them from the virtual environment, we have to use `uv pip uninstall <package>`.

Is there a solution or suggestion for this?

### Example

(.venv) D:\SOFTWARE\PYTHON\Projects\noone>**uv remove pip**
Resolved 1 package in 15ms
Audited in 0.06ms

(.venv) D:\SOFTWARE\PYTHON\Projects\noone>**uv remove pip**
error: The dependency `pip` could not be found in `project.dependencies`

(.venv) D:\SOFTWARE\PYTHON\Projects\noone>**uv pip uninstall pip**
Uninstalled 1 package in 36ms
 - pip==25.2

(.venv) D:\SOFTWARE\PYTHON\Projects\noone>**uv pip uninstall pip**
warning: Skipping pip as it is not installed
warning: No packages to uninstall

---

_Label `enhancement` added by @Mefamex on 2025-08-07 22:27_

---

_Comment by @zanieb on 2025-08-08 00:55_

Can you share a reproducible example? e.g., starting with `uv init`?

---

_Comment by @Mefamex on 2025-08-08 12:34_

## COMMAND (win-bat)

```batch

@ECHO off 

ECHO.&&ECHO.&&ECHO Check uv 
uv self version && python.exe -m uv self version



ECHO.&&ECHO.&&ECHO create folder
mkdir uv_issue
cd uv_issue && cd



ECHO.&&ECHO.&&ECHO create virtual env safely
python.exe -m venv .venv
dir /B


ECHO.&&ECHO.&&ECHO include UV
uv init 



ECHO.&&ECHO.&&ECHO check files before changes
more pyproject.toml
dir .venv\Lib\site-packages /B 


ECHO.&&ECHO.&&ECHO add a package
uv add pip 

ECHO.&&ECHO.&&ECHO check the existence of the package in the project
more pyproject.toml
dir .venv\Lib\site-packages /B 

ECHO.&&ECHO.&&ECHO maybe I dont want it in my project
uv remove pip 

ECHO.&&ECHO.&&ECHO Why do they still have folders?
more pyproject.toml
dir .venv\Lib\site-packages /B 



ECHO.&&ECHO.&&ECHO Let's try another package
uv add uv 
uv remove uv 
more pyproject.toml
dir .venv\Lib\site-packages /B 

ECHO.&&ECHO.&&ECHO its same 



ECHO.&&ECHO.&&ECHO But 'uv remove' only does not work on some packages. 

ECHO.&&ECHO.&&ECHO for example its work on another package

uv add wheel
uv remove wheel
more pyproject.toml
dir .venv\Lib\site-packages /B 


ECHO.&&ECHO.&&ECHO Why are some package files not removed from the 'site-packages' folder 
ECHO.&&ECHO.&&ECHO                               even though they are removed from 'pyproject.toml'?

```

<br><br>

## OUTPUT:
```


Check uv
uv 0.8.6 (329a6b446 2025-08-07)
uv 0.8.6 (329a6b446 2025-08-07)


create folder


create virtual env safely


include UV
Initialized project `uv-issue`


check files before changes
[project]
name = "uv-issue"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = []
pip
pip-25.1.1.dist-info


add a package
Resolved 2 packages in 6ms
Uninstalled 1 package in 101ms
Installed 1 package in 67ms
 - pip==25.1.1
 + pip==25.2


check the existence of the package in the project
[project]
name = "uv-issue"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "pip>=25.2",
]
pip
pip-25.2.dist-info


maybe I dont want it in my project
Resolved 1 package in 6ms
Audited in 0.08ms


Why do they still have folders?
[project]
name = "uv-issue"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = []
pip
pip-25.2.dist-info


Let's try another package
Resolved 2 packages in 306ms
Installed 1 package in 9ms
 + uv==0.8.6
Resolved 1 package in 5ms
Audited in 0.09ms
[project]
name = "uv-issue"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = []
pip
pip-25.2.dist-info
uv
uv-0.8.6.dist-info


its same


But 'uv remove' only does not work on some packages.


for example its work on another package
Resolved 2 packages in 7ms
Installed 1 package in 25ms
 + wheel==0.45.1
Resolved 1 package in 4ms
Uninstalled 1 package in 3ms
 - wheel==0.45.1
[project]
name = "uv-issue"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = []
pip
pip-25.2.dist-info
uv
uv-0.8.6.dist-info


Why are some package files not removed from the 'site-packages' folder
                   even though they are removed from 'pyproject.toml'?
```


---

_Comment by @zanieb on 2025-08-08 12:39_

We don't remove `pip` here because it's seeded by `python -m venv`. It's already present in the environment and not managed by uv. If you didn't use `python -m venv`, we would remove it.

```
❯ uv init example
Initialized project `example` at `/Users/zb/workspace/uv/example`
❯ cd example
❯ uv add pip
Using CPython 3.13.2
Creating virtual environment at: .venv
Resolved 2 packages in 77ms
Prepared 1 package in 278ms
Installed 1 package in 9ms
 + pip==25.2
❯ uv remove pip
Resolved 1 package in 4ms
Uninstalled 1 package in 41ms
 - pip==25.2
❯ rm -rf .venv
❯ uvx python -m venv .venv
❯ uv add pip
Resolved 2 packages in 9ms
Uninstalled 1 package in 50ms
Installed 1 package in 12ms
 - pip==24.3.1
 + pip==25.2
❯ uv remove pip
Resolved 1 package in 2ms
Audited in 0.04ms
 ```

---

_Label `enhancement` removed by @zanieb on 2025-08-08 12:40_

---

_Label `question` added by @zanieb on 2025-08-08 12:40_

---

_Comment by @zanieb on 2025-08-08 12:40_

(There's no reason to create the venv ahead of time, uv will do that for you)

---

_Comment by @Mefamex on 2025-08-09 11:07_

# Thank you, I understand now.

The issue occurs because `pip` was pre-installed by `python -m venv` and therefore isn't managed by uv. Since uv didn't install it originally, `uv remove` only removes it from the project dependencies but leaves the physical package untouched in the virtual environment.

## Regarding my workflow choice:

I prefer using `python -m venv .venv` initially because I want to maintain full control over the virtual environment creation process and ensure compatibility with standard Python tooling.


## Alternative hybrid approach:

For projects where I want the speed benefits of uv for package management while maintaining a standard venv setup, this workflow works well:

```bash
python.exe -m venv .venv
.venv\Scripts\activate
uv pip install -r requirements.txt
```

This approach gives me:
- Fast package installation via uv
- Standard Python virtual environment
- Compatibility with existing pip-based workflows
- No project structure changes imposed by uv


# Trade-offs to consider

**However, I recognize this is a trade-off.** While this hybrid approach works for my current needs, it means missing out on uv's advanced features like automatic dependency resolution, lock files (though I could still use `pip freeze` if needed), and integrated project management. For teams or projects that can fully adopt uv's ecosystem, using `uv init` and letting uv manage the entire environment would likely be more efficient.

The key insight here is understanding which packages uv manages versus which are pre-existing in the environment.


---

_Closed by @Mefamex on 2025-08-09 11:07_

---

_Comment by @zanieb on 2025-08-09 14:29_

>  I want to maintain full control over the virtual environment creation process and ensure compatibility with standard Python tooling.

You can use `uv venv`. We follow all of the standards for virtual environment creation and the environment is usable with any other tools.

---
