---
number: 8480
title: uv cannot find its own free-threaded Python when installing tools
type: issue
state: closed
author: layday
labels:
  - question
assignees: []
created_at: 2024-10-22T21:31:55Z
updated_at: 2024-10-24T14:08:52Z
url: https://github.com/astral-sh/uv/issues/8480
synced_at: 2026-01-07T13:12:17-06:00
---

# uv cannot find its own free-threaded Python when installing tools

---

_Issue opened by @layday on 2024-10-22 21:31_

```
$ uv python install 3.13t
Searching for Python versions matching: Python 3.13t
Installed Python 3.13.0 in 4.20s
 + cpython-3.13.0+freethreaded-macos-aarch64-none
$ UV_PYTHON_DOWNLOADS=manual UV_PYTHON_PREFERENCE=only-managed uv tool install nox 
error: No interpreter found in managed installations
$ uv --version           
uv 0.4.25 (97eb6ab4a 2024-10-21)
$ uname   
Darwin
```


---

_Comment by @charliermarsh on 2024-10-22 21:34_

I guess it's not allowing the free-threaded build unless requested, or something like that?

---

_Comment by @zanieb on 2024-10-22 21:35_

Yeah we don't allow the free-threaded distribution unless you request it, even if it's the only one, due to its experimental nature.

---

_Label `question` added by @zanieb on 2024-10-22 21:35_

---

_Closed by @zanieb on 2024-10-24 12:32_

---

_Comment by @layday on 2024-10-24 14:08_

Thanks for the explanation.

---

_Referenced in [pyca/cryptography#12555](../../pyca/cryptography/pulls/12555.md) on 2025-09-09 00:43_

---

_Referenced in [astral-sh/uv#15756](../../astral-sh/uv/issues/15756.md) on 2025-09-09 15:37_

---
