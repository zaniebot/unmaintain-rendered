---
number: 10669
title: "uv sync x dependency groups doesn't work with PyPy"
type: issue
state: closed
author: hynek
labels:
  - bug
assignees: []
created_at: 2025-01-16T08:44:43Z
updated_at: 2025-01-20T07:24:26Z
url: https://github.com/astral-sh/uv/issues/10669
synced_at: 2026-01-07T13:12:18-06:00
---

# uv sync x dependency groups doesn't work with PyPy

---

_Issue opened by @hynek on 2025-01-16 08:44_

```console
$ uv --version
uv 0.5.20 (1c17662b3 2025-01-15)
```

I've run into this in my experimental attempt to use fully-locked env for attrs https://github.com/python-attrs/attrs/pull/1349 so you can just check out the https://github.com/python-attrs/attrs/tree/uv-lock branch for testing.

Put simply, with PyPy nothing except the current package gets installed:


```console
$ rm -rf .venv && uv sync --python pypy3.10 && uv pip list
Using PyPy 3.10.14 interpreter at: /opt/homebrew/bin/pypy3.10
Creating virtual environment at: .venv
Resolved 99 packages in 1ms
Installed 1 package in 1ms
 + attrs==24.3.1.dev20 (from file:///Users/hynek/FOSS/attrs)
Package Version      Editable project location
------- ------------ -------------------------
attrs   24.3.1.dev20 /Users/hynek/FOSS/attrs
```

and:

```console
$ rm -rf .venv && uv sync --python python3.13 && uv pip list
Using CPython 3.13.1 interpreter at: /Library/Frameworks/Python.framework/Versions/3.13/bin/python3.13
Creating virtual environment at: .venv
Resolved 99 packages in 1ms
Installed 23 packages in 28ms
 + attrs==24.3.1.dev20 (from file:///Users/hynek/FOSS/attrs)
 + cloudpickle==3.1.1
 + decorator==5.1.1
 + hypothesis==6.124.0
 + iniconfig==2.0.0
 + jinja2==3.1.5
 + jsonschema==4.23.0
 + jsonschema-specifications==2024.10.1
 + markupsafe==3.0.2
 + mypy==1.14.1
 + mypy-extensions==1.0.0
 + packaging==24.2
 + pluggy==1.5.0
 + pympler==1.1
 + pytest==8.3.4
 + pytest-mypy-plugins==3.2.0
 + pyyaml==6.0.2
 + referencing==0.35.1
 + regex==2024.11.6
 + rpds-py==0.22.3
 + sortedcontainers==2.4.0
 + tomlkit==0.13.2
 + typing-extensions==4.12.2
Package                   Version      Editable project location
------------------------- ------------ -------------------------
attrs                     24.3.1.dev20 /Users/hynek/FOSS/attrs
cloudpickle               3.1.1
decorator                 5.1.1
hypothesis                6.124.0
iniconfig                 2.0.0
jinja2                    3.1.5
jsonschema                4.23.0
jsonschema-specifications 2024.10.1
markupsafe                3.0.2
mypy                      1.14.1
mypy-extensions           1.0.0
packaging                 24.2
pluggy                    1.5.0
pympler                   1.1
pytest                    8.3.4
pytest-mypy-plugins       3.2.0
pyyaml                    6.0.2
referencing               0.35.1
regex                     2024.11.6
rpds-py                   0.22.3
sortedcontainers          2.4.0
tomlkit                   0.13.2
typing-extensions         4.12.2
```

---


The current approach of using `uv pip install --extra dev` as on the main branch works.

I've tried on the extra-based `main` to set min Python to 3.9 and running the following:

```console
$ rm -rf .venv && uv sync --python pypy3.10 --extra dev && uv pip list
Using PyPy 3.10.14 interpreter at: /opt/homebrew/bin/pypy3.10
Creating virtual environment at: .venv
Resolved 84 packages in 2ms
   Built psutil==6.1.1
   Built pyyaml==6.0.2
Prepared 13 packages in 4.93s
Installed 24 packages in 17ms
 + attrs==24.3.1.dev3 (from file:///Users/hynek/FOSS/attrs)
 + cfgv==3.4.0
 + distlib==0.3.9
 + exceptiongroup==1.2.2
 + execnet==2.1.1
 + filelock==3.16.1
 + hypothesis==6.124.0
 + identify==2.6.5
 + iniconfig==2.0.0
 + nodeenv==1.9.1
 + packaging==24.2
 + platformdirs==4.3.6
 + pluggy==1.5.0
 + pre-commit==4.0.1
 + pre-commit-uv==4.1.4
 + psutil==6.1.1
 + pympler==1.1
 + pytest==8.3.4
 + pytest-xdist==3.6.1
 + pyyaml==6.0.2
 + sortedcontainers==2.4.0
 + tomli==2.2.1
 + uv==0.5.20
 + virtualenv==20.29.0
Package          Version     Editable project location
---------------- ----------- -------------------------
attrs            24.3.1.dev3 /Users/hynek/FOSS/attrs
cfgv             3.4.0
distlib          0.3.9
exceptiongroup   1.2.2
execnet          2.1.1
filelock         3.16.1
hypothesis       6.124.0
identify         2.6.5
iniconfig        2.0.0
nodeenv          1.9.1
packaging        24.2
platformdirs     4.3.6
pluggy           1.5.0
pre-commit       4.0.1
pre-commit-uv    4.1.4
psutil           6.1.1
pympler          1.1
pytest           8.3.4
pytest-xdist     3.6.1
pyyaml           6.0.2
sortedcontainers 2.4.0
tomli            2.2.1
uv               0.5.20
virtualenv       20.29.0
```

So it seems to be connected to PEP 735 / dependency groups??

---

_Label `bug` added by @konstin on 2025-01-16 13:44_

---

_Referenced in [astral-sh/uv#10682](../../astral-sh/uv/pulls/10682.md) on 2025-01-16 14:43_

---

_Comment by @konstin on 2025-01-16 14:45_

This bug is two bugs on uv's side: We need to discard lockfiles with invalid fork markers (#10682 with the minimal reproduction), but we must also figure out why we generate a resolution with these fork markers in the first place (TBD):

```toml
resolution-markers = [
    "python_full_version >= '3.10' and platform_python_implementation == 'CPython'",
    "python_full_version == '3.9.*'",
    "python_full_version < '3.9'",
]
```

---

_Comment by @charliermarsh on 2025-01-17 01:32_

Thanks Hynek, we're looking into it...

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-17 01:40_

---

_Comment by @charliermarsh on 2025-01-17 01:40_

This appears to be a bug in the "fork by `requires-python`" logic. We must be dropping a fork there accidentally?

---

_Comment by @charliermarsh on 2025-01-17 02:06_

When solving `python_full_version < '3.10' or platform_python_implementation != 'CPython'`, we fork on `requires-python`, and produce two branches:

- `python_full_version == '3.8.*'`
- `python_full_version == '3.9.*'`

So we end up losing the `platform_python_implementation != 'CPython'` portion. I'm not quite sure how to solve this. I naively tried to add a "remainder" fork, but it pushed the resolver into an infinite loop.


---

_Comment by @charliermarsh on 2025-01-17 02:41_

@hynek -- In the meantime, you can avoid this by disabling the `requires-python` optimizations:

```toml
[tool.uv]
fork-strategy = "fewest"
```

---

_Comment by @charliermarsh on 2025-01-17 03:19_

As a smaller example, given this `requirements.in`:

```
iniconfig ; platform_python_implementation == "CPython" and python_version >= "3.10"
coverage
```

Then `uv pip compile requirements.in --python-version 3.8 --universal` produces:

```
coverage==7.6.1 ; python_full_version < '3.9'
coverage==7.6.10 ; (python_full_version == '3.9.*' and platform_python_implementation != 'CPython') or (python_full_version >= '3.9' and platform_python_implementation == 'CPython')
iniconfig==2.0.0 ; python_full_version >= '3.10' and platform_python_implementation == 'CPython'
```

Which omits `coverage` on Python 3.10 and later for non-CPython.


---

_Referenced in [astral-sh/uv#10704](../../astral-sh/uv/pulls/10704.md) on 2025-01-17 04:33_

---

_Closed by @charliermarsh on 2025-01-17 16:25_

---

_Closed by @charliermarsh on 2025-01-17 16:25_

---

_Comment by @charliermarsh on 2025-01-18 18:43_

Should be fixed in v0.5.21!

---

_Comment by @hynek on 2025-01-20 07:24_

yay, https://github.com/python-attrs/attrs/pull/1349 is green, thanks!

---
