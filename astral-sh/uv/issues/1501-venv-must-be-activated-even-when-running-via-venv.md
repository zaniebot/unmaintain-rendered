```yaml
number: 1501
title: "venv must be activated even when running via `venv/bin/python -m uv`"
type: issue
state: closed
author: altendky
labels:
  - bug
assignees: []
created_at: 2024-02-16T15:22:31Z
updated_at: 2024-02-16T18:55:25Z
url: https://github.com/astral-sh/uv/issues/1501
synced_at: 2026-01-12T15:58:29Z
```

# venv must be activated even when running via `venv/bin/python -m uv`

---

_@altendky_

Seems likely related to https://github.com/astral-sh/uv/issues/1374.  I expect that even if usage outside of a venv is blocked, either universally by uv or because of `PIP_REQUIRE_VIRTUALENV=1`, activation should not be required as many people prefer to `venv/bin/python` instead of using `source venv/bin/activate; python`.

```console
$ python3.11 -m venv venv
```

```console
$ venv/bin/python -m pip install 'uv @ git+https://github.com/astral-sh/uv@1ed6ba0ba0d3756312085ec21c90469985b5bbb6'
Collecting uv@ git+https://github.com/astral-sh/uv@1ed6ba0ba0d3756312085ec21c90469985b5bbb6
  Using cached uv-0.1.2-py3-none-linux_x86_64.whl
Installing collected packages: uv
Successfully installed uv-0.1.2

[notice] A new release of pip is available: 23.2.1 -> 24.0
[notice] To update, run: /home/altendky/tmp/venv/bin/python -m pip install --upgrade pip
```

```console
$ venv/bin/python -m uv pip install --force-reinstall attrs
error: Failed to locate a virtualenv or Conda environment (checked: `VIRTUAL_ENV`, `CONDA_PREFIX`, and `.venv`). Run `uv venv` to create a virtualenv.
```

Above should work just as below does.

```console
$ venv/bin/python -m pip install --force-reinstall attrs
Collecting attrs
  Obtaining dependency information for attrs from https://files.pythonhosted.org/packages/e0/44/827b2a91a5816512fcaf3cc4ebc465ccd5d598c45cefa6703fcf4a79018f/attrs-23.2.0-py3-none-any.whl.metadata
  Downloading attrs-23.2.0-py3-none-any.whl.metadata (9.5 kB)
Downloading attrs-23.2.0-py3-none-any.whl (60 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 60.8/60.8 kB 844.6 kB/s eta 0:00:00
Installing collected packages: attrs
Successfully installed attrs-23.2.0

[notice] A new release of pip is available: 23.2.1 -> 24.0
[notice] To update, run: /home/altendky/tmp/venv/bin/python -m pip install --upgrade pip
```

```console
$ source venv/bin/activate
```

Just showing that below the same command works after the activation above.

```console
(venv) $ venv/bin/python -m uv pip install --force-reinstall attrs
Resolved 1 package in 1ms
Installed 1 package in 2ms
 - attrs==23.2.0
 + attrs==23.2.0
```

---

_Assigned to @zanieb by @zanieb on 2024-02-16 15:27_

---

_Label `bug` added by @zanieb on 2024-02-16 15:27_

---

_Comment by @zanieb on 2024-02-16 15:29_

Thanks for the well written issue!

Related https://github.com/astral-sh/uv/issues/1396

---

_Closed by @zanieb on 2024-02-16 18:55_

---
