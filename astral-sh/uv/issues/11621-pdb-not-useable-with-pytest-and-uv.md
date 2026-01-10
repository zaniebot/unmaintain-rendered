---
number: 11621
title: "`pdb` not useable with pytest and uv"
type: issue
state: closed
author: ccurme
labels:
  - question
  - external
assignees: []
created_at: 2025-02-19T15:19:37Z
updated_at: 2025-02-19T17:06:07Z
url: https://github.com/astral-sh/uv/issues/11621
synced_at: 2026-01-10T01:25:08Z
---

# `pdb` not useable with pytest and uv

---

_Issue opened by @ccurme on 2025-02-19 15:19_

### Summary

When I insert a breakpoint into a test via `pdb.set_trace()` and run with `pytest`, we enter the debugger but I cannot run any commands.

To reproduce:
```
uv init hello-world
cd hello-world
uv add pytest
echo "def test_debugging():\n    import pdb; pdb.set_trace()" > test_debugging.py
uv run pytest .
```
```
-> import pdb; pdb.set_trace()
```
^ Cannot run any commands from there.


### Platform

macOS 14 arm64 (`uname -orsm` gives Darwin 23.6.0 arm64)

### Version

uv 0.5.26 (Homebrew 2025-01-30)

### Python version

Python 3.13.1

---

_Label `bug` added by @ccurme on 2025-02-19 15:19_

---

_Comment by @zanieb on 2025-02-19 16:07_

Hm, I can reproduce this with a HomeBrew Python and invoking `pytest` directly

```
❯ uv venv --python-preference only-system
Using CPython 3.13.1 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ uv sync
Resolved 6 packages in 1ms
Installed 4 packages in 3ms
 + iniconfig==2.0.0
 + packaging==24.2
 + pluggy==1.5.0
 + pytest==8.3.4
❯ .venv/bin/pytest
==================================================== test session starts ====================================================
platform darwin -- Python 3.13.1, pytest-8.3.4, pluggy-1.5.0
rootdir: /Users/zb/workspace/hello-world
configfile: pyproject.toml
collected 1 item                                                                                                            

test_debugging.py 
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> PDB set_trace (IO-capturing turned off) >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
> /Users/zb/workspace/hello-world/test_debugging.py(2)test_debugging()
-> import pdb; pdb.set_trace()
^C--KeyboardInterrupt--
^D
F                                                                                                   [100%]

========================================================= FAILURES ==========================================================
______________________________________________________ test_debugging _______________________________________________________

    def test_debugging():
>       import pdb; pdb.set_trace()

test_debugging.py:2: 
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _
/opt/homebrew/Cellar/python@3.13/3.13.1/Frameworks/Python.framework/Versions/3.13/lib/python3.13/bdb.py:110: in trace_dispatch
    return self.dispatch_opcode(frame, arg)
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

self = <_pytest.debugging.pytestPDB._get_pdb_wrapper_class.<locals>.PytestPdbWrapper object at 0x10223cad0>
frame = <frame at 0x10218e980, file '/Users/zb/workspace/hello-world/test_debugging.py', line 2, code test_debugging>
arg = None

    def dispatch_opcode(self, frame, arg):
        """Invoke user function and return trace function for opcode event.
        If the debugger stops on the current opcode, invoke
        self.user_opcode(). Raise BdbQuit if self.quitting is set.
        Return self.trace_dispatch to continue tracing in this scope.
        """
        if self.stop_here(frame) or self.break_here(frame):
            self.user_opcode(frame)
>           if self.quitting: raise BdbQuit
E           bdb.BdbQuit

/opt/homebrew/Cellar/python@3.13/3.13.1/Frameworks/Python.framework/Versions/3.13/lib/python3.13/bdb.py:210: BdbQuit
================================================== short test summary info ==================================================
FAILED test_debugging.py::test_debugging - bdb.BdbQuit
===================================================== 1 failed in 1.26s =====================================================
```

Is there a context in which this works?

---

_Comment by @zanieb on 2025-02-19 16:09_

You need to disable capturing

```
❯ .venv/bin/pytest --capture=no
==================================================== test session starts ====================================================
platform darwin -- Python 3.13.1, pytest-8.3.4, pluggy-1.5.0
rootdir: /Users/zb/workspace/hello-world
configfile: pyproject.toml
collected 1 item                                                                                                            

test_debugging.py 
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> PDB set_trace >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
> /Users/zb/workspace/hello-world/test_debugging.py(2)test_debugging()
-> breakpoint()
(Pdb) 
```

---

_Label `bug` removed by @zanieb on 2025-02-19 16:09_

---

_Label `question` added by @zanieb on 2025-02-19 16:09_

---

_Label `external` added by @zanieb on 2025-02-19 16:09_

---

_Comment by @ccurme on 2025-02-19 16:12_

That works for me, thanks for the quick response!

---

_Closed by @zanieb on 2025-02-19 17:06_

---
