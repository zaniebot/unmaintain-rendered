```yaml
number: 17174
title: uv run pytest breaks history in pdb on windows
type: issue
state: closed
author: RmStorm
labels:
  - bug
  - windows
assignees: []
created_at: 2025-12-18T12:23:04Z
updated_at: 2026-01-10T18:11:14Z
url: https://github.com/astral-sh/uv/issues/17174
synced_at: 2026-01-12T02:26:26Z
```

# uv run pytest breaks history in pdb on windows

---

_Issue opened by @RmStorm on 2025-12-18 12:23_

### Summary

When running `pytest` using `uv run` it breaks history in `pdb`.. so something like:
```
➜ cat src/shell_pytest/test_thingy.py -p
def test_my_test():
    breakpoint()
```
drops you into a debugger when running `uv run pytest`.. Then you can do something like:
```
➜ uv run pytest
========================================= test session starts ==========================================
platform linux -- Python 3.12.9, pytest-9.0.2, pluggy-1.6.0
rootdir: /home/rmstorm/Documents/python/shell_pytest
configfile: pyproject.toml
collected 1 item                                                                                       

src/shell_pytest/test_thingy.py 
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> PDB set_trace (IO-capturing turned off) >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
--Return--
> /home/rmstorm/Documents/python/shell_pytest/src/shell_pytest/test_thingy.py(2)test_my_test()->None
-> breakpoint()
(Pdb) p 5
5
(Pdb) p 5
5
(Pdb) exit()


!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! _pytest.outcomes.Exit: Quitting debugger !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
======================================== no tests ran in 7.12s =========================================
```
And on Linux this works.. you get arrow up functionality and a nice debugging experience.. On Windows arrow up will be broken. I've found two workarounds for the issue that might help in debugging.

The first is to directly invoke `pytest.exe` from the uv managed venv like so `.\.venv\Scripts\pytest.exe` everything works as expected! The second is to invoke the `pytest` module through python like so: `uv run python -m pytest` This also gives working history in pdb under pytest..

So I think the issue is a strange interaction between `pytest.exe` and uv.

### Platform

Windows 11 X86_64

### Version

uv 0.9.5

### Python version

_No response_

---

_Label `bug` added by @RmStorm on 2025-12-18 12:23_

---

_Label `windows` added by @konstin on 2025-12-18 12:32_

---

_Comment by @zelosleone on 2025-12-22 17:00_

This is most definitely not an uv bug. uv currently makes invalid handlers on windows due to `close_handles` closing DUPed handles that share the same underlying kernel object as STDIN/STDOUT as it was not checking the handler correctly, changing these things and making sure we get the standard handlers from OS will fix that and we won't have invalid handles on trampoline after `close_handles` function performs. However, this won't fix the issue of pytest. Python by itself works correctly by the way, it's a pytest issue.

Some logs after i added things to diagnostics (before the mentioned fixes first)

```
--- TRAMPOLINE DIAGNOSTICS: STARTUP ---
  STDIN: Handle=520
    -> Console Mode: 0x000001F7
  STDOUT: Handle=812
    -> Console Mode: 0x00000007
  STDERR: Handle=1008
    -> Console Mode: 0x00000007
-------------------------------------------
--- TRAMPOLINE DIAGNOSTICS: AFTER CLOSE_HANDLES ---
  STDIN: Handle=-1
    -> INVALID
  STDOUT: Handle=-1
    -> INVALID
  STDERR: Handle=1008
    -> Console Mode: 0x00000007
```
after:

```
--- TRAMPOLINE DIAGNOSTICS: STARTUP ---
  STDIN: Handle=532
    -> Console Mode: 0x000001F7
  STDOUT: Handle=920
    -> Console Mode: 0x00000007
  STDERR: Handle=936
    -> Console Mode: 0x00000007
-------------------------------------------
--- TRAMPOLINE DIAGNOSTICS: AFTER CLOSE_HANDLES (DISABLED) ---
  STDIN: Handle=532
    -> Console Mode: 0x000001F7
  STDOUT: Handle=920
    -> Console Mode: 0x00000007
  STDERR: Handle=936
    -> Console Mode: 0x00000007
```

which still won't make pytest see the history for some reason. And yes, directly calling it with a direct path works but that could be a path issue from pytest itself as well.

---

_Comment by @zelosleone on 2025-12-22 20:58_

I basically traced it back to pdb itself. https://github.com/python/cpython/pull/143083 this fixes it.

---

_Comment by @konstin on 2026-01-06 19:09_

Amazing, thanks for fixing this in CPython!

---

_Comment by @konstin on 2026-01-06 19:09_

> uv currently makes invalid handlers on windows due to `close_handles` closing DUPed handles that share the same underlying kernel object as STDIN/STDOUT as it was not checking the handler correctly, changing these things and making sure we get the standard handlers from OS will fix that and we won't have invalid handles on trampoline after `close_handles` function performs. However, this won't fix the issue of pytest.

Can you share that fix?

---

_Comment by @zelosleone on 2026-01-09 12:09_

> > uv currently makes invalid handlers on windows due to `close_handles` closing DUPed handles that share the same underlying kernel object as STDIN/STDOUT as it was not checking the handler correctly, changing these things and making sure we get the standard handlers from OS will fix that and we won't have invalid handles on trampoline after `close_handles` function performs. However, this won't fix the issue of pytest.
> 
> Can you share that fix?

sure, here is it https://github.com/astral-sh/uv/pull/17374

---

_Comment by @konstin on 2026-01-09 12:10_

Thank you!

---

_Comment by @zelosleone on 2026-01-09 12:14_

Also if you guys can connect with pyreadline3 guys, their window build is gonna be completely broken in the future (its already broken in newer python versions) https://github.com/pyreadline3/pyreadline3/pull/44 (just to let you guys know since you guys might know one of them)

---

_Closed by @zanieb on 2026-01-10 18:11_

---
