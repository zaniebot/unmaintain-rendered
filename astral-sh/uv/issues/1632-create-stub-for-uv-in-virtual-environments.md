---
number: 1632
title: "Create stub for `uv` in virtual environments"
type: issue
state: open
author: zanieb
labels:
  - enhancement
  - needs-design
  - virtualenv
assignees: []
created_at: 2024-02-18T08:06:33Z
updated_at: 2025-02-25T08:06:38Z
url: https://github.com/astral-sh/uv/issues/1632
synced_at: 2026-01-10T01:23:08Z
---

# Create stub for `uv` in virtual environments

---

_Issue opened by @zanieb on 2024-02-18 08:06_

It seems like we should probably stub `uv` into the environment by default so that you can use `python -m uv`. I'm not exactly sure what that would look like.

_Originally posted by @zanieb in https://github.com/astral-sh/uv/issues/1598#issuecomment-1950259889_
            

---

_Referenced in [astral-sh/uv#1625](../../astral-sh/uv/issues/1625.md) on 2024-02-18 08:06_

---

_Referenced in [astral-sh/uv#1598](../../astral-sh/uv/issues/1598.md) on 2024-02-18 08:10_

---

_Referenced in [astral-sh/uv#1422](../../astral-sh/uv/issues/1422.md) on 2024-02-18 08:16_

---

_Label `enhancement` added by @zanieb on 2024-02-18 08:24_

---

_Referenced in [astral-sh/uv#1331](../../astral-sh/uv/issues/1331.md) on 2024-02-18 17:25_

---

_Comment by @SnoopJ on 2024-02-18 18:01_

+1 to the idea of a stub, it seems like a fairly natural solution to #1625.

It seems to me that the mechanism would be to include a symlink to or copy of `uv` in the venv (gated by a flag perhaps?) and inspect `argv[0]` when doing venv detection. I believe the analogous CPython behavior that allows `/path/to/venv/bin/python3` to Just Work™ is [scanning for a nearby `pyvenv.cfg`](https://github.com/python/cpython/blob/0c80da4c14d904a367968955544dd6ae58c8101c/Lib/site.py#L529-L539) and it seems like `uv` could probably use the same process as an early step in the venv detection logic.

---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:37_

---

_Comment by @zanieb on 2024-02-18 21:02_

We added a similar scan for the `pyvenv.cfg` in #1504

---

_Comment by @henryiii on 2024-02-19 14:58_

(`pyvenv.cfg`, you mean?)

---

_Comment by @zanieb on 2024-02-19 15:44_

Thanks :) fixed

---

_Referenced in [astral-sh/uv#1831](../../astral-sh/uv/issues/1831.md) on 2024-02-21 21:16_

---

_Referenced in [astral-sh/uv#1869](../../astral-sh/uv/issues/1869.md) on 2024-02-22 18:48_

---

_Comment by @wpk-nist-gov on 2024-02-23 00:25_

Playing around, I think there's workable solution (with a caveat) right now.  I did the following:

```
# 1. create environment using global uv
$ uv venv .venv

# 2. install uv in the venv
$ uv pip install uv

# 3. hack.  Remove uv executable, but leaves python uv interface
$ rm .venv/bin/uv

# 4. This still works. (find_uv_bin() finds my globally installed uv)
$ cd somewhere/else
$ full/path/to/.venv/bin/python -m uv pip freeze
uv==0.1.8
```

Basically, the code in [python/uv](https://github.com/astral-sh/uv/tree/main/python/uv) is your stub.  Maybe just spin off a package `python-uv` which could be (optionally?) included when creating a venv.

However, regardless of step 3 (the hack), using the full path to the python doesn't work if a virtualenv is also activated.  For example (continuing from above):

```
$ uv venv .venv2
$ source .venv2/bin/activate

# Now try to install to .venv using path...
$ .venv/bin/python -m uv pip install typing-extensions

# This installs into .venv2!
$ uv pip freeze
typing-extensions==4.9.0

# not in .venv
$ source .venv/bin/activate
$ uv pip freeze
uv==0.1.8
```

I'm pretty sure this is not the intended behavior.   I'm guessing a solution to #1396 will allow this to be fixed.  For example, if an option `--python-executable`  is available, then instead of searching for, and setting `VIRTUAL_ENV` variable [here](https://github.com/astral-sh/uv/blob/main/python/uv/__main__.py), you could simply pass  `f"--python-executable={sys.executable}"`. 

I figured it made sense to add this to this issue.  Please let me know if I should start a new issue instead.
 

Thank you to all the developers for this awesome package!



---

_Referenced in [astral-sh/uv#1396](../../astral-sh/uv/issues/1396.md) on 2024-02-23 00:41_

---

_Label `needs-design` added by @zanieb on 2024-07-01 21:51_

---

_Referenced in [tribut/homeassistant-docker-venv#38](../../tribut/homeassistant-docker-venv/issues/38.md) on 2024-11-07 15:03_

---

_Comment by @eabase on 2025-02-25 08:06_

I think a better solution is to tear out the .venv (file installs) from sub-directory, and instead link to a system directory for all venv installs, as they do in the [venvlink](https://github.com/fohrloop/venvlink/) project. 

See my proposal in #11773 

---
