```yaml
number: 2552
title: "Run interpreter discovery under `-I` mode"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/isolated
created_at: 2024-03-19T19:53:22Z
updated_at: 2024-03-20T00:19:47Z
url: https://github.com/astral-sh/uv/pull/2552
synced_at: 2026-01-12T16:05:06Z
```

# Run interpreter discovery under `-I` mode

---

_@charliermarsh_

## Summary

If you have a file `typing.py` in the current working directory, `python -m` doesn't work in some Python versions:

```sh
❯ python -m foo
Could not import runpy module
Traceback (most recent call last):
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.9.18/lib/python3.9/runpy.py", line 15, in <module>
    import importlib.util
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.9.18/lib/python3.9/importlib/util.py", line 2, in <module>
    from . import abc
  File "/Users/crmarsh/.local/share/rtx/installs/python/3.9.18/lib/python3.9/importlib/abc.py", line 17, in <module>
    from typing import Protocol, runtime_checkable
ImportError: cannot import name 'Protocol' from 'typing' (/Users/crmarsh/workspace/uv/typing.py)
```

This did _not_ cause problems for us on Python 3.11 or later, because we set `PYTHONSAFEPATH`, which avoids adding the current working directory to `sys.path`. However, on earlier versions, we _were_ failing with the above. (It's important that we run interpreter discovery in the current working directory, since doing otherwise breaks pyenv shims.)

The fix implemented here uses `-I` to run Python in isolated mode, which is even stricter. The downside of isolated mode is that we currently rely on setting `PYTHONPATH` to find the "fake module" that we create on disk, and `-I` means `PYTHONPATH` is totally ignored. So, instead, we run a script directly, and that _script_ injects the path we care about into `PYTHONSAFEPATH`.

Closes https://github.com/astral-sh/uv/issues/2547.


---

_Marked ready for review by @charliermarsh on 2024-03-19 19:55_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-19 19:55_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-03-19 19:55_

---

_Label `bug` added by @charliermarsh on 2024-03-19 19:56_

---

_Comment by @charliermarsh on 2024-03-19 21:30_

I don't understand why this broke the Windows shims.

---

_Comment by @charliermarsh on 2024-03-19 21:58_

I guess the `bat` doesn't like a multi-line script argument... Very weird.

---

_Renamed from "Run interpreter discovery under -I mode" to "Run interpreter discovery under `-I` mode" by @charliermarsh on 2024-03-19 22:00_

---

_Comment by @charliermarsh on 2024-03-19 22:01_

I also manually verified that the Pyenv shims continue to work:

```
DEBUG Starting interpreter discovery for default Python
DEBUG Probing interpreter info for: /Users/crmarsh/.pyenv/shims/python3
DEBUG Found Python 3.10.2 for: /Users/crmarsh/.pyenv/shims/python3
Using Python 3.10.2 interpreter at: /Users/crmarsh/.pyenv/versions/3.10.2/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```

---

_@zanieb approved on 2024-03-20 00:15_

Interesting. Why `-c` (and previously `-m`) instead of just `python <path>`?

---

_Comment by @charliermarsh on 2024-03-20 00:18_

We need to be able to resolve relative imports (so we can't use `python <path>`), but we can't use `-m` because the module we want to run isn't in `sys.path` until we modify it. So complicated.

---

_Merged by @charliermarsh on 2024-03-20 00:19_

---

_Closed by @charliermarsh on 2024-03-20 00:19_

---

_Branch deleted on 2024-03-20 00:19_

---
