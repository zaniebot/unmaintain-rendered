```yaml
number: 5691
title: "Move `--python` and `--python-version` into the \"Python options\" help"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/python-options
created_at: 2024-08-01T16:19:56Z
updated_at: 2024-08-01T16:55:22Z
url: https://github.com/astral-sh/uv/pull/5691
synced_at: 2026-01-10T13:37:23Z
```

# Move `--python` and `--python-version` into the "Python options" help

---

_Pull request opened by @zanieb on 2024-08-01 16:19_

Part of #4454 

e.g. for `uv help pip compile`

```
Python options:
      --python <PYTHON>
          The Python interpreter against which to compile the requirements.
          
          By default, uv uses the virtual environment in the current working directory or any parent
          directory, falling back to searching for a Python executable in `PATH`. The `--python`
          option allows you to specify a different interpreter.
          
          Supported formats:
          - `3.10` looks for an installed Python 3.10 using `py --list-paths` on Windows, or
            `python3.10` on Linux and macOS.
          - `python3.10` or `python.exe` looks for a binary with the given name in `PATH`.
          - `/home/ferris/.local/bin/python3.10` uses the exact Python at the given path.

  -p, --python-version <PYTHON_VERSION>
          The minimum Python version that should be supported by the resolved requirements (e.g., `3.8` or `3.8.17`).
          
          If a patch version is omitted, the minimum patch version is assumed. For example, `3.8` is mapped to `3.8.0`.

      --python-preference <PYTHON_PREFERENCE>
          Whether to prefer using Python installations that are already present on the system, or those that are downloaded and installed by uv

          Possible values:
          - only-managed: Only use managed Python installations; never use system Python installations
          - managed:      Prefer managed Python installations over system Python installations
          - system:       Prefer system Python installations over managed Python installations
          - only-system:  Only use system Python installations; never use managed Python installations

      --python-fetch <PYTHON_FETCH>
          Whether to automatically download Python when required

          Possible values:
          - automatic: Automatically fetch managed Python installations when needed
          - manual:    Do not automatically fetch managed Python installations; require explicit installation
 ```

---

_Label `cli` added by @zanieb on 2024-08-01 16:19_

---

_Comment by @zanieb on 2024-08-01 16:39_

cc @eth3lbert 

---

_@eth3lbert approved on 2024-08-01 16:52_

LGTM 👍 

Not a blocker, should we add some snapshots for these commands?

---

_Comment by @zanieb on 2024-08-01 16:55_

I don't feel strongly. The snapshots are going to be kind of annoying to update and it's not really critical? Not sure.

---

_Merged by @zanieb on 2024-08-01 16:55_

---

_Closed by @zanieb on 2024-08-01 16:55_

---

_Branch deleted on 2024-08-01 16:55_

---

_Comment by @zanieb on 2024-08-01 16:55_

Thanks for reviewing :)

---
