```yaml
number: 2120
title: "Remove `base-prefix` and friends from `pyvenv.cfg`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - virtualenv
assignees: []
merged: true
base: main
head: charlie/keys
created_at: 2024-03-01T20:34:47Z
updated_at: 2024-03-03T17:32:06Z
url: https://github.com/astral-sh/uv/pull/2120
synced_at: 2026-01-10T14:54:43Z
```

# Remove `base-prefix` and friends from `pyvenv.cfg`

---

_Pull request opened by @charliermarsh on 2024-03-01 20:34_

## Summary

It looks like these have been included since the very first gourgeist commit, but `virtualenv` and `venv` don't include them, and they only add complexity AFAICT.

---

_Review requested from @konstin by @charliermarsh on 2024-03-01 20:34_

---

_Comment by @charliermarsh on 2024-03-01 20:55_

I see these _are_ included if I use Homebrew Python.

---

_Comment by @charliermarsh on 2024-03-01 21:05_

@gaborbernat - Sorry to bother, do you know what these are for? I don't see code in `site.py` that leverages them.

---

_Comment by @gaborbernat on 2024-03-01 21:11_

virtualenv includes these for book-keeping. We don't use it though at runtime for now.

```
❯ virtualenv venv --clear
created virtual environment CPython3.12.2.final.0-64 in 179ms
  creator CPython3Posix(dest=~/venv, clear=True, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, via=copy, app_data_dir=/Users/bgabor8/Library/Application Support/virtualenv)
    added seed packages: pip==24.0
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator

❯ cat venv/pyvenv.cfg
home = /Users/bgabor8/.pyenv/versions/3.12.2/bin
implementation = CPython
version_info = 3.12.2.final.0
virtualenv = 20.25.1
include-system-site-packages = false
base-prefix = /Users/bgabor8/.pyenv/versions/3.12.2
base-exec-prefix = /Users/bgabor8/.pyenv/versions/3.12.2
base-executable = /Users/bgabor8/.pyenv/versions/3.12.2/bin/python3.12

```
```
❯ python3.12 -m venv venv --clear

❯ cat venv/pyvenv.cfg
home = /Users/bgabor8/.pyenv/versions/3.12.1/bin
include-system-site-packages = false
version = 3.12.1
executable = /Users/bgabor8/.pyenv/versions/3.12.1/bin/python3.12
command = /Users/bgabor8/.pyenv/versions/3.12.1/bin/python3.12 -m venv --clear ~/venv
```

---

_Comment by @charliermarsh on 2024-03-01 21:13_

Cool, thank you, that makes sense.

---

_Merged by @charliermarsh on 2024-03-01 21:17_

---

_Closed by @charliermarsh on 2024-03-01 21:17_

---

_Branch deleted on 2024-03-01 21:17_

---

_Label `virtualenv` added by @charliermarsh on 2024-03-01 21:20_

---

_Comment by @konstin on 2024-03-03 17:32_

I just copied that from virtualenv, we don't use them anywhere either

---
