```yaml
number: 4940
title: F841 fix since 0.0.264 can break code
type: issue
state: closed
author: scop
labels:
  - bug
assignees: []
created_at: 2023-06-07T21:07:10Z
updated_at: 2023-06-07T22:25:38Z
url: https://github.com/astral-sh/ruff/issues/4940
synced_at: 2026-01-10T11:09:47Z
```

# F841 fix since 0.0.264 can break code

---

_Issue opened by @scop on 2023-06-07 21:07_

```shellsession
$ curl -O https://raw.githubusercontent.com/home-assistant/core/03710e58b5cc3cd1e2472d2e497bf5780695eca5/tests/components/homekit_controller/test_init.py
$ ruff test_init.py
test_init.py:150:5: F841 [*] Local variable `is_connected` is assigned to but never used
test_init.py:218:5: F841 [*] Local variable `is_available` is assigned to but never used
Found 2 errors.
[*] 2 potentially fixable with the --fix option.
$ ruff --fix test_init.py
Found 6 errors (6 fixed, 0 remaining).
```

Note found 2 vs 6 errors. After the `--fix`:
```shellsession
$ python3 -m test_init
Traceback (most recent call last):
  File "/usr/lib/python3.8/runpy.py", line 185, in _run_module_as_main
    mod_name, mod_spec, code = _get_module_details(mod_name, _Error)
  File "/usr/lib/python3.8/runpy.py", line 155, in _get_module_details
    code = loader.get_code(mod_name)
  File "<frozen importlib._bootstrap_external>", line 981, in get_code
  File "<frozen importlib._bootstrap_external>", line 911, in source_to_code
  File "<frozen importlib._bootstrap>", line 219, in _call_with_frames_removed
  File ".../test_init.py", line 116
    nonlocal is_connected
    ^
SyntaxError: no binding for nonlocal 'is_connected' found
```

Haven't actually checked, but I suppose the fix for these issues is removing too many of the same named variables, not only the two it flagged errors for in non-fix mode.

---

_Comment by @charliermarsh on 2023-06-07 21:10_

None of those should be considered "assigned but unused", right?

---

_Label `question` added by @charliermarsh on 2023-06-07 21:10_

---

_Comment by @scop on 2023-06-07 21:14_

I guess so. But the code is a bit hairy, can't tell for sure on a quick skim, and unfortunately have no time right now to dig deeper.

---

_Label `question` removed by @charliermarsh on 2023-06-07 21:21_

---

_Label `bug` added by @charliermarsh on 2023-06-07 21:21_

---

_Comment by @charliermarsh on 2023-06-07 21:21_

No worries. I feel like the `nonlocal` reads aren't being properly attributed to the scope above, or something.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-07 22:18_

---

_Closed by @charliermarsh on 2023-06-07 22:25_

---
