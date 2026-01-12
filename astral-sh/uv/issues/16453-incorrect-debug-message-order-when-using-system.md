```yaml
number: 16453
title: "Incorrect debug message order when using `--system` flag to uv pip interface commands"
type: issue
state: closed
author: DhavalGojiya
labels:
  - tracing
assignees: []
created_at: 2025-10-26T13:42:12Z
updated_at: 2025-10-28T12:18:14Z
url: https://github.com/astral-sh/uv/issues/16453
synced_at: 2026-01-12T16:02:32Z
```

# Incorrect debug message order when using `--system` flag to uv pip interface commands

---

_@DhavalGojiya_

### Summary

**Command:**

```bash
uv pip install pysolr -v --system
```

**Observed log output:**

```
DEBUG uv 0.9.5
DEBUG Acquired shared lock for `/home/dhaval/.cache/uv`
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/usr/bin/python3` (first executable in the search path)
Using Python 3.12.3 environment at: /usr
DEBUG Released lock at `/home/dhaval/.cache/uv/.lock`
```

---

### Description

The current debug message suggests that `uv` first checks **managed installations** and then the **system search path**.

However, when using the `--system` flag, the behavior should logically prioritize the **system search path first**, since uv managed python installations (under `~/.local/share/uv/python/...`) are not supposed to be modified due to the `EXTERNALLY-MANAGED` marker.

Therefore, the debug message is misleading in this context.

---

### Current Behavior

```
DEBUG Searching for default Python interpreter in managed installations or search path
```

### Expected Behavior (Option 1 - Swap)

```
DEBUG Searching for default Python interpreter in search path or managed installations
```

### Expected Behavior (Option 2 - Limit)
**Removing managed installations lookup enitrely**

```
DEBUG Searching for default Python interpreter in search path
```

---

### Additional Notes

* This is purely a **logging/UX issue** - functionality seems correct.
* Clarifying this message will help users better understand where `uv` looks for interpreters when `--system` is used.


### Platform

Linux 5.15.153.1-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

uv 0.9.5

### Python version

Python 3.12

---

_Label `bug` added by @DhavalGojiya on 2025-10-26 13:42_

---

_Comment by @DhavalGojiya on 2025-10-26 13:54_

I also have one more question:  

Does uv internally treat all its managed installations as “externally managed” for safety,  
or is this particular Python version installed by uv itself actually configured to be marked as “externally managed”?

---

_Comment by @zanieb on 2025-10-26 15:47_

> Does uv internally treat all its managed installations as “externally managed” for safety,
> or is this particular Python version installed by uv itself actually configured to be marked as “externally managed”?

I don't understand the two cases you're distinguishing between here, but all Python versions managed by uv are marked as externally-managed.


---

_Label `bug` removed by @zanieb on 2025-10-26 15:47_

---

_Label `tracing` added by @zanieb on 2025-10-26 15:47_

---

_Comment by @zanieb on 2025-10-26 15:49_

Regarding the OP, I think the change would be at https://github.com/astral-sh/uv/blob/1fbc1c7ff4c587292e60bf4706ca31a9dadffc3a/crates/uv-python/src/discovery.rs#L3170-L3180

---

_Comment by @DhavalGojiya on 2025-10-26 17:14_

> > Does uv internally treat all its managed installations as “externally managed” for safety,
> > or is this particular Python version installed by uv itself actually configured to be marked as “externally managed”?
> 
> I don't understand the two cases you're distinguishing between here, but all Python versions managed by uv are marked as externally-managed.  

I mean, is the Python version installed by uv already marked as `externally-managed`, or does it only behave that way temporarily when uv runs its Rust code (during the any uv CLI command execution)

But your reply makes it clear that it is not temporary - the Python version itself comes marked as `externally-managed` when we install it using `uv python install $VERSION`.

---

_Comment by @DhavalGojiya on 2025-10-27 10:51_

1)
```bash
dhaval@lg-ubuntu:~/.local/share/uv/python$ ls
cpython-3.9.7-linux-x86_64-gnu

dhaval@lg-ubuntu:~/.local/share/uv/python$ uv python dir  # I'm in the same directory that uv prints
/home/dhaval/.local/share/uv/python

dhaval@lg-ubuntu:~/.local/share/uv/python$ cpython-3.9.7-linux-x86_64-gnu/bin/python -m pip install pyjwt
Collecting pyjwt
  Using cached PyJWT-2.10.1-py3-none-any.whl (22 kB)
Installing collected packages: pyjwt
Successfully installed pyjwt-2.10.1
```

Isn’t every Python version installed by `uv` using
`uv python install <VERSION>` considered **externally managed**?

Or does that rule not apply to older Python versions installed by `uv`?
Because here you can see that I’m able to modify the actual uv managed Python version’s `site-packages/` folder.

2) Try with some new python version

```
dhaval@lg-ubuntu:~/.local/share/uv/python$ cpython-3.13.7-linux-x86_64-gnu/bin/python -m pip install pyjwt

error: externally-managed-environment

× This environment is externally managed
╰─> This Python installation is managed by uv and should not be modified.
```
externally managed marker works fine.

---

_Closed by @zanieb on 2025-10-28 12:18_

---
