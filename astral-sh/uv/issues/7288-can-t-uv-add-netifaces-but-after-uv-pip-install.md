```yaml
number: 7288
title: "Can't uv add netifaces (but after uv pip install netifaces it works)"
type: issue
state: closed
author: romaia
labels:
  - needs-mre
assignees: []
created_at: 2024-09-11T13:10:31Z
updated_at: 2024-12-27T14:42:16Z
url: https://github.com/astral-sh/uv/issues/7288
synced_at: 2026-01-12T15:59:12Z
```

# Can't uv add netifaces (but after uv pip install netifaces it works)

---

_@romaia_


Using uv version 0.4.9 on linux, I cant `uv add netifaces`, but if I install it with `uv pip install netifaces` before hand, it works.

Steps to reproduce. On a new project:

```
uv init
uv add netifaces
```

results in:

```
Using Python 3.10.14
Creating virtualenv at: .venv
Resolved 2 packages in 1ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: netifaces==0.11.0
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
running build
running build_ext
checking for getifaddrs...not found. (cached)
checking for getnameinfo...not found. (cached)
checking for socket IOCTLs...not found. (cached)
checking for optional header files...none found. (cached)
checking whether struct sockaddr has a length field...no. (cached)
checking which sockaddr_xxx structs are defined...none! (cached)
checking for routing socket support...no. (cached)
checking for sysctl(CTL_NET...) support...no. (cached)
checking for netlink support...no. (cached)
building 'netifaces' extension
clang -pthread -Wno-unused-result -Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -fPIC -fPIC -DNETIFACES_VERSION=0.11.0 -I/home/romaia/.cache/uv/builds-v0/.tmpbYhrAX/include -I/home/romaia/.local/share/uv/python/cpython-3.10.14-linux-x86_64-gnu/include/python3.10 -c netifaces.c -o build/temp.linux-x86_64-cpython-310/netifaces.o
--- stderr:
netifaces.c:210:6: error: You need to add code for your platform.
  210 | #    error You need to add code for your platform.
      |      ^
netifaces.c:1469:22: warning: unused variable 'defaults' [-Wunused-variable]
 1469 |   PyObject *result, *defaults;
      |                      ^~~~~~~~
1 warning and 1 error generated.
error: command '/usr/bin/clang' failed with exit code 1
```

but if I run:

```
uv pip install netifaces
Resolved 1 package in 1ms
Installed 1 package in 0.61ms
 + netifaces==0.11.0
```

It install successfully, and after that, uv add works:

```
uv add netifaces
Resolved 2 packages in 2ms
Audited 1 package in 0.01ms
```

I am not sure if its a problem with netifaces itself, but since uv pip works, seems like a issue with uv add

---

_Comment by @charliermarsh on 2024-09-11 13:16_

For whatever reason, that `uv pip install netifaces` is not building the package. It must've already been built and cached by the time you ran that command (otherwise, we'd see `Prepared 1 package in...`). So I suspect something else is going on here...?

---

_Label `needs-mre` added by @charliermarsh on 2024-09-11 13:16_

---

_Label `virtualenv` added by @charliermarsh on 2024-09-11 13:16_

---

_Label `virtualenv` removed by @charliermarsh on 2024-09-11 13:16_

---

_Comment by @romaia on 2024-09-11 17:48_

Indeed, testing on 3 fresh vms (ubuntu 24/22/20), the problem does not happen, so I guess something in my current setup is kinda of broken.

Will try to dig deeper to find out what.

---

_Comment by @bluss on 2024-09-11 20:23_

netifaces has wheels for python 3.8 (all platforms?) and 3.9 (not all platforms)? And no pre-built wheels available for newer pythons (>=3.10). So python version will be important for what happens there. You can use a strength of uv - easy to switch python version - configure python 3.8 for the project and that should work well for netifaces - judging from the wheel list on PyPI (See the download files view).

---

_Closed by @charliermarsh on 2024-12-27 14:42_

---
