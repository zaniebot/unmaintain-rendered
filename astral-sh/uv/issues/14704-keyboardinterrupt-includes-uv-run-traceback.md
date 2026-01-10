```yaml
number: 14704
title: "KeyboardInterrupt includes `uv run` traceback"
type: issue
state: closed
author: Dobatymo
labels:
  - bug
  - windows
assignees: []
created_at: 2025-07-18T07:40:05Z
updated_at: 2025-07-18T13:07:37Z
url: https://github.com/astral-sh/uv/issues/14704
synced_at: 2026-01-10T03:32:45Z
```

# KeyboardInterrupt includes `uv run` traceback

---

_Issue opened by @Dobatymo on 2025-07-18 07:40_

### Summary

Running `py -3.13 -m uv run python -c "import time;time.sleep(1000000)"` and hitting ctrl-c outputs

```
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import time;time.sleep(1000000)
                ~~~~~~~~~~^^^^^^^^^
KeyboardInterrupt
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "...\Python313\Lib\site-packages\uv\__main__.py", line 47, in <module>
    _run()
    ~~~~^^
  File "...\Python313\Lib\site-packages\uv\__main__.py", line 40, in _run
    completed_process = subprocess.run([uv, *sys.argv[1:]], env=env)
  File "...\Python313\Lib\subprocess.py", line 556, in run
    stdout, stderr = process.communicate(input, timeout=timeout)
                     ~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^
  File "...\Python313\Lib\subprocess.py", line 1214, in communicate
    self.wait()
    ~~~~~~~~~^^
  File "...\Python313\Lib\subprocess.py", line 1280, in wait
    return self._wait(timeout=timeout)
           ~~~~~~~~~~^^^^^^^^^^^^^^^^^
  File "...\Python313\Lib\subprocess.py", line 1606, in _wait
    result = _winapi.WaitForSingleObject(self._handle,
                                         timeout_millis)
KeyboardInterrupt
^C
```

Is this expected when running uv as a Python module?

### Platform

Windows 11 x86_64

### Version

uv 0.8.0 (0b2357294 2025-07-17)

### Python version

Python 3.13.5

---

_Label `bug` added by @Dobatymo on 2025-07-18 07:40_

---

_Comment by @zanieb on 2025-07-18 12:08_

If you use `python -m uv`, yes we'll show the trace for interrupting the uv subprocess. I guess we can hide that?

---

_Comment by @zanieb on 2025-07-18 12:09_

Ah you're on Windows. This isn't a thing on Linux, because we can exec.

---

_Label `windows` added by @konstin on 2025-07-18 12:32_

---

_Closed by @zanieb on 2025-07-18 13:07_

---
