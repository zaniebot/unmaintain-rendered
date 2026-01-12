```yaml
number: 1276
title: "Cannot resolve imported module: `<module import>` ty 'unresolved-import' for packages installed in editable mode"
type: issue
state: closed
author: sitar777
labels:
  - imports
assignees: []
created_at: 2025-09-29T13:14:52Z
updated_at: 2025-09-29T18:08:06Z
url: https://github.com/astral-sh/ty/issues/1276
synced_at: 2026-01-12T15:54:24Z
```

# Cannot resolve imported module: `<module import>` ty 'unresolved-import' for packages installed in editable mode

---

_@sitar777_

### Summary

ty does not resolve imports to packages installed in editable mode in VSCode Language Server.

`pip install -e <package_path>`

### Version

ty 0.0.1-alpha.21

---

_Renamed from "Cannot resolve imported module: `<module import>` ty 'unresolved-import' for packages installed in in editable mode" to "Cannot resolve imported module: `<module import>` ty 'unresolved-import' for packages installed in editable mode" by @sitar777 on 2025-09-29 13:15_

---

_Label `needs-info` added by @sharkdp on 2025-09-29 13:25_

---

_Label `imports` added by @sharkdp on 2025-09-29 13:25_

---

_Comment by @sharkdp on 2025-09-29 13:26_

Thank you for reporting this.

Can you please tell us more about your setup? Do you install into a virtual environment? How do you run ty?

---

_Comment by @sitar777 on 2025-09-29 13:35_

Course!

It's VSCode extension

I have a venv. Basically it's a Django app with many subapps, that are installed using -e feature. I can't say for sure whats the problem with some of them but some packages are working okay, some are not. Every subapp is a different repo, but all of them are in the same folder of my workspace.

Let's we have a installed package "something"

So importing `something.subsomething` provoke this behavior. Even from the inside of that package. Also it looks like that the relative imports are working fine. Was excited to try your Language Server for VSCode as Pylance is so slow with large scale repos. But for the time being I can't :(( 

But I beleive I will see the day when it's gonna help me:)))))))) 

---

_Comment by @sitar777 on 2025-09-29 13:37_

Unfortunately I can't provide you with the setup as it is proprietary. But I will gladly answer questions and provide info that I can provide.

---

_Comment by @sitar777 on 2025-09-29 13:46_

Maybe it's not -e flag that is not working

---

_Comment by @sitar777 on 2025-09-29 13:55_

Yep. Repoduced the problem on a pet project with FastAPI so it's not even Django and -e flag:

structure
```
.
├── main.py
├── src
│   ├── api
│   │   ├── __init__.py
│   │   ├── models.py
│   │   └── rest.py
```

so in my `main.py` I have 

```
from src.api import rest_router
```

in `__init__.py`

```
from src.api.rest import router as rest_router

__all__ = [
    "rest_router",
]
```

and I get Cannot resolve imported module `src.api` ty [unresolved-import](https://ty.dev/rules#unresolved-import) in `main.py`
and the same issue in `__init__.py`

The server is working alright and Pylance is able to resolve this with no problem.

---

_Comment by @sitar777 on 2025-09-29 13:58_

@sharkdp Sorry for misinformation

---

_Label `needs-info` removed by @sharkdp on 2025-09-29 14:07_

---

_Comment by @sharkdp on 2025-09-29 14:13_

I can not reproduce this, neither with the command-line version, nor with the VS Code extensions:

```
▶ tree
.
├── main.py
└── src
    └── api
        ├── __init__.py
        └── rest.py

3 directories, 3 files


▶ cat main.py                              
from src.api import rest_router


▶ cat src/api/__init__.py 
from src.api.rest import router as rest_router

__all__ = [
    "rest_router",
]


▶ cat src/api/rest.py    
router = 1


▶ uvx ty --version
ty 0.0.1-alpha.21


▶ uvx ty check    
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 3/3 files
All checks passed!
```




---

_Comment by @sitar777 on 2025-09-29 14:22_

Try to move all of the code in a directory 

```
tree                                                                                                                              127 ↵
.
└── another_workdir
    ├── main.py
    └── src
        └── api
            ├── __init__.py
            └── rest.py
```

---

_Comment by @sharkdp on 2025-09-29 14:24_

Ohh, I see. I think you're looking for mono-repo support: https://github.com/astral-sh/ty/issues/819?

---

_Comment by @sitar777 on 2025-09-29 14:25_

<img width="1462" height="552" alt="Image" src="https://github.com/user-attachments/assets/d3d578bd-61b4-42de-b922-2d4c29278261" />

---

_Comment by @sitar777 on 2025-09-29 14:26_

> Ohh, I see. I think you're looking for mono-repo support: [#819](https://github.com/astral-sh/ty/issues/819)?

Looks like it


---

_Comment by @sitar777 on 2025-09-29 14:30_

Interestingly VSCode is giving me errors while CLI does not

---

_Comment by @sharkdp on 2025-09-29 14:31_

If you open the subfolder in VS Code, it should also work.

---

_Comment by @sitar777 on 2025-09-29 14:36_

Yes it does but still in my django project I can't do this as I need all subfolders.
Maybe adding folders to workspace instead of using them as real dirs would work but it's gonna take so much time as theres a lot of subfolders... Well anyways thank you. For the time being it won't work for me.

---

_Comment by @sitar777 on 2025-09-29 14:39_

Will wait for mono-repo support:)

Actually the speed of Rust is why I use ruff in mono-repo. It's so much faster than Pylance. In big mono-repos I can see it, Pylance takes some seconds to lint and ruff is doing it immediately. ty language server is also gonna be very useful in this case. 

---

_Comment by @faroukcharkas on 2025-09-29 17:43_

Having a similar issue except for module discovery. Opening the subfolder works great but as soon as I have it in a monorepo it becomes an issue.

---

_Comment by @sharkdp on 2025-09-29 18:08_

Ok, let's move the discussion to #819 

---

_Closed by @sharkdp on 2025-09-29 18:08_

---
