```yaml
number: 10195
title: Fix find_uv_bin to find uv with system site packages
type: pull_request
state: closed
author: ssbarnea
labels: []
assignees: []
base: main
head: fix/find_uv_bin
created_at: 2024-12-27T15:13:01Z
updated_at: 2025-04-16T15:22:39Z
url: https://github.com/astral-sh/uv/pull/10195
synced_at: 2026-01-10T11:10:34Z
```

# Fix find_uv_bin to find uv with system site packages

---

_Pull request opened by @ssbarnea on 2024-12-27 15:13_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This allows `uv` to find its binary when it was installed as a system package. Current implementation fails if someone creates a venv with --system-packages because the relative uv path would be different.

Fixes: #10194

## Test Plan

<!-- How was it tested? -->


---

_Marked ready for review by @ssbarnea on 2024-12-27 15:20_

---

_Comment by @ssbarnea on 2024-12-28 12:09_

@charliermarsh please take a look at this and its bug. Let me know if further changes are needed as this is a blocker for addressing https://github.com/tox-dev/tox-uv/issues/109 and in fact can happen easily even without tox. As soon you create a venv and install uv in it, once you try to create a new one, you may discover that you have an unusable uv package.

---

_@zanieb reviewed on 2024-12-28 17:23_

---

_Review comment by @zanieb on `python/uv/_find_uv.py`:33 on 2024-12-28 17:23_

Why `../../..`? What path is this targeting?

---

_Comment by @zanieb on 2024-12-28 17:25_

> As soon you create a venv and install uv in it, once you try to create a new one, you may discover that you have an unusable uv package.

To be clear, this should _not_ work by design

```
❯ uv venv one
Using CPython 3.13.1
Creating virtual environment at: one
Activate with: source one/bin/activate
❯ uv venv two
Using CPython 3.13.1
Creating virtual environment at: two
Activate with: source two/bin/activate
❯ uv pip install -p one uv
Using Python 3.13.1 environment at: one
Resolved 1 package in 181ms
Prepared 1 package in 1.12s
Installed 1 package in 7ms
 + uv==0.5.13
❯ ./one/bin/python -c "import uv; uv.find_uv_bin()"
❯ ./two/bin/python -c "import uv; uv.find_uv_bin()"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import uv; uv.find_uv_bin()
    ^^^^^^^^^
ModuleNotFoundError: No module named 'uv'
```

Do you mean something else?

---

_Assigned to @zanieb by @zanieb on 2025-01-06 20:35_

---

_Comment by @ssbarnea on 2025-01-16 16:38_

> To be clear, this should _not_ work by design

Can you please elaborate around that? The way I read it is that you cannot make use of uv with venvs using system site packages.

```shell
uv venv --seed x1
source x1/bin/activate
pip install uv
python -m uv --version # works
uv --version # works

# case two:
uv venv --seed  --system-site-packages x2

source x2/bin/activate

pip install uv
Requirement already satisfied: uv in /Users/ssbarnea/.config/mise/installs/python/3.13.1/lib/python3.13/site-packages (0.5.18)
# ^ first problem, yes uv module is present due to system-site-packages but is not usable
# in any way, see below:

> python -m uv --version
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/Users/ssbarnea/.config/mise/installs/python/3.13.1/lib/python3.13/site-packages/uv/__main__.py", line 47, in <module>
    _run()
    ~~~~^^
  File "/Users/ssbarnea/.config/mise/installs/python/3.13.1/lib/python3.13/site-packages/uv/__main__.py", line 27, in _run
    uv = os.fsdecode(find_uv_bin())
                     ~~~~~~~~~~~^^
  File "/Users/ssbarnea/.config/mise/installs/python/3.13.1/lib/python3.13/site-packages/uv/_find_uv.py", line 36, in find_uv_bin
    raise FileNotFoundError(path)
FileNotFoundError: /Users/ssbarnea/.local/bin/uv

# calling `uv` might give unreliable results now because clearly is not inside `x1/bin`
uv --version 

# example from my system:
❯ which -a uv
/Users/ssbarnea/.venv/bin/uv
/Users/ssbarnea/.config/mise/installs/python/3.13.1/bin/uv
/Users/ssbarnea/.config/mise/installs/python/3.12.8/bin/uv
/Users/ssbarnea/.config/mise/installs/python/3.11.11/bin/uv
/Users/ssbarnea/.config/mise/installs/python/3.10.16/bin/uv
/Users/ssbarnea/.config/mise/installs/python/3.9.21/bin/uv
/opt/homebrew/bin/uv

# expected behavior would be to use uv from within currently active venv (x1). The .venv one that is found instead is from user default

```


---

_Comment by @zanieb on 2025-01-16 17:06_

> Can you please elaborate around that? The way I read it is that you cannot make use of uv with venvs using system site packages.

I think using system site packages seems okay if the virtual environment has opted into it. The part I'm saying should not work "by design" is that one virtual environment should not be able to access the uv of another virtual environment.

---

_Comment by @zanieb on 2025-04-16 15:22_

I don't see a clear path forward here, I think this needs more design discussion and it looks like you unblocked tox in https://github.com/tox-dev/tox-uv/pull/147?

---

_Closed by @zanieb on 2025-04-16 15:22_

---
