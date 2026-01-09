---
number: 15577
title: "\"uv pip install *** --compile-bytecode\" fails with obscure message"
type: issue
state: closed
author: mrmathematica
labels:
  - bug
assignees: []
created_at: 2025-08-28T23:20:07Z
updated_at: 2025-08-31T14:53:01Z
url: https://github.com/astral-sh/uv/issues/15577
synced_at: 2026-01-07T13:12:19-06:00
---

# "uv pip install *** --compile-bytecode" fails with obscure message

---

_Issue opened by @mrmathematica on 2025-08-28 23:20_

### Summary

1. We run python and uv in containners. Python comes the following site.getsitepackages(): '/usr/local/lib64/python3.12/site-packages', '/usr/local/lib/python3.12/site-packages', '/usr/lib64/python3.12/site-packages', '/usr/lib/python3.12/site-packages'.
2. '/usr/local/lib64/python3.12/site-packages' folder doesn't exist by default (This is after we run `dnf install python3.12` on ubi8 base image). If calling "uv pip install" with some binary package, it will be created and the binary package will be installed into there.
3. In this case we want to call "uv pip install some-pure-python-package --bytecode-compile". I can confirm, if calling "uv pip install some-pure-python-package", the pacakge will be installed into '/usr/local/lib/python3.12/site-packages', and '/usr/local/lib64/python3.12/site-packages' won't be created.
4. calling "uv pip install some-pure-python-package --bytecode-compile" fails with obscure message:

error: Failed to bytecode-compile Python file in: /usr/local/lib64/python3.12/site-packages
Caused by: Failed to start Python interpreter to run compile script

It took me more then half a day, to finally figure out the error is because '/usr/local/lib64/python3.12/site-packages' doesn't exist, and the packages are being installed into '/usr/local/lib/python3.12/site-packages'. In this case, I think uv should gracefully taking care the situation, either to create an empty '/usr/local/lib/python3.12/site-packages' folder, or not fail anyway.


### Platform

Redhat ubi8 base image

### Version

0.8.13

### Python version

Python 3.12.11

---

_Label `bug` added by @mrmathematica on 2025-08-28 23:20_

---

_Comment by @charliermarsh on 2025-08-28 23:59_

Are you able to provide a minimal reproduction, e.g. with a Dockerfile?

---

_Label `needs-mre` added by @charliermarsh on 2025-08-28 23:59_

---

_Comment by @mrmathematica on 2025-08-29 01:21_

Yes, this is my Dockerfile

```
FROM registry.access.redhat.com/ubi8/ubi:latest

# Install python
RUN dnf install -y python3.12 python3.12-devel curl ca-certificates \
    && dnf clean all

# Install uv and link it to /usr/local/bin
RUN curl -LsSf https://astral.sh/uv/install.sh | sh \
    && ln -s /root/.local/bin/uv /usr/local/bin/uv

# Install 'six' using uv and compile bytecode
RUN uv pip install --system six --compile-bytecode

CMD ["/bin/bash"]
```


---

_Label `needs-mre` removed by @charliermarsh on 2025-08-31 14:33_

---

_Comment by @charliermarsh on 2025-08-31 14:33_

Thanks! That reproduces for me.

---

_Referenced in [astral-sh/uv#15608](../../astral-sh/uv/pulls/15608.md) on 2025-08-31 14:40_

---

_Closed by @charliermarsh on 2025-08-31 14:53_

---
