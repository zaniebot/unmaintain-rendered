---
number: 6084
title: Unable to install pyre2 with uv
type: issue
state: closed
author: sgrimee
labels:
  - needs-mre
assignees: []
created_at: 2024-08-14T14:17:58Z
updated_at: 2024-11-29T17:21:03Z
url: https://github.com/astral-sh/uv/issues/6084
synced_at: 2026-01-10T01:23:56Z
---

# Unable to install pyre2 with uv

---

_Issue opened by @sgrimee on 2024-08-14 14:17_

Unable to install pyre2 with uv pip, it works with pip directly.

On mac, in a nix environment with python 3.12.4 and uv 0.1.45.
Using a venv created by uv.

Trying to install pyre2:
```
uv pip install pyre2==0.3.6          
Resolved 1 package in 2ms
error: Failed to download distributions
  Caused by: Failed to fetch wheel: pyre2==0.3.6
  Caused by: Failed to build: `pyre2==0.3.6`
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
running build
running build_ext
-- Configuring incomplete, errors occurred!
--- stderr:
CMake Error at CMakeLists.txt:3 (project):
  Running

   '/Users/sgrimee/Library/Caches/uv/.tmpRpC52h/.venv/bin/ninja' '--version'

  failed with:

   no such file or directory
```

It also fails if I first install:
```
uv pip install ninja setuptools wheel
Resolved 3 packages in 2ms
Installed 3 packages in 11ms
 + ninja==1.11.1.1
 + setuptools==72.2.0
 + wheel==0.44.0
```



---

_Comment by @charliermarsh on 2024-08-14 14:18_

It looks like you might need `--no-build-isolation`?

---

_Comment by @charliermarsh on 2024-08-14 14:19_

See: https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#pep-517-build-isolation

---

_Comment by @sgrimee on 2024-08-14 14:27_

Thanks for the response! So I try from a fresh venv, I install setuptools and ninja, then:
```
uv pip install --no-build-isolation  pyre2==0.3.6 
Resolved 1 package in 0.94ms
error: Failed to download distributions
  Caused by: Failed to fetch wheel: pyre2==0.3.6
  Caused by: Failed to build: `pyre2==0.3.6`
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
running build
running build_ext
-- Configuring incomplete, errors occurred!
--- stderr:
CMake Error at CMakeLists.txt:3 (project):
  Running

   '/Users/sgrimee/Library/Caches/uv/.tmpRpC52h/.venv/bin/ninja' '--version'

  failed with:

   no such file or directory
   ```

---

_Comment by @charliermarsh on 2024-08-14 14:41_

Hmm, this works as expected for me:

```
❯ uv venv

❯ uv pip install setuptools ninja cython

❯ uv pip install --no-build-isolation  pyre2==0.3.6
Resolved 1 package in 1ms
   Built pyre2==0.3.6
Prepared 1 package in 3.23s
Installed 1 package in 0.64ms
 + pyre2==0.3.6
```


---

_Label `needs-mre` added by @charliermarsh on 2024-08-14 14:41_

---

_Comment by @sgrimee on 2024-08-15 13:33_

thanks for checking... are you on python 3.12?
I still see the error, and had a colleague test it to, but I see it only on 3.12.
With 3.9-11, I get [another error](https://github.com/facebook/pyre2/issues/24), I think not related, which I am trying to solve also by upgrading my c++ to v17.
Would you mind sharing your python version, as well as c++ version you are using?





---

_Comment by @charliermarsh on 2024-08-15 17:29_

@sgrimee -- I just tested and it succeeds for me on both Python 3.11 and Python 3.12, unfortunately.

---

_Comment by @charliermarsh on 2024-09-14 22:20_

Gonna close due to difficulty reproducing.

---

_Closed by @charliermarsh on 2024-09-14 22:20_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-14 22:20_

---

_Comment by @vlcek on 2024-11-29 17:21_

Sorry for writing into an already closed issue, however, I hit the same problem with `pyre2` on Python 3.11 with uv 0.5.5.
I tried the `--no-build-isolation` with no success, however, once I nuked the uv's cache (`rm -rf ~/.cache/uv`) and run `uv sync` again, suddenly the `pyre2` build was successful.

Maybe this info will help anyone reading this issue.

---
