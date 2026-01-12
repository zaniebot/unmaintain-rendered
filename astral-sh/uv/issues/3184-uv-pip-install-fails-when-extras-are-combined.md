```yaml
number: 3184
title: "uv pip install fails when extras are combined with `--no-deps` "
type: issue
state: closed
author: danielhollas
labels:
  - bug
assignees: []
created_at: 2024-04-22T13:07:25Z
updated_at: 2024-04-22T21:17:02Z
url: https://github.com/astral-sh/uv/issues/3184
synced_at: 2026-01-12T15:58:42Z
```

# uv pip install fails when extras are combined with `--no-deps` 

---

_@danielhollas_

uv seems to choke on a (arguably weird) combination of `--no-deps` together with extras, such as (using `aiida-core` package as an example):

```console
$ git clone https://github.com/aiidateam/aiida-core && cd aiida-core
$ uv venv --seed
$ uv pip install --no-deps -e .[pre-commit]
   Built file:///home/hollas/atmospec/aiida-core                                                                                           Built 1 editable in 112ms
error: Attempted to wait on an unregistered task
```

Note that the error itself is the same as in #2941, but I'd assume the root cause is different?

The same command with pip succeeds, pip simply ignores the extras requirement (I guess that makes sense)
```console
$ source .venv/bin/activate
$ python -m pip install --no-deps -e .[pre-commit]
Obtaining file:///home/hollas/atmospec/aiida-core
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... done
  Preparing editable metadata (pyproject.toml) ... done
Checking if build backend supports build_editable ... done
Building wheels for collected packages: aiida-core
  Building editable for aiida-core (pyproject.toml) ... done
  Created wheel for aiida-core: filename=aiida_core-2.5.1.post0-py3-none-any.whl size=7307 sha256=eff52e947083c32c441e74146f11f57874e1f3994949db8f2c74532d294e38aa
  Stored in directory: /tmp/pip-ephem-wheel-cache-fvl5b5bp/wheels/45/ad/9e/2a9235e41bf1c7f0a56f7fb7c8f986c771f1ed56f8dc5de33b
Successfully built aiida-core
Installing collected packages: aiida-core
  Attempting uninstall: aiida-core
    Found existing installation: aiida-core 2.5.1.post0
    Uninstalling aiida-core-2.5.1.post0:
      Successfully uninstalled aiida-core-2.5.1.post0
Successfully installed aiida-core-2.5.1.post0
```

uv version: 0.1.35
OS: Linux

(obligatory thanks for uv as always: aiida-core installation drops from 1m 30s to 10s! :heart_eyes: )

---

_Comment by @charliermarsh on 2024-04-22 14:24_

Thanks, will take a look -- this happened once before and I thought I fixed it hah.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-22 14:24_

---

_Label `bug` added by @charliermarsh on 2024-04-22 14:24_

---

_Closed by @charliermarsh on 2024-04-22 16:29_

---

_Comment by @danielhollas on 2024-04-22 21:17_

Just tested with 0.1.36, works now as expected. Thanks so much for a super quick fix! :heart_decoration: 

---
