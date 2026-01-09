---
number: 4828
title: "FR: (`uv venv`) add support for '-p 3.13t' as a shorthand for '-p 3.13*t' ?"
type: issue
state: closed
author: neutrinoceros
labels:
  - enhancement
  - uv python
assignees: []
created_at: 2024-07-05T08:35:12Z
updated_at: 2024-09-23T22:36:17Z
url: https://github.com/astral-sh/uv/issues/4828
synced_at: 2026-01-07T13:12:17-06:00
---

# FR: (`uv venv`) add support for '-p 3.13t' as a shorthand for '-p 3.13*t' ?

---

_Issue opened by @neutrinoceros on 2024-07-05 08:35_

pyenv recently introduced free-threading variants for Python 3.13:
https://github.com/pyenv/pyenv/pull/2995
https://github.com/pyenv/pyenv/pull/3001

Assuming I installed Python `3.13.0b3` (both variants) as:
```shell
pyenv install 3.13.0b3 3.13t-dev
pyenv local 3.13.0b3 3.13t-dev
```

and as of pyenv `2.4.4` + uv `0.2.21`, the following invocations work:
```shell
$ python3.13 --version
Python 3.13.0b3

$ python3.13t --version
Python 3.13.0b3+

$ uv venv -p 3.13
Using Python 3.13.0b3 interpreter at: /Users/clm/.pyenv/versions/3.13.0b3/bin/python3.13
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

$ uv venv -p python3.13
Using Python 3.13.0b3 interpreter at: /Users/clm/.pyenv/versions/3.13.0b3/bin/python3.13
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

$ uv venv -p python3.13t
Using Python 3.13.0b3 interpreter at: /Users/clm/.pyenv/versions/3.13t-dev/bin/python3.13t
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```

but this one doesn't:
```shell
$ uv venv -p 3.13t
  × No interpreter found for executable name `3.13t` in system path
```

This is still fresh paint and not a well established naming convention yet, but since it's pushed in pyenv by [PEP 703](https://peps.python.org/pep-0703/) author Sam Gross, maybe it's reasonable to support it already ?
It certainly wasn't obvious to me that I could use `uv venv -p python3.13t`, and I discovered it by trial and error.

---

_Comment by @zanieb on 2024-07-05 15:42_

Related https://github.com/astral-sh/uv/issues/4400

Agree we should add support for requesting free-threaded builds via shorthand.

---

_Label `enhancement` added by @zanieb on 2024-07-05 15:42_

---

_Label `uv python` added by @zanieb on 2024-08-27 16:02_

---

_Referenced in [astral-sh/uv#7193](../../astral-sh/uv/issues/7193.md) on 2024-09-08 17:31_

---

_Referenced in [astral-sh/python-build-standalone#320](../../astral-sh/python-build-standalone/issues/320.md) on 2024-09-08 17:31_

---

_Assigned to @zanieb by @zanieb on 2024-09-08 17:34_

---

_Referenced in [astral-sh/uv#7431](../../astral-sh/uv/pulls/7431.md) on 2024-09-16 15:51_

---

_Closed by @zanieb on 2024-09-23 22:36_

---

_Closed by @zanieb on 2024-09-23 22:36_

---
