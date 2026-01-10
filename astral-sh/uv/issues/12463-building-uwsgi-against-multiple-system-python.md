---
number: 12463
title: Building uwsgi against multiple system Python versions corrupts it
type: issue
state: open
author: andersk
labels:
  - bug
assignees: []
created_at: 2025-03-25T12:28:20Z
updated_at: 2025-03-25T13:23:39Z
url: https://github.com/astral-sh/uv/issues/12463
synced_at: 2026-01-10T01:25:20Z
---

# Building uwsgi against multiple system Python versions corrupts it

---

_Issue opened by @andersk on 2025-03-25 12:28_

### Summary

When I use `uv` to build `uwsgi` against both Python 3.10 from Ubuntu 22.04 and Python 3.12 from Ubuntu 24.04, the binary from the latter build crashes at runtime with a mysterious error:

```console
$ docker run --rm -it ubuntu:22.04
# sed -i 'p; s/jammy/noble/g' /etc/apt/sources.list
# apt-get update
# apt-get -y install curl gcc python3.10-dev python3.12-dev
# curl -LsSf https://astral.sh/uv/install.sh | sh
# source $HOME/.local/bin/env
# echo 'def application(env, start_response): start_response("200", []); return []' > myapp.py
# uvx -p3.10 uwsgi --version
      Built uwsgi==2.0.28
Installed 1 package in 0.82ms
2.0.28
# uvx -p3.12 uwsgi --wsgi-file myapp.py --http :8080
      Built uwsgi==2.0.28
Installed 1 package in 0.96ms
*** Starting uWSGI 2.0.28 (64bit) on [Tue Mar 25 05:17:52 2025] ***
…
Fatal Python error: PyImport_AppendInittab: PyImport_AppendInittab() may not be called after Py_Initialize()
Python runtime state: initialized

Current thread 0x00007f08c39e8740 (most recent call first):
  <no Python frame>
```

It seems that `uv` has built both wheels out of the same directory without properly cleaning it, leading to an issue similar to unbit/uwsgi#2579. The error disappears if I skip the first build, or if I `uv cache clean` between the two builds.

### Platform

Ubuntu 22.04 amd64

### Version

uv 0.6.9

### Python version

Python 3.10.12, 3.12.3

---

_Label `bug` added by @andersk on 2025-03-25 12:28_

---

_Referenced in [zulip/zulip#34156](../../zulip/zulip/pulls/34156.md) on 2025-03-25 12:39_

---

_Comment by @charliermarsh on 2025-03-25 13:20_

Is this the same as https://github.com/astral-sh/uv/issues/10575?

---

_Comment by @andersk on 2025-03-25 13:22_

Not sure, that one doesn’t seem to involve multiple Python versions.

---

_Referenced in [vexxhost/atmosphere#2773](../../vexxhost/atmosphere/pulls/2773.md) on 2025-06-20 00:26_

---
