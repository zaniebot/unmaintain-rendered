```yaml
number: 15635
title: uv cannot install packages on its own managed Python with virtual environment activated
type: issue
state: closed
author: rgandhasri87
labels:
  - question
assignees: []
created_at: 2025-09-02T18:42:18Z
updated_at: 2025-09-02T20:10:58Z
url: https://github.com/astral-sh/uv/issues/15635
synced_at: 2026-01-12T16:02:14Z
```

# uv cannot install packages on its own managed Python with virtual environment activated

---

_@rgandhasri87_

### Summary

I have an activated virtual environment and I am trying to install packages from a requirements.txt file in it.
The environment was created by running `uv init` after obtaining the correct `pyproject.toml` file.

Running `sudo uv pip install --system -r requirements.txt --verbose` yields the following output:

```
DEBUG uv 0.8.3 (7e78f54e7 2025-07-24)
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/Users/r0g0fqv/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.5-macos-aarch64-none`
DEBUG Found `cpython-3.13.5-macos-aarch64-none` at `/Users/r0g0fqv/.local/share/uv/python/cpython-3.13.5-macos-aarch64-none/bin/python3.13` (managed installations)
Using Python 3.13.5 environment at: /Users/r0g0fqv/.local/share/uv/python/cpython-3.13.5-macos-aarch64-none
error: The interpreter at /Users/r0g0fqv/.local/share/uv/python/cpython-3.13.5-macos-aarch64-none is externally managed, and indicates the following:

  This Python installation is managed by uv and should not be modified.

Consider creating a virtual environment with `uv venv`.
```

I am not sure why this is occurring when the virtual environment is activated. 
I reviewed the following issues: [10300](https://github.com/astral-sh/uv/issues/10300) [12204](https://github.com/astral-sh/uv/issues/12204) but they did not seem to apply to my issue, unless I am mistaken.

Please let me know if I can provide more information. I am afraid I cannot share much of the setup code due to enterprise policies.

### Platform

macOS Darwin 24.6.0 arm64

### Version

uv 0.8.3 (7e78f54e7 2025-07-24)

### Python version

Python 3.13.5

---

_Label `bug` added by @rgandhasri87 on 2025-09-02 18:42_

---

_Comment by @zanieb on 2025-09-02 19:45_

You're using the `--system` flag which bypasses the virtual environment.

---

_Label `bug` removed by @zanieb on 2025-09-02 19:55_

---

_Label `question` added by @zanieb on 2025-09-02 19:55_

---

_Comment by @rgandhasri87 on 2025-09-02 20:10_

Thanks. Can't believe I missed that. Resolved

---

_Closed by @rgandhasri87 on 2025-09-02 20:10_

---
